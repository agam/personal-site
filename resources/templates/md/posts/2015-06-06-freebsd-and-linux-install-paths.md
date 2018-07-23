{:title "freebsd and linux install paths"
:layout :post
 :tags ["old-post"]}



One of those **WTF** moments of unexplainable distro choices. This on an ubuntu box:



```sh

$ which netcat

/bin/netcat



$ ls -l /bin/netcat

lrwxrwxrwx 1 root root 24 Oct 14  2013 /bin/netcat -> /etc/alternatives/netcat



$ ls -l /etc/alternatives/netcat

lrwxrwxrwx 1 root root 15 Oct 14  2013 /etc/alternatives/netcat -> /bin/nc.openbsd



$ which nc

/usr/bin/nc



$ ls -l /usr/bin/nc

lrwxrwxrwx 1 root root 7 Jun  4  2014 /usr/bin/nc -> /bin/nc



$ ls -l /bin/nc

lrwxrwxrwx 1 root root 20 Oct 14  2013 /bin/nc -> /etc/alternatives/nc



$ ls -l /etc/alternatives/nc

lrwxrwxrwx 1 root root 15 Oct 14  2013 /etc/alternatives/nc -> /bin/nc.openbsd

```



... whereas on FreeBSD:



```sh

% ls -l /usr/bin/nc

-r-xr-xr-x  1 root  wheel  28008 Nov 11  2014 /usr/bin/nc



% file /usr/bin/nc

/usr/bin/nc: ELF 64-bit LSB executable, x86-64, version 1 (FreeBSD), dynamically linked (uses shared libs), for FreeBSD 10.1, stripped

```



Which would you rather remember?
