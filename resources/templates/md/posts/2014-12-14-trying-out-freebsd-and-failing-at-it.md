{:title "trying out freebsd and failing at it"
:layout :post
 :tags ["old-post"]}



Wait, what? Why?! No reason, really. I saw the new FreeBSD book[^1] lying around in someone's office, and separately I was reminded of the week I spent getting Gentoo up and running a decade ago, and decided I missed all that and wanted to repeat a similar experience.



So, this is an attempt to both (1) install it within a VM on the Google Compute Engine[^2], and (2) slowly learn more about it. Here follows a log of everything I did, based on the original instructions from the mailing list [^3]. (_Meta-note: if running remotely, be sensible; use `tmux` or `screen`_)



### _Step 1_: Install the emulator

```sh

$ sudo apt-get install qemu

```



### _Step 2_: Get the FreeBSD version to install -- in my case, I picked the "disc1" version corresponding to the "RELEASE" image[^4]

```sh

$ wget ftp://ftp.freebsd.org/pub/FreeBSD/releases/amd64/amd64/ISO-IMAGES/10.1/FreeBSD-10.1-RELEASE-amd64-disc1.iso

```



### _Step 3_: Create the disk image for the emulator

```sh

$ qemu-img create disk.raw 100g

Formatting 'disk.raw', fmt=raw size=107374182400 

```



### _Step 4_: Boot from the ISO downloaded earlier



Notes:

- The original instructions also mention `--enable_kvm`, but that didn't work for me. I tried `sudo modprobe kvm-intel`, but `dmesg | grep kvm` informed me that it had been disabled by the Bios, so I skipped it here.

- If you are running this remotely (like I was), you'll have to add `--curses` to the `qemu` command line.



### _Step 5_: Install FreeBSD

At this point, if all went well, the emulator should boot up and you should see the "Welcome" screen, where you can hit "Install" and begin.



Notes:

- Pretty standard stuff, if you've done a linux install before.

- When it came to the disk setup I chose `ufs` for no particular reason[^5], but you can choose any of the options; it'll show you the suggested table for the "entire disk" as split between `freebsd-boot`, `freebsd-ufs` and `freebsd-swap`, just hit _"Finish"_ and then _"Commit"_ to proceed.

- Take a nice long break when the _"Archive Extraction"_ step begins; this is computationally intensive, and (especially without `kvm` enabled) takes a _long_ time on the emulator.

- DNS settings are `google.internal.` for the _"search"_, and `169.254.169.254` for the DNS IP.

- Include `sshd` and `ntpd` in the list of services to start at boot



### _Step 6_: Further configuration (as root)

Once you hit _"Exit"_ at the end, choose the option to drop into a shell and then run the following:



```sh

echo 'console="comconsole"' >>/etc/rc.conf

echo ‘console=”comconsole”’ > /boot/loader.conf

sed -I -e “/hostname/d” /etc/rc.conf

echo -Dh >/boot.config



cat <<EOF >/etc/dhclient.conf

interface "vtnet0" {

  supersede subnet-mask 255.255.0.0;

}

EOF



sed -I -e “/server/d” /etc/ntp.conf

echo “server 169.254.169.254” >> /etc/ntp.conf



echo “169.254.169.254 metadata.google.internal metadata” >> /etc/hosts

```



### _Step 7_: Add yourself as a user

- Run `adduser` and follow the prompts[^6]

- Add yourself to the `wheel` group. E.g. in my case:

```sh

$ pw user mod agam -G wheel

```

- Allow yourself to login via `ssh`:

```sh

$ sed -I -e “s/#PasswordAuthentication no/PasswordAuthentication yes/” /etc/ssh/sshd_config

```



### _Step 8_: Setup GCE (within the image)

- Enable either OpenDNS or Google Public DNS . E.g. for the latter:

```sh

$ echo “nameserver 8.8.8.8” >> /etc/resolv.conf

```

- Install `sudo`, `python`[^7], and `wget`[^8]

- Get `gcloud`[^9][^10]

- Remove the DNS record added earlier:

```sh

$ sed -I -e “/8.8.8.8/d” /etc/resolv.conf

```

- Turn off FreeBSD (run `poweroff`)



### _Step 9_: Setup GCE (on your workstation)

- Install `gcloud` (as before)

