{:title "io performance is that really an ssd"
:layout :post
 :tags ["old-post"]}



I created a new VM in Google Compute Engine a few days ago, and assigned it an SSD disk, and then began wondering _if it was really an SSD disk_.



(_Digression_: I never appreciated the difference an SSD makes until the first time a Macbook Air awoke from sleep _instantly_. From that moment onwards, the time taken for my desktop at home to show its login screen has _always_ felt irritatingly long)



So, this is a crude benchmark of a "regular" hard drive on my workstation:



```sh

$ sudo hdparm -tT /dev/sda1                                                                                                                                   



/dev/sda1:

 Timing cached reads:   20772 MB in  2.00 seconds = 10403.36 MB/sec

 Timing buffered disk reads: 242 MB in  1.34 seconds = 180.53 MB/sec

```



By the way, the "fresh" disk division for a new VM looks something like this:



```sh

$ df -h

Filesystem                                              Size  Used Avail Use% Mounted on

rootfs                                                  9.9G  734M  8.7G   8% /

udev                                                     10M     0   10M   0% /dev

tmpfs                                                   3.0G  104K  3.0G   1% /run

/dev/disk/by-uuid/foo  9.9G  734M  8.7G   8% /

tmpfs                                                   5.0M     0  5.0M   0% /run/lock

tmpfs                                                   6.0G     0  6.0G   0% /run/shm

```



`foo` above is really a long uuid, which I subsituted to avoid repeating. This is the output from `hdparm`, run within my VM:



```sh

agam@myvm:~$ sudo hdparm -tT /dev/disk/by-uuid/foo

/dev/disk/by-uuid/foo:

 Timing cached reads:   16996 MB in  2.00 seconds = 8506.48 MB/sec

 Timing buffered disk reads: 310 MB in  3.02 seconds = 102.65 MB/sec

```



So that's ... _confusing_. The two seem pretty close (and the SSD seems slower!) Obviously, we're looking at the wrong metric. The big distinguishing factor for these two types of storage is _seek time_. So instead of `hdparm`, let's try `ioping` instead.



Here's the workstation with the hard drive first (run twice, first random then sequential):



```sh

$ sudo ioping -R /dev/sda1



--- /dev/sda1 (device 243.0 MiB) ioping statistics ---

450 requests completed in 3.0 s, 150 iops, 600.3 KiB/s

min/avg/max/mdev = 54 us / 6.7 ms / 74.1 ms / 6.5 ms



$ sudo ioping -RL /dev/sda1



--- /dev/sda1 (device 243.0 MiB) ioping statistics ---

1.8 k requests completed in 3.0 s, 600 iops, 150.1 MiB/s

min/avg/max/mdev = 648 us / 1.7 ms / 152.2 ms / 5.2 ms

```



And now ... here's the VM with the SSD:



```sh

agam@myvm:~$ sudo ioping -R /dev/disk/by-uuid/foo

--- /dev/disk/by-uuid/foo (device 10.0 Gb) ioping statistics ---

9020 requests completed in 3000.1 ms, 5016 iops, 19.6 mb/s

min/avg/max/mdev = 0.0/0.2/6.0/0.4 ms



agam@myvm:~$ sudo ioping -RL /dev/disk/by-uuid/foo

--- /dev/disk/by-uuid/foo (device 10.0 Gb) ioping statistics ---

1230 requests completed in 3002.0 ms, 439 iops, 109.6 mb/s

min/avg/max/mdev = 0.2/2.3/8.2/0.6 ms

```



_Here_ is where you see a big difference ... a smaller one for the sequential case, but a huge one for the random case (at least an order of magnitude more).



So yeah, the SSD is legit.