- Authenticate: Run `gcloud auth login` and follow the url to enter the code

- Prepare the image for upload: `tar -Szcf freebsd.tar.gz disk.raw`

- Create a bucket[^11] and upload[^12] the image there (Note: I was shocked, _shocked_, by how _fast_ this upload went!)

- Prepare the image for use in your VM and insert it

```sh

$ gcutil addimage freebsd gs://<bucket>/<object>

$ gcutil --project <project_id> freebsd gs://<bucket>/<object>

```



- Add a VM and SSH to it (both these operations can be done either through the "Google Cloud Console" or the command-line client[^13]). E.g. in my case the latter is

`> gcloud compute ssh myvm`[^14]



### _Step 10_: Nope

Yeah this didn't work for me. Luckily, the "serial console" can be viewed through the dashboard, and what I saw was a repeated failure to boot.



```

Mounting local file systems:.

Writing entropy file:.

vtnet0: link state changed to UP

Starting Network: lo0 vtnet0.

lo0: flags=8049<UP,LOOPBACK,RUNNING,MULTICAST> metric 0 mtu 16384

     options=600003<RXCSUM,TXCSUM,RXCSUM_IPV6,TXCSUM_IPV6>

     inet6 ::1 prefixlen 128 

     inet6 fe80::1%lo0 prefixlen 64 scopeid 0x2 

     inet 127.0.0.1 netmask 0xff000000 

     nd6 options=21<PERFORMNUD,AUTO_LINKLOCAL>

vtnet0: flags=8943<UP,BROADCAST,RUNNING,PROMISC,SIMPLEX,MULTICAST> metric 0 mtu 1500

	options=6c01bb<RXCSUM,TXCSUM,VLAN_MTU,VLAN_HWTAGGING,JUMBO_MTU,VLAN_HWCSUM,TSO4,VLAN_HWTSO,LINKSTATE,RXCSUM_IPV6,TXCSUM_IPV6>

	ether 42:01:0a:f0:c8:6c

	nd6 options=29<PERFORMNUD,IFDISABLED,AUTO_LINKLOCAL>

	media: Ethernet 10Gbase-T <full-duplex>

	status: active

Starting devd.

Starting dhclient.

DHCPREQUEST on vtnet0 to 255.255.255.255 port 67

DHCPNAK from 169.254.169.254

DHCPDISCOVER on vtnet0 to 255.255.255.255 port 67 interval 8

DHCPOFFER from 169.254.169.254

DHCPREQUEST on vtnet0 to 255.255.255.255 port 67

DHCPACK from 169.254.169.254

panic: ffs_write: type 0xfffff80008a1cb10 0 (0,150)

cpuid = 2

KDB: stack backtrace:

#0 0xffffffff80963000 at kdb_backtrace+0x60

#1 0xffffffff80928125 at panic+0x155

#2 0xffffffff80b7f825 at ffs_write+0x5b5

#3 0xffffffff80e428f5 at VOP_WRITE_APV+0x145

#4 0xffffffff809d96f9 at vn_write+0x259

#5 0xffffffff809d598b at vn_io_fault+0x10b

#6 0xffffffff8097a437 at dofilewrite+0x87

#7 0xffffffff8097a168 at kern_writev+0x68

#8 0xffffffff8097a0f3 at sys_write+0x63

#9 0xffffffff80d25851 at amd64_syscall+0x351

#10 0xffffffff80d0aa6b at Xfast_syscall+0xfb

Uptime: 7s

```



Looks like this may be a corrupt disk? Not sure. But pretty bummed :(



_**Update**_: I see people having luck with either (1) building a "rescue disk" on a running FreeBSD machine (not an option for me), or (2) using the `mfsbsd`[^15] remote install. I think I'll try the latter when I get time.



_**Update**_: Here's how that went :-



- Get the `mfsbsd` image[^16]



```sh

$curl -o disk.raw http://mfsbsd.vx.sk/files/iso/10/amd64/mfsbsd-se-10.1-RELEASE-amd64.iso`

```



- Tar it, upload it



```sh

$ tar -Szcf mfs-freebsd.tar.gz disk.raw

$ gsutil cp mfs-freebsd.tar.gz gs://<bucket_name>

$ gcutil addimage mfs-freebsd gs://<bucket_name>/mfs-freebsd.tar.gz

```



- Create a VM with this instance



Aaand ... NOPE again; fails to even show the serial console this time :(

Error: `The resource 'projects/algol-c/zones/us-central1-a/instances/myvm' is not ready`



- Deleted the VM and created it again[^17]



Failed again, but this time I grabbed the output before it vanished:



```

Unable to lock ram - bridge not found

Changing serial settings was 3/2 now 3/0

enter handle_19:

  NULL

Booting from Hard Disk...

Boot failed: not a bootable disk



enter handle_18:

  NULL

Booting from Floppy...

Boot failed: could not read the boot disk



enter handle_18:

  NULL

No bootable device.  Powering off VM.

END OF LINE

  Retrying in 60 seconds.

```



- _Of course!_ I have to _make_ a bootable image using this iso (!) But, But, But .... that needs a running FreeBSD system?!



_**Update**_: Damn, looks like GCE is behind both Amazon and (wtf!) Microsoft on this. From the release notes for v10.1:



> FreeBSD 10.1-RELEASE is also available on these cloud hosting platforms:

>

> - Amazon® EC2™ FreeBSD/amd64

> - Microsoft® Azure™ FreeBSD/amd64,  FreeBSD/i386



_**Conclusion**_: The `qemu` path should have worked, I don't yet understand why not. Another option might be to try the _vhd_ image and get that to work. Or try EC2/Azure. Or wait for someone to figure this out and publicly share a working image. Or give up on FreeBSD and get back "to real work" :)



[^1]: The [newer edition](http://www.amazon.com/gp/product/0321968972/ref=pd_lpo_sbs_dp_ss_1?pf_rd_p=1944687562&pf_rd_s=lpo-top-stripe-1&pf_rd_t=201&pf_rd_i=0201702452&pf_rd_m=ATVPDKIKX0DER&pf_rd_r=0QBY4BRZHVN8ZNNFWDMW) of "The Design and Implementation of the FreeBSD Operating System"

[^2]: Yes, I've heard [the Amazon experience](http://www.daemonology.net/freebsd-on-ec2/) is easier, but that would be ... uh ... slightly disloyal right now :P

[^3]: https://groups.google.com/forum/#!msg/gce-discussion/YWoa3Aa_49U/FYAg9oiRlLUJ

[^4]: From [this list](ftp://ftp.freebsd.org/pub/FreeBSD/releases/amd64/amd64/ISO-IMAGES/10.1/)

[^5]: Don't necessarily care too much about the ["Cult of ZFS"](http://www.smbitjournal.com/2014/05/the-cult-of-zfs/); besides, UFS is faster.

[^6]: Shell choices are `sh`/`csh`/`tcsh`, with the first being the default option. Take your pick; I'm going to replace `sh` with `bash` later anyway.

[^7]: If you try to guess and install Python 3, you'll see this error: `ERROR: Python 3 is not supported by the Google Cloud SDK.  Please use a Python 2.x version that is 2.6 or greater` (Do as it says! (Also, you might not want to put bets on python 3?))

[^8]: Either run `pkg_add -r` for each tool, or compile it from source with ports. For the latter, first run `portsnap fetch`, `portsnap extract`, and `portsnap update`, in that order (this is a one-time setup), then `cd` to the appropriate directory under `/usr/ports` and run `make install` for each tool (**warning**: this takes a long, long, _long_ time! If you're running the emulator without `kvm` support, you have to be _extremely masochistic_ to try compiling from source).

[^9]: Original instructions refer to `gsutil` and `gcutil` separately, but they were (you guessed it!) _deprecated_.

[^10]: More information [here](https://cloud.google.com/compute/docs/gcloud-compute/)

[^11]: `gsutil -p <project_id> -c gs://<bucket_name>`

[^12]: `gsutil cp <gzipped_file> gs://<bucket_name>`

[^13]: https://cloud.google.com/sdk/gcloud/reference/compute/instances/create

[^14]: You also need a `--project` flag, but you can set a global value for this by running `gcloud config set project <project_id>`

[^15]: https://www.freebsd.org/doc/en_US.ISO8859-1/articles/remote-install/preparation.html

[^16]: In case you were wondering, the file name inside the uploaded image _has_ to be `disk.raw`

[^17]: "Terminated" VMs have to be dealt with this way, AFAICS
