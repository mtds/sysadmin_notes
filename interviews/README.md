# Sysadmin Interview Questions

The questions reported here comes from the [Linux Sysadmin Interview](https://github.com/chassing/linux-sysadmin-interview-questions) repository published on Github.

I took the liberty to reorganize or remove some of the questions, whenever I deemed it necessary.
 
## <a name='toc'>Table of Contents</a>

  1. [General Questions](#general)
  1. [Simple Linux Questions](#simple)
  1. [Medium Linux Questions](#medium)
  1. [Hard Linux Questions](#hard)
  1. [Expert Linux Questions](#expert)
  1. [Networking Questions](#network)

#### [[⬆]](#toc) <a name='general'>General Questions:</a>

### What function does DNS play on a network?

The Domain Name System (DNS) is a hierarchical decentralized naming system for computers, services, or other resources connected to the Internet or a private network. It associates various information with domain names assigned (NS/MX/TXT records too) to each of the participating entities. Most prominently, it translates more readily memorized domain names to the numerical IP addresses needed for locating and identifying computer services and devices with the underlying network protocols.

Reference: [DNS page on Wikipedia](https://en.wikipedia.org/wiki/Domain_Name_System)

### What is HTTP?

The Hypertext Transfer Protocol (HTTP) is an application protocol for distributed, collaborative, and hypermedia information systems.[1] HTTP is the foundation of data communication for the World Wide Web. Hypertext is structured text that uses logical links (hyperlinks) between nodes containing text. HTTP is the protocol to exchange or transfer hypertext.

Reference: [HTTP page on Wikipedia](https://en.wikipedia.org/wiki/Hypertext_Transfer_Protocol)

### What is an HTTP proxy and how does it work?

In computer networks, a proxy server is a server (a computer system or an application) that acts as an intermediary for requests from clients seeking resources from other servers. A client connects to the proxy server, requesting some service, such as a file, connection, web page, or other resource available from a different server and the proxy server evaluates the request as a way to simplify and control its complexity.

A HTTP proxy speaks the HTTP protocol and it's especially made for HTTP connections:

1. The browser (CLIENT) sends GET http://SERVER/path HTTP/1.1 to the PROXY.
2. Now the PROXY will forward the actual request to the SERVER.
3. The SERVER will only see the PROXY as connection and answer to the PROXY just like to a CLIENT.
4. The PROXY receives the response and forwards it back to the CLIENT.

The process is (usually) transparent to the client and nearly like directly communicating with the server. Additional headers could be introduced by the proxy itself and the content traversing the proxy can be changed for various reasons. Some proxies for example include your real IP in a special HTTP HEADER which can be logged server-side, or intercepted in their scripts. 

References: 
- [Proxy Server on Wikipedia](https://en.wikipedia.org/wiki/Proxy_server)
- [Proxy servers and tunneling (Mozilla Developers Network](https://developer.mozilla.org/en-US/docs/Web/HTTP/Proxy_servers_and_tunneling)

### Describe briefly how HTTPS works.

HTTPS takes the well-known and understood HTTP protocol, and layers a SSL/TLS encryption layer on top of it. Servers and clients still speak exactly the same HTTP to each other, but over a secure SSL connection that encrypts and decrypts their requests and responses.

An SSL connection between a client and server is set up by a handshake which goes through the following steps:
- verify if the client is talking to the right server and viceversa;
- both parties have to agreed on a "cipher suite", which includes which encryption algorithm they will use to exchange data;
- both parties have to agreed on any necessary keys for the aforementioned algorithm.

Once the connection is established, both parties can use the agreed algorithm and keys to securely send messages to each other. The handshake process can be broken up in three main phases (Hello, Certificate Exchange and Key Exchange).

Reference: [How does HTTPS actually works?](https://robertheaton.com/2014/03/27/how-does-https-actually-work/)

### What is SMTP? Give the basic scenario of how a mail message is delivered via SMTP.

Simple Mail Transfer Protocol is an Internet Standard for Electronic Mail Transmission. SMTP defines the message format and the message transfer agent (MTA), which stores and forwards the mail. SMTP is a relatively simple, text-based protocol, where one or more recipients of a message are specified and then the message text is transferred.

References:
- [SMTP page on Wikipedia](https://en.wikipedia.org/wiki/Simple_Mail_Transfer_Protocol)
- [The SMTP Transaction](http://www.tldp.org/HOWTO/Spam-Filtering-for-MX/smtpintro.html)

### What is RAID? What is RAID0, RAID1, RAID5, RAID10?

RAID stands for Redundancy Array of Independent Disks. It is a way to make multiple disks/drives operate as one drive and provide striping, mirroring and parity depending on the type of RAID used.

- RAID 0

![RAID 0](/interviews/imgs/raid0.png)

RAID 0 (also known as a stripe set or striped volume) splits ("stripes") data evenly across two or more disks, without parity information, redundancy, or fault tolerance. Since RAID 0 provides no fault tolerance or redundancy, the failure of one drive will cause the entire array to fail; as a result of having data striped across all disks, the failure will result in total data loss. This configuration is typically implemented having speed as the intended goal.[2][3] RAID 0 is normally used to increase performance, although it can also be used as a way to create a large logical volume out of two or more physical disks.

- RAID 1

![RAID 1](/interviews/imgs/raid1.png)

RAID 1 consists of an exact copy (or mirror) of a set of data on two or more disks; a classic RAID 1 mirrored pair contains two disks. This configuration offers no parity, striping, or spanning of disk space across multiple disks, since the data is mirrored on all disks belonging to the array, and the array can only be as big as the smallest member disk. This layout is useful when read performance or reliability is more important than write performance or the resulting data storage capacity. The array will continue to operate so long as at least one member drive is operational.

- RAID 5

![RAID 5](/interviews/imgs/raid5.png)

RAID 5 consists of block-level striping with distributed parity. Parity information is distributed among the drives. It requires that all drives but one be present to operate. Upon failure of a single drive, subsequent reads can be calculated from the distributed parity such that no data is lost. RAID 5 requires at least three disks.

- RAID 10

![RAID 10](/interviews/imgs/raid10.png)

RAID 10 or RAID 1+0 is a combination of RAID 1 and RAID 0. It provides mirroring and striping for redundancy and higher performance.The main disadvantage is that it needs half the disk space for mirroring while it provides more redundancy and higher performance.

References:
- [Standard RAID levels (Wikipedia)](https://en.wikipedia.org/wiki/Standard_RAID_levels)
- [Nested RAID levels (Wikipedia)](https://en.wikipedia.org/wiki/Nested_RAID_levels)

### What is a level 0 backup? What is an incremental backup?

**Level 0** backup is a full backup where the entire data is backed up byte to byte. **Incremental backup** captures only the changes made since the last incremental backup. In incremental backup, only the newly made changes since the last backup are stored. In the latter case you save both time and disk space but the cost are slow recoveries and the risk of losing data (in case some of the incremental backups was lost).

### Describe the general file system hierarchy of a Linux system.

```
/bin       Essential command binaries
/boot      Static files of the boot loader
/dev       Device files
/etc       Host-specific system configuration
/lib       Essential shared libraries and kernel modules
/media     Mount point for removeable media
/mnt       Mount point for mounting a filesystem temporarily
/opt       Add-on application software packages
/sbin      Essential system binaries
/srv       Data for services provided by this system
/tmp       Temporary files
/usr       Secondary hierarchy
/var       Variable data
/proc      Process information for the kernel. Updated dynamically by the kernel.
```

#### [[⬆]](#toc) <a name='simple'>Simple Linux Questions:</a>

### What is the name and the UID of the administrator user?

The main administrator on a Linux host is called **root** and the UID is **0**, which is usually the same of its group, 'root'. Check UID/GUID of a Linux user with:
```bash
>>> id root
uid=0(root) gid=0(root) groups=0(root)
```

### How to list all files, including hidden ones, in a directory?

Through the following command: ``ls -al``

### What is the Unix/Linux command to remove a directory and its contents?

The usual command is ``rm -rf dir_name`` (to be used with care, since the ``-f`` (force) flag is specified).

### Which command will show you free/used memory? Does free memory exist on Linux?

The most used command is called ``free``.

```bash
>>> free -h
              total        used        free      shared  buff/cache   available
Mem:           7,7G        5,7G        718M        361M        1,3G        1,3G
Swap:          7,6G         46M        7,6G
```
Other possibilities include ``vmstat`` or reading directly the ``/proc/meminfo`` file.

Answer to the second question needs to be more detailed: Linux (and all Unix OS'es) try to have as little free memory as possible. Instead they use memory which is not actively mapped to processes in the running OS for things like file cache and buffers for various IO transfer operations. In general, it could be said that the Linux disk cache is very unobtrusive: it uses spare memory to greatly increase disk access speeds, and without taking any memory away from applications. A fully used store of RAM on Linux is efficient hardware use, not a warning sign.

References:
- [Linux ate my RAM](https://www.linuxatemyram.com/)
- [Experiments with the Linux Disk Cache](https://www.linuxatemyram.com/play.html)

### How do you report the correct memory usage of a running process?

One of the easiest way would be to use the **smem** command. Because large portions of physical memory are typically shared among multiple applications, the standard measure of memory usage known as resident set size (RSS) will significantly overestimate memory usage. ``smem`` can report the proportional set size (PSS) which measures each application’s "fair share" of each shared area to give a realistic measure.

References:
- [smem memory reporting tool](https://www.selenic.com/smem/)
- [Visualizing memory usage with smem (LWN.net)](https://lwn.net/Articles/329458/)

### How to search for the string "my konfi is the best" in files of a directory recursively?

Quickest answer would be to use grep:
```bash
>>> grep -r 'my konfi is the best' ./*
```

### How to connect to a remote server or what is SSH?

```bash
>>> ssh username@server
```

__Secure Shell (SSH)__ is a cryptographic network protocol for operating network services securely over an unsecured network. SSH provides a secure channel over an unsecured network in a client-server architecture, connecting an SSH client application with an SSH server. Common applications include remote command-line login and remote command execution, but any network service can be secured with SSH. SSH was designed as a replacement for Telnet and for unsecured remote shell protocols such as the Berkeley rlogin, rsh, and rexec protocols.

### How to get all environment variables and how can you use them?

Quickest way would be using the ``printenv`` command. Environment variables can be used on the command line directly, since the shell will expand them at runtime (e.g. ``ls $HOME/src``) or inside shell scripts.

### I get "command not found" when I run ``ifconfig -a``. What can be wrong?

Possible causes:
1. net-tools package is not installed in your system.
2. there's no "/sbin" directory in $PATH, so the command can be invoked with the full path: ``/sbin/ifconfig -a``

### What happens if I type TAB-TAB?

It depends on the context:
1. On Bash/Zsh shells, typing TAB-TAB will enable the built in "command completion" function.
2. It can also complete directory paths, when used (for example) with ls: ``ls /usr/lo`` will complete into ``/usr/local``.

### What command will show the available disk space on the Unix/Linux system?

Quickest way would be with the ``df`` command.

### What commands do you know that can be used to check DNS records?

```bash
>>> host example.com
>>> nslookup example.com
>>> dig example.com
```

### What Unix/Linux commands will alter a files ownership, files permissions?

* chown - command to change file owner and group information.
* chmod - command to change file access permissions such as read, write, and access.

### What does ``chmod +x FILENAME`` do?

It will make 'FILENAME' executable by the owner, group owner and others. Note that ``chmod`` will never change permissions of a symbolic link, which in any case are never used (cf. ``man chmod``).

### What does the permission 0750 on a file mean?

``chmod`` can also use an octal notation to change permissions. When 0750 got applied to a file the resulting permissions will be: ``rwxr-x---``, which means that the owner can read\write\execute this file, also members of group can read and execute while other users can do nothing with it.

### What does the permission 0750 on a directory mean?

This permissions means that the owner can read\write\execute (which means be able to see the list of files) this directory, also members of group can read and list while other users can not access nor be able to read or write the directory.

### How to add a new system user without login permissions?

Considering the simplest example of local users (no LDAP or NIS), the command ``adduser`` with the '--system' option can be used. By default, the new system user will have the shell set to ``/bin/false`` and have logins disabled (``man adduser``).

### How to add/remove a group from a user?

For local users, the command ``adduser`` can be used to add a group to a user. In case a user need to be removed from a group, there are two different command: ``deluser user group`` or  ``gpasswd -d user group``, depending if you are in a Debian/Ubuntu environment or in a RHEL/CentOS/Fedora one.

### What is a Bash alias?

A Bash alias is essentially nothing more than a keyboard shortcut, an abbreviation, a means of avoiding typing a long command sequence. If, for example, we include ``alias lm="ls -l | more"`` in the ~/.bashrc file, then each lm typed at the command-line will automatically be replaced by a ``ls -l | more``. This can save a great deal of typing at the command-line and avoid having to remember complex combinations of commands and options.

Reference: [Aliases page at the Linux Documentation Project](https://www.tldp.org/LDP/abs/html/aliases.html)

### How do you set the mail address of the root/a user?

Answer to this question is higly dependent on which kind of MTA service is running on the machine and it s configured. In general, it would be possible to change root or other user's email addresses through the ``/etc/aliases`` file (if Postfix is used) or modifying the ``.forward`` file in the home directory of the user.

### What does CTRL-c do?

On both POSIX and Windows this sends a **SIGINT** (aka "Interrupt") signal. By default this will terminate the process. But, a program may provide its own handler, which should do some clean up work and then return control to you (and it might not actually terminate – imagine software doing a massive, slow, calculation; a SIGINT might request that it return the best result so far and then keep going, for example).

Reference: [Signalling for your process’s attention](https://blog.superuser.com/2011/04/13/signalling-for-your-processs-attention/)

### What is in /etc/services?

Most UNIX network services are provided by individual programs called servers. For a server to operate, it must be assigned a protocol, e.g. TCP or UDP, be assigned a port number, and somehow be started. When a client opens a connection across the network to a server, the client uses the port to specify which service it wishes to use. These ports are called __well-known ports__ because they need to be known in advance by both the client and the server. UNIX uses the /etc/services file as a small local database. For each service this file specifies the service’s well-known port number and notes whether the service is available as a TCP or UDP service. The /etc/services file is distributed as part of the UNIX operating system and the information in it are derived from Internet RFCs and other sources.

Most UNIX servers determine their port number by looking up each port in the ``/etc/services`` file using the ``getservbyname()`` library call.

### How to redirect STDOUT and STDERR in bash? (> /dev/null 2>&1)

- stdout redirection: ``program > output``
- stderr redirection: ``program 2> err.txt``
- stdout AND stderr redirection: ``program &> .txt``
- stdout redirect to stderr: ``program 1>&2``
- stderr redirect to stdout: ``program 2>&1``

References:
- [Bash Programming Guide: Intro to Redirection.](http://tldp.org/HOWTO/Bash-Prog-Intro-HOWTO-3.html)
- [Advanced Bash Scripting Guide: I/O Redirection.](https://www.tldp.org/LDP/abs/html/io-redirection.html)

### What is the difference between UNIX and Linux?

Linux is just a **kernel**: all Linux distributions includes GUI system plus GNU utilities (such coreutils, bash, etc.) and installation/management tools, including development tools like the GNU Compilers Collection and Editors (vi/emacs/etc.) and various applications (browsers, office productivity suite, multimedia tools, etc.). However, most UNIX operating systems are considered as a complete operating system as everything comes from a single source or vendor (consider OS like AIX, IRIX, HP-UX, etc.). Another big difference is the license: Linux distributions are mostly following the open source licensing models while commercial UNIX operative systems are offered only after paying a fee to the vendor (in the past, it includes specialized hardware, peripherals, manuals, etc.).

### What is the difference between Telnet and SSH?

The main difference is the possibility of SSH clients to encrypt their communication channel with a remote server while on the other hand Telnet is only able to send data in clear text (including usernames and passwords) and thus it's a huge security risk.

### Explain the three load averages and what do they indicate. What command can be used to view the load averages?

The three numbers after load average represent the 1-, 5-, and 15-minute load averages on the machine. A system load average is equal to the average number of processes in a runnable or uninterruptible state. Runnable processes are either currently using the CPU or waiting to do so, and uninterruptible processes are waiting for I/O. On Unix/Linux, the system load is a measure of the amount of computational work that a computer system performs. The load average represents the average system load over a period of time.

Commands to view the load averages:
```bash
>>> top
>>> uptime
>>> cat /proc/loadavg
```

References:
- [Computing Load (Wikipedia)](https://en.wikipedia.org/wiki/Load_%28computing%29)
- [Linux Load Averages: Solving the Mystery (Brendan Gregg)](http://www.brendangregg.com/blog/2017-08-08/linux-load-averages.html)
- [Understanding Linux CPU Load](http://blog.scoutapp.com/articles/2009/07/31/understanding-load-averages)

### What is a Linux kernel module?

Kernel modules are pieces of code that can be loaded and unloaded into the kernel upon demand. They extend the functionality of the kernel without the need to reboot the system. Reference: [Kernel Modules (Archlinux Wiki)](https://wiki.archlinux.org/index.php/Kernel_module)

### Walk me through the steps in booting into single user mode to troubleshoot a problem.

Consider using a server which uses GRUB as boot loader. Once booted, wait until the kernel selection menu appears and then press ``e`` in order to modify the boot parameters for the kernel: add the string ``single`` or the number ``1`` to it, in order to force Linux into booting in single user mode. At the end of the booting process you'll be dropped directed into a root shell (few network services, if any at all, will be started).

### Walk me through the steps you'd take to troubleshoot a 404 error on a web application you administer.

Broad topic so a few considerations are needed. "404 Not Found Error" is an HTTP response status code, which indicates that the requested resource could not be found. Since it's an error on the client side, we are supposing that a browser or another application did try to access a resource on the server which should be available but it is not.

Checklist:
- Verify the logs on the server side: which URL has generated this error?
- Check the web root directory and identify the requested resource.
- Verify that the web server configuration (Apache, NGINX, etc.) has not changed.
- If the web application is using a DB as a backend, verify if the anything related to the missed resource was changed.

#### [[⬆]](#toc) <a name='medium'>Medium Linux Questions:</a>

### What do the following commands do and how would you use them?

* ``tee``: read from standard input and write to standard output and files.
* ``awk``: it is a powerful method for processing or analyzing text files—in particular, data files that are organized by lines (rows) and columns.
* ``tr``: translate or delete characters.
* ``cut``: remove sections from each line of files.
* ``tac``: concatenate and print files in reverse.
* ``curl``: a tool to transfer data from or to a server (it supports ftp/ftps,http/https,ldap/ldaps,imap/imaps,smtp/smtps,etc.).
* ``wget``: utility for non-interactive download of files from the Web. It supports HTTP, HTTPS, and FTP protocols, as well as retrieval through HTTP proxies.
* ``watch``: execute a program periodically, showing output fullscreen.
* ``head``: output the first N lines of a file.
* ``tail``: output the last N lines of a file.

### What does an ``&`` after a command do?

This is known as __job control__ under Linux/Unix. With ``&`` the process starts in the background, so it is possible to use the shell and do not have to wait until the script is finished. Jobs in background will be reported with ``jobs``. Using ``fg`` the command previously in background will be reported in foreground, whicle it's possible to use the combination of Ctrl-Z and then ``bg`` to put a command/script in background.

### What does ``& disown`` after a command do?

After the command is put in background by ``&``, ``disown`` removes the job from the shell's job list. It means basically that the command in background will not be listed by ``jobs``, it cannot be turned into a foreground job using ``fg`` and it will not receive a SIGHUP if the shell receives this signal.

### What is a packet filter and how does it work?

Packet filtering is a firewall technique used to control network access by monitoring outgoing and incoming packets and allowing them to pass or halt based on the source and destination Internet Protocol (IP) addresses, protocols and ports. During network communication, a node transmits a packet that is filtered and matched with predefined rules and policies. Once matched, a packet is either accepted or denied.

### What is Virtual Memory?

Linux supports virtual memory, that is, using a disk as an extension of RAM so that the effective size of usable memory grows correspondingly. The kernel will write the contents of a currently unused block of memory to the hard disk so that the memory can be used for another purpose. When the original contents are needed again, they are read back into memory. This is all made completely transparent to the user; programs running under Linux only see the larger amount of memory available and don't notice that parts of them reside on the disk from time to time. Of course, reading and writing the hard disk is slower (on the order of a thousand times slower) than using real memory, so the programs don't run as fast. The part of the hard disk that is used as virtual memory is called the swap space.

Reference: [Linux Systems Administrator Guide](https://www.tldp.org/LDP/sag/html/vm-intro.html)

### What is swap and what is it used for?

Linux divides its physical RAM (random access memory) into chunks of memory called pages. Swapping is the process whereby a page of memory is copied to the preconfigured space on the hard disk, called swap space, to free up that page of memory. The combined sizes of the physical memory and the swap space is the amount of virtual memory available.

Reference: [Swap (Archlinux Wiki](https://wiki.archlinux.org/index.php/swap)

### What is an A record, an NS record, a PTR record, a CNAME record, an MX record?

* ``A``:    it specifies an IP address (IPv4) for a given host.
* ``NS``:   it specifies an authoritative name server for a given host or domain.
* ``MX``:   it specifies a mail exchange server for a DNS domain name. The information is used by SMTP to route emails to proper hosts. 
* ``PTR``:  as opposed to forward DNS resolution (A and AAAA DNS records), this record is used to look up host/domain names based on an IP address.
* ``CNAME``: it specifies a DNS alias for a domain or host name.

### Are there any other RRs and what are they used for?

Additional ``Resource Records`` are:
* ``SOA``: it specifies core information about a DNS zone, including the primary name server, the email of the domain administrator, the domain serial number, and several timers relating to refreshing the zone.
* ``TXT``: the text record can hold arbitrary non-formatted text string. Typically, the record is used by __SPF__.

### What is a Split-Horizon DNS?

Split-horizon DNS is designed to provide different authoritative answers, usually selected by the source address of the DNS request. One common use case for Split-horizon DNS is when a webserver has a private IP address on a local area network, but the world accesses it at a NAT'ed public address. By using split-horizon DNS the same URL can lead to either private IP address or public IP address for different client machines. This allows for critical local client machines to access a webserver directly through a network switch, without the need to pass through a router. Passing through fewer network devices improves the network latency.

### What is the sticky bit?

A sticky bit is a permission bit that is set on a directory and allows only the owner or the root user to delete or rename files int. No other user has the needed privileges to delete files created by some other user. Use: ``chmod (+|-)t /dir``.

### What does the immutable bit do to a file?

A file with an immutable attribute cannot be:

- Modified
- Deleted
- Renamed
- Subject to symlinks (hard or soft), including the root user.

Only root or a process possessing the ``CAP_LINUX_IMMUTABLE`` capability can set or clear this attribute. Attributes can be added or removed with ``chattr`` and listed with ``lsattr``.

### What is the difference between hardlinks and symlinks? What happens when you remove the source to a symlink/hardlink?

**Hard Links**: In Linux, whenever you perform an listing in a directory, the listing is actually a list of references that map to an inode. An hard link is yet another reference to the same inode as the original file and it allows a user to create two exact files without having to duplicate the data on disk. However unlike creating a copy, if you modify the hard link you are in turn modifying the original file as well as they both reference the same inode. Hard links have some limitations however, in most (but not all) Unix/Linux distributions hard links cannot be created for directories. Hard links are also not allowed to cross file systems, since inodes are unique to each single file system.

**Symbolic Links**: A symbolic link is similar to a hard link in that it is used to link to an already existing file, however it is very different in its implementation. A symbolic link is not a reference to an inode but rather an pointer that redirects to another file or directory. Differently from hard links, symlinks can be used to reference a file in another file system or to point to a directory.

__Differences__: Since a hard link is simply a reference to an inode, you can actually delete the original file and the data will still be available via the hard link while with a symlink, if the target is deleted, it will no longer be able to provide data.

### What is an inode and what fields are stored in an inode?

The inode is a data structure in a Unix-style file system that describes a filesystem object such as a file or a directory. Each inode stores the attributes and disk block location(s) of the object's data. Filesystem object attributes may include metadata (times of last change, access, modification), as well as owner and permission data. Directories are lists of names assigned to inodes. A directory contains an entry for itself, its parent, and each of its children.

Reference: [Inode (Wikipedia)](https://en.wikipedia.org/wiki/Inode)

### How to force/trigger a file system check on next reboot?

If it's only needed once then it is sufficient to do: ``touch /forcefsck``. For more regular filesystem checks, it is possible to use the ``tune2fs`` command:

```bash
>>> tune2fs -c 30 /dev/sda1 # force checks every 30 mounts
>>> tune2fs -i 3m /dev/sda1 # force checks every 3 months
```

### What is SNMP and what is it used for?

Simple Network Management Protocol (SNMP) is an internet standard protocol which can be used to remotely retrieve operational statistics on devices attached to the network (e.g. routers, switches, firewalls, etc.). SNMP data can be collected through NMS (Network Management System) like Zabbix, OpenNMS, Nagios, Zenoss.

Reference: [SNMP (Linux Journal)](https://www.linuxjournal.com/content/snmp)

### What is a runlevel and how to get the current runlevel?

A runlevel is one of the modes that a Unix-based operating system will run in. Each runlevel has a certain number of services stopped or started, giving the user control over the behavior of the machine. Conventionally, seven runlevels exist, numbered from zero to six:

| Run Level | Mode        |      Action
|-----------|-------------|----------------
|    0	    |     Halt	  |  Shuts down system
|    1	    | Single-User |  Does not configure network interfaces, start daemons, or allow non-root logins
|    2      |  Multi-User |  Does not configure network interfaces or start daemons
|    3      |  Multi-User |  Starts the system normally, including networking
|    4      |  Undefined  |  Not used/User-definable
|    5      |     X11     |  As runlevel 3 + display manager(X11/Xorg)
|    6      |    Reboot   |  Reboots the system

**init** based systems:

```bash
>>> /sbin/runlevel
N 3
```

**systemd** based systems: SystemD uses "targets" instead of run-levels and relies on the ``systemctl`` command to change runlevel or to change the target.

```bash
>>> systemctl get-default
multi-user.target
```

### What is SSH port forwarding?

SSH port forwarding is a mechanism in SSH for tunneling application ports from the client machine to the server machine, or vice versa. It can be used for adding encryption to legacy applications, going through firewalls, accessing network services behind NAT (reverse tunnel), etc.

### What is the difference between local and remote port forwarding?

* __Local forwarding__ is used to forward a port from the client machine to the server machine. Basically, the SSH client listens for connections on a configured port, and when it receives a connection, it tunnels the connection to an SSH server. The server connects to a configurated destination port, possibly on a different machine than the SSH server.

```bash
>>> ssh -L 80:intra.example.com:80 gw.example.com
```
This example opens a connection to the gw.example.com jump server, and forwards any connection to port 80 on the local machine to port 80 on intra.example.com.

* __Remote forwarding__ allows anyone on the remote server to connect to an TCP/UDP port on the remote server. The connection will then be tunneled back to the client host, and the client then makes a TCP/UDP connection to a local port on localhost. Any other host name or IP address could be used instead of localhost to specify the host to connect to.

```bash
>>> ssh -R 8080:localhost:80 public.example.com
```
This particular example would be useful for giving someone on the outside access to an internal web server. Or exposing an internal web application to the public Internet. This feature could be stopped on the SSH gateway with the sshd_config: ``GatewayPorts no``.

References:
* [SSH Tunneling examples (ssh.com)](https://www.ssh.com/ssh/tunneling/example)
* [SSH Tunnel - Local and Remote Port Forwarding Explained With Examples](https://blog.trackets.com/2014/05/17/ssh-tunnel-local-and-remote-port-forwarding-explained-with-examples.html)

### What are the steps to add a user to a system without using useradd/adduser?

1. Add an entry for the user in /etc/passwd file (using ``vipw``).
2. Add an entry for the group in /etc/group file (using ``vigr``).
3. Create the home directory for the new user.
4. Copy the files undeer ``/etc/skel`` to the newly created home directory.
5. Set the new user password using the ``passwd`` command.

### What is MAJOR and MINOR numbers of special files?

One of the basic features of the Linux kernel is that it abstracts the handling of devices. All hardware devices look like regular files; they can be opened, closed, read and written using the same, standard, system calls that are used to manipulate files. Every device in the system is represented by a file. For block (disk) and character devices, these device files are created by the ``mknod`` command and they describe the device using __major__ and __minor__ device numbers. Network devices are also represented by device special files but they are created by Linux as it finds and initializes the network controllers in the system.

The __major number__ identifies the driver associated with the device. For example, ``/dev/null`` and ``/dev/zero`` are both managed by driver 1, whereas virtual consoles and serial terminals are managed by driver 4; similarly, both vcs1 and vcsa1 devices are managed by driver 7. The kernel uses the major number at open time to dispatch execution to the appropriate driver.

The __minor number__ is used only by the driver specified by the major number; other parts of the kernel don’t use it, and merely pass it along to the driver. It is common for a driver to control several devices; the minor number provides a way for the driver to differentiate among them.

Reference:  [Linux Device Drivers (2nd Edition, O'Reilly)](https://www.safaribooksonline.com/library/view/linux-device-drivers/0596000081/)

### Describe the mknod command and when you'd use it.

``mknod`` is the command used to create device files (character or block devices). It follows the syntax:

```bash
>>> mknod device-name device-type(c|b) major-number minor-number
```

* device-name - full name of device (e.g. ``/dev/random``).
* device-type - device files can represent two types of device, a storage device (block) or a device use for other purpose (character). 
  - block devices are things like cd-roms, hard drives, etc. 
  - character devices are things like /dev/zero, /dev/null, or any other device not used to store info.
* major-number - driver associated with the device.
* minor-number - the number of the device within the group.

### Describe a scenario when you get a "filesystem is full" error, but 'df' shows there is free space.

One possibility is that the filesystem ran  out of __inodes__.

### Describe a scenario when deleting a file, but 'df' not showing the space being freed.

On Linux or Unix systems, deleting a file via rm or through a file manager application will unlink the file from the file system's directory structure; however, if the file is still open (in use by a running process) it will still be accessible to this process and will continue to occupy space on disk. Therefore such processes may need to be restarted before that file's space will be cleared up on the filesystem. It is useful in this case to identify processes which are still have opened files on the filesystem with ``lsof``.

Reference: [Why is space not being freed from disk after deleting a file? (Red Hat portal)](https://access.redhat.com/solutions/2316)

### Describe how 'ps' works.

On Linux, the ps command works by reading files in the [proc filesystem](https://en.wikipedia.org/wiki/Procfs#Linux).

Reference: [Linux Kernel docs about procfs](https://www.kernel.org/doc/Documentation/filesystems/proc.txt) 

### What happens to a child process that dies and has no parent process to wait for it and what’s bad about this?

An orphan process is a computer process whose parent process has finished or terminated, though it remains running itself. In a Unix-like operating system any orphaned process will be immediately adopted by the special init system process: the kernel sets the parent to init. This operation is called re-parenting and occurs automatically. Even though technically the process has the "init" process as its parent, it is still called an orphan process since the process that originally created it no longer exists. In other systems orphaned processes are immediately terminated by the kernel. In modern Linux systems, an orphan process may be reparented to a **subreaper** process instead of init.

References:
* [Orphaned Processes (Wikipedia)](https://en.wikipedia.org/wiki/Orphan_process)
* [Zombie Processes (Wikipedia)](https://en.wikipedia.org/wiki/Zombie_process)

### Explain briefly each one of the process states.

status |  cause
-------|-------
   D   | uninterruptible sleep (usually IO)
   R   |running or runnable (on run queue)
   S   | interruptible sleep (waiting for an event to complete)
   T   | stopped, either by a job control signal or because it is being traced
   Z   | defunct ("zombie") process, terminated but not reaped by its parent

Life cycle of a process:

* Spawn
* Waiting
* Runnable
* Running
* Stopped
* Zombie
* Removed from process table

### What is a zombie process and what could be the cause of it?

On Unix and Unix-like computer operating systems, a zombie process or defunct process is a process that has completed execution (via the exit system call) but still has an entry in the process table: it is a process in the "Terminated state". If a parent process never calls wait(), its zombie children will stick around in memory until they’re cleaned up.

Linux systems have a finite number of process IDs (32767 by default otherwise check with ``cat /proc/sys/kernel/pid_max``). If zombies are accumulating at a very quick rate - for example, if improperly programmed server software is creating zombie processes under load - the entire pool of available PIDs will eventually become assigned to zombie processes, preventing other processes from launching.

### What is the difference between a process and a thread? And parent and child processes after a fork system call?

* Threads
  - Will by default share memory
  - Will share file descriptors
  - Will share filesystem context
  - Will share signal handling

* Processes
  - Will by default not share memory
  - Most file descriptors not shared
  - Don't share filesystem context
  - Don't share signal handling

After forking, the child is a copy of the parent, with both returning from the system call executing the same code, differing only in return value: the child's PID to the parent, zero to the child.

### What is the difference between exec and fork?

The __fork()__ system call will spawn a new child process which is an identical process to the parent except that has a new system process ID. The process is copied in memory from the parent and a new process structure is assigned by the kernel. The return value of the function is which discriminates the two threads of execution. A zero is returned by the fork function in the child's process. The environment, resource limits, umask, controlling terminal, current working directory, root directory, signal masks and other process resources are also duplicated from the parent in the forked child process.

Forking provides a way for an existing process to start a new one, but what about the case where the new process is not part of the same program as parent process? This is the case in the shell; when a user starts a command it needs to run in a new process, but it is unrelated to the shell. This is where the __exec()__ system call comes into play. ``exec`` will replace the contents of the currently running process with the information from a program binary. Thus the process the shell follows when launching a new program is to firstly fork, creating a new process, and then exec (i.e. load into memory and execute) the program binary it is supposed to run.

### How can you limit process memory usage?

* Easiest way would be using ulimit or configuring ``/etc/security/limits.conf``.
* Modern Linux systems added support for __cgroups__ (control groups), a kernel feature that allows you to allocate resources - such as CPU time, system memory, network bandwidth, or combinations of these resources - among hierarchically ordered groups of processes running on a system. 

Reference: [Cgroups (RedHat Resource Management Guide)](https://access.redhat.com/documentation/en-us/red_hat_enterprise_linux/7/html/resource_management_guide/chap-introduction_to_control_groups)

### What is "nohup" used for?

It allows to keep a program spawned from the shell running, even when the shell is closed.

```bash
>>> nohup my_script &
```

### How to know which process listens on a specific port?

There are three different ways:

* ``netstat -ltunp`` (Shows all PIDs listening for TCP/UDP connections).
* ``fuser 7000/tcp`` (Find out the processes PID that opened tcp port 7000).
* ``lsof -i :80 | grep LISTEN`` (List all PIDs listening for connections on port 80).

Additional information about a process could always be extracted from ``/proc/$PID``.

### You run a bash script and you want to see its output on your terminal and save it to a file at the same time. How could you do it?

```bash
>>> ntpq -p | tee output
```

### What is the difference between these two commands?

* ``myvar=hello``
* ``export myvar=hello``

When ``export`` is used the variable ``myvar`` is available to __any process__ spawned from the same shell. In the first case we have a simple assignment which means the scope of ``myvar`` is limited only to the shell itself. There's also the edge case: ``myvar=hello command``, in this case the variable is also available in the command subprocess environment.

### What is bash quick substitution/caret replace(^x^y)?

It's a feature called __history expansion__: it will take the last command and replace the first instance of "x" with "y".

Reference: [Catonmat: The Definitive Guide to Bash Command Line History](http://www.catonmat.net/blog/the-definitive-guide-to-bash-command-line-history/)

### Explain what echo "1" > /proc/sys/net/ipv4/ip_forward does.

Enable the 'IP Forwarding', a feature of the Linux kernel. In this case, it is just a synonym for "routing".

### What is a wildcard certificate?

In computer networking, a wildcard certificate is a public key certificate which can be used with multiple subdomains of a domain. The principal use is for securing web sites with HTTPS, but there are also applications in many other fields. Compared with conventional certificates, a wildcard certificate can be cheaper and more convenient than a certificate for each subdomain.

Reference: [Wildcard Certificate (Wikipedia)](https://en.wikipedia.org/wiki/Wildcard_certificate)

### Describe briefly the steps you need to take in order to create and install a valid certificate for the site https://foo.example.com.

* Create a Certificate Signing Request (CSR) for the domain.
* Once the CSR is ready, it can be submitted to a certification authority (CA) to be signed.

Reference: [OpenSSL essentials (Digital Ocean)](https://www.digitalocean.com/community/tutorials/openssl-essentials-working-with-ssl-certificates-private-keys-and-csrs)

### Can you have several HTTPS virtual hosts sharing the same IP?

Yes, it is possible thanks to an extension to the SSL protocol called __Server Name Indication__ (RFC 4366), which allows the client to include the requested hostname in the first message of its SSL handshake (connection setup). This allows the server to determine the correct named virtual host for the request and set the connection up accordingly from the start.

Reference: [SSL with Virtual Hosts Using SNI (Apache Wiki)](https://wiki.apache.org/httpd/NameBasedSSLVHostsWithSNI)

### Which Linux file types do you know?

* ``-`` : regular file (text files, images, binary files, shared libraries, etc.)
* ``d`` : directory
* ``c`` : character device file
* ``b`` : block device file
* ``s`` : local socket file (they are used for communication between processes, for example services like X, syslog, etc.)
* ``p`` : named pipe (allow communication between two local processes)
* ``l`` : symbolic link

### How many NTP servers would you configure in your local ntp.conf?

At least three servers.

References: 
* [NTP Advanced Configuration](http://www.ntp.org/ntpfaq/NTP-s-config-adv.htm)
* [NTP Best Practices](https://insights.sei.cmu.edu/sei_blog/2017/04/best-practices-for-ntp-services.html)

### What does the column 'reach' mean in ``ntpq -p`` output?

The value displayed in column **reach** is __octal__, and it represents the reachability register. One digit in the range of 0 to 7 represents three bits. The initial value of that register is 0, and after every poll that register is shifted left by one position. If the corresponding time source sent a valid response, the rightmost bit is set.

During a normal startup the registers values are these: 0, 1, 3, 7, 17, 37, 77, 177, 377

References:
* [NTP FAQ (Troubleshooting section)](http://www.ntp.org/ntpfaq/NTP-s-trouble.htm)
* [Understanding reachability statistics (Linux Journal)](https://www.linuxjournal.com/article/6812)

### You need to upgrade kernel at 100-1000 servers, how you would do this?

1. Isolate a small subset of the server fleet with identical HW specs.
2. Proceed with the kernel upgrade.
3. Let this subset of the server fleet run for a period of time which last three or four days.
4. If anything goes well (no kernel panics, services running as expected, etc.) proceed further to upgrade the server fleet.

Supposedly, you can rely on configuration management tools (Cfengine, Ansible, Chef, SaltStack, etc.) to do the upgrade process programmatically. If possible, tools like [Spacewalk](https://spacewalkproject.github.io/) and [Foreman](https://www.theforeman.org/) from RedHat or [Landscape](https://landscape.canonical.com/) from Canonical can help further in the process of managing upgrades for hundreds of servers.

References:
* [Byte.nl: upgrade 2000 Ubuntu servers](https://www.byte.nl/blog/dont-run-this-on-any-system-you-expect-to-be-up-they-said-but-we-did-it-anyway)
* [Live Upgrading Thousands of Servers from an Ancient Red Hat Distribution to 10 Year Newer Debian Based One (Google - LISA 2013)](https://www.usenix.org/system/files/conference/lisa13/lisa13-merlin.pdf)

### What is a tarpipe (or, how would you go about copying everything, including hardlinks and special files, from one server to another)?

The simplest tar pipe possible would be the following:

```bash
>>> (cd src && tar -cf - .) | (cd dest && tar -xpf -)
```

It basically means "copy the src directory to dst, preserving permissions and other special stuff." It does this by firing up two tars - one tarring up src, the other untarring to dst, and wiring them together.

### How can you tell if the httpd package was already installed?

* RedHat/CentOS/Fedora: ``yum list installed | grep httpd``
* Debian/Ubuntu: ``dpkg --list | grep httpd``

### How can you list the contents of a package?

* Debian/Ubuntu: ``dpkg -L <package>``
* RedHat/CentOS/Fedora: ``repoquery -l <package>``

### Can you explain to me the difference between block based, and object based storage?

* __Block Storage__ devices provide fixed-sized raw storage capacity. Each storage volume can be treated as an independent disk drive and controlled by an external server operating system. This block device can be mounted by the guest operating system as if it were a physical disk. The most common examples of Block Storage are SAN, iSCSI, and local disks.

* __Object Storage__  is a computer data storage architecture that manages data as objects, as opposed to other storage architectures like file systems which manage data as a file hierarchy, and block storage which manages data as blocks within sectors and tracks. Each object typically includes the data itself, a variable amount of metadata, and a globally unique identifier. Object storage can be implemented at multiple levels, including the device level (object-storage device), the system level, and the interface level. In each case, object storage seeks to enable capabilities not addressed by other storage architectures, like interfaces that can be directly programmable by the application, a namespace that can span multiple instances of physical hardware, and data-management functions like data replication and data distribution at object-level granularity.

References:
* [Object Storage (Wikipedia)](https://en.wikipedia.org/wiki/Object_storage)
* [Object Storage and Block Storage use cases](https://cloudacademy.com/blog/object-storage-block-storage/)

### How can you get Host, Channel, ID, LUN of SCSI disk?

Easiest way: ``cat /proc/scsi/scsi`` or look into the kernel ring buffer with: ``dmesg | grep -i "attached "``.

#### [[⬆]](#toc) <a name='hard'>Hard Linux Questions:</a>

### What is a tunnel and how you can bypass a http proxy?

Tunneling is a way to transform data frames to allow them pass networks with incompatible address spaces or even incompatible protocols. There are different kinds of tunnels: some process only IPv4 packets and some can carry any type of frame. Linux kernel supports three tunnel types: IPIP (IPv4 in IPv4), GRE (IPv4/IPv6 over IPv4) and SIT (IPv6 over IPv4). Tunnels are managed with the ``ip`` program, part of the Iproute2 package.

References:
* [Tunneling (Linux Foundation Wiki)](https://wiki.linuxfoundation.org/networking/tunneling)
* [Set Up SSH tunneling on a Linux/Unix/BSD server to bypass NAT](https://www.cyberciti.biz/faq/set-up-ssh-tunneling-on-a-linux-unix-bsd-server-to-bypass-nat/)

### What is the difference between IDS and IPS?

* __Firewall__ - A device or application that analyzes packet headers and enforces policy based on protocol type, source address, destination address, source port, and/or destination port. Packets that do not match policy are rejected.
* __Intrusion Detection System__ - A device or application that analyzes whole packets, both header and payload, looking for known events. When a known event is detected a log message is generated detailing the event.
* __Intrusion Prevention System__ - A device or application that analyzes whole packets, both header and payload, looking for known events. When a known event is detected the packet is rejected.

### What is the Linux Standard Base?

The Linux Standard Base (LSB) is a joint project by several Linux distributions under the organizational structure of the Linux Foundation to standardize the software system structure, including the filesystem hierarchy used in the Linux operating system. The LSB is based on the POSIX specification, the Single UNIX Specification (SUS), and several other open standards, but extends them in certain areas.

Reference: [Linux Standard Base (Wikipedia)](https://en.wikipedia.org/wiki/Linux_Standard_Base)

### What is an atomic operation?

An atomic operation is an operation that will always be executed without any other process being able to read or change state that is read or changed during the operation.

References: 
* [OSDev Wiki](https://wiki.osdev.org/Atomic_operation)
* [Atomic vs. Non-Atomic Operations](https://preshing.com/20130618/atomic-vs-non-atomic-operations/)

### Your freshly configured http server is not running after a restart, what can you do?

1. Check for error messages in the log (error.log, access.log).
2. Perhaps the configuration file is not syntatically correct.
3. Lack of resources on the host machine?

### What kind of keys are in ~/.ssh/authorized_keys and what it is this file used for?

Authorized keys specify which users are allowed to log into a server using public key authentication in SSH. In OpenSSH, authorized keys are configured separately for each user, typically in a file called ``authorized_keys``.

### I've added my public ssh key into authorized_keys but I'm still getting a password prompt, what can be wrong?

There could be a permission problem while accessing the authorized_keys file or the public key was not copied correctly. In general, this file should be created with the **ssh-copy-id** command.

### What does ```:(){ :|:& };:``` do on your system?

It's the Bash __fork bomb__ and it is nothing but a bash function which get executed recursively using the **fork** operation. If proper limits are not configured correctly (e.g. ``nproc`` under ``/etc/security/limits.conf``) it would be possible to saturate the resources available on the machine till the point it's not usable anymore until rebooted.

### How do you catch a Linux signal on a script?

In Bash it's possible to do that with the ``trap`` statement, which has the following syntax:

```
trap [COMMANDS] [SIGNALS]
```

This instructs the trap command to catch the listed SIGNALS, which may be signal names with or without the SIG prefix, or signal numbers. If a signal is 0 or EXIT, the COMMANDS are executed when the shell exits. If one of the signals is DEBUG, the list of COMMANDS is executed after every simple command. A signal may also be specified as ERR; in that case COMMANDS are executed each time a simple command exits with a non-zero status.

Reference: [Catching Signals (Bash Guide)](http://tldp.org/LDP/Bash-Beginners-Guide/html/sect_12_02.html)

### Can you catch a SIGKILL?

No, it's not possible: __SIGKILL__ or __SIGSTOP__ signals cannot be caught or ignored. It's instead possible to catch __SIGTERM__.

### What's happening when the Linux kernel is starting the OOM killer and how does it choose which process to kill first?

The Linux kernel allocates memory upon the demand of the applications running on the system. Because many applications allocate their memory up front and often don't utilize the memory allocated, the kernel was designed with the ability to over-commit memory to make memory usage more efficient. This over-commit model allows the kernel to allocate more memory than it actually has physically available. If a process actually utilizes the memory it was allocated, the kernel then provides these resources to the application. When too many applications start utilizing the memory they were allocated, the over-commit model sometimes becomes problematic and the kernel must start killing processes in order to stay operational. The mechanism the kernel uses to recover memory on the system is referred to as the out-of-memory killer or OOM killer for short.

The OOM Killer works by reviewing all running processes and assigning them a score: the process that has the highest score is the one that is killed.Brief list of criteria used by the OOM killer: 

* The process and all of its childs are using a lot of memory.
* The minimum number of processes are killed (ideally one) in order to free up enough memory to resolve the situation.
* Root, Kernel and important system processes are given much lower scores.

Reference: [Configure the Linux OOM Killer](http://www.oracle.com/technetwork/articles/servers-storage-dev/oom-killer-1911807.html)

### Describe the linux boot process with as much detail as possible, starting from when the system is powered on and ending when you get a prompt.

Brief schematic summary:

1. Power On
2. Load BIOS/UEFI for NVRAM
3. Probe for Hardware
4. Select boot device (disk, network, ...)
5. Identify EFI system partition
6. Load Boot Loader (e.g. GRUB)
7. Determine which Kernel to boot
8. Load Kernel
9. Instantiate Kernel data structures
10. Start init/systemd (as PID 1)
11. Execute startup scripts
12. Running system

### What's a chroot jail?

A chroot on Unix operating systems is an operation that changes the apparent root directory for the current running process and its children. A program that is run in such a modified environment cannot name (and therefore normally cannot access) files outside the designated directory tree. The term "chroot" may refer to the chroot(2) system call or the chroot(8) wrapper program. The modified environment is called a chroot jail.

Reference: [Chroot (Wikipedia)](https://en.wikipedia.org/wiki/Chroot)

### When trying to umount a directory it says it's busy, how to find out which PID holds the directory?

It's possibile with **lsof**: `` lsof +d '/some/directory'``

### What's LD_PRELOAD and when it's used?

LD_PRELOAD is an optional environmental variable containing one or more paths to shared libraries, or shared objects, that the loader will load before any other shared library including the C runtime library (libc.so). This operation is called __library preloading__, it means that its functions will be used before others of the same name in later libraries. This enables library functions to be intercepted and replaced (overwritten.) As a result program behavior can be non-invasively modified, i.e. a recompile is not necessary.

References: 
* Man page Linux Dynamic Loader: ``man 8 ld-linux``
* [All about LD_PRELOAD](https://blog.fpmurphy.com/2012/09/all-about-ld_preload.html)
* [LD_PRELOAD Tutorial (Catonmat)](http://www.catonmat.net/blog/simple-ld-preload-tutorial/)

### You ran a binary and nothing happened. How would you debug this?

The best shot would be to run the binary preceded by the strace command: ``strace my_binary`` and looks into the output for some clues (missing configuration files, wrong libraries, etc.).

### What are cgroups? Can you specify a scenario where you could use them?

**C**ontrol **g**roups are a feature of the Linux kernel. cgroups allow you to allocate resources - such as CPU time, system memory, network bandwidth, or combinations of these resources - among hierarchically ordered groups of processes running on a system. By using cgroups, system administrators gain fine-grained control over allocating, prioritizing, denying, managing, and monitoring system resources. Hardware resources can be smartly divided up among applications and users, increasing overall efficiency.

Potential use case scenarios:
* To keep a Web server from using all the memory on a system that's also running a data base.
* To keep a backup system from using too much network I/O bandwidth and crashing the business apps running on the same system.
* To allocate system resources among user groups of different priority (the faculty, staff and students of a university, for instance).

Reference: [Resource Management Guide from RedHat](https://access.redhat.com/documentation/en-us/red_hat_enterprise_linux/7/html/resource_management_guide/)

### How can you remove/delete a file with file-name consisting of only non-printable/non-type-able characters?

There are different methods:

1. Put filenames in quotes.
2. Insert a backslash before the special character.
3. Try a ``./`` at the beginning of the filename.
4. Try a ``--`` at the beginning of the filename (it will signal ``rm`` to disable further option processing).
5. Remove file by an inode number.

Reference: [Remove files with names that contains spaces and special characters (Linux.com blog)](https://www.linux.com/blog/linux-shell-tip-remove-files-names-contains-spaces-and-special-characters-such)

### How can you increase or decrease the priority of a process in Linux?

Priority (or niceness) of a process can be increased or decreased using the **nice** command from the GNU coreutils. Niceness values range from **-20** (most favorable to the process) to **19** (least favorable to the process). The niceness value of a process can be checked with the ``ps`` command (column **NI**). By default, a process started by a user has always niceness set to zero and only root can set it up to a negative number. In case it is necessary to modify the niceness of an already running process the command **renice** can be used. Additionally, there's also the **ionice** command (part of util-linux) which sets or gets the I/O scheduling class and priority for a program. Note that ionice only works with the CFQ disk scheduler but it's not able to affect RAID or LVM disks.

References:
* Man pages: ``man nice``; ``man renice``; ``man ionice``
* [Process execution priorities (IBM DeveloperWorks Library)](https://www.ibm.com/developerworks/library/l-lpic1-103-6/index.html)
* [Some notes on Linux's ionice](https://utcc.utoronto.ca/~cks/space/blog/linux/IoniceNotes)

#### [[⬆]](#toc) <a name='expert'>Expert Linux Questions:</a>

### A running process gets EAGAIN: Resource temporarily unavailable on reading a socket. How can you close this bad socket/file descriptor without killing the process?

1. Get the PID of the process with ``pidof`` or ``pgrep``:
```bash
>>> pidof my_process
3466
```

2. To get a list of the file descriptors related to the process: 
```bash
>>> ls -la /proc/3466/fd
total 0
dr-x------ 2 user group  0 Jun 27 10:35 .
dr-xr-xr-x 9 user group  0 Jun 27 10:35 ..
lrwx------ 1 user group 64 Jun 27 10:35 0 -> /dev/pts/3
lrwx------ 1 user group 64 Jun 27 10:35 1 -> /dev/pts/3
lrwx------ 1 user group 64 Jun 27 10:35 2 -> /dev/pts/3
lrwx------ 1 user group 64 Jun 27 10:35 3 -> socket:[35799042]
lrwx------ 1 user group 64 Jun 27 10:35 4 -> socket:[35799047]
lrwx------ 1 user group 64 Jun 27 10:35 5 -> /dev/pts/3
lrwx------ 1 user group 64 Jun 27 10:35 6 -> /dev/pts/3
lrwx------ 1 user group 64 Jun 27 10:35 7 -> /dev/pts/3
```

3. Removing open sockets or interacting with them from outside of the process is not possible, so it is necessary to use an additional program like **GDB**:
```bash
>>> gdb
[...]
>>> (gdb) attach 3466
[...]
>>> p close(4)
$1 = 0
>>> (gdb) detach
Detaching from program: /usr/bin/my_process, process 3466
>>> (gdb) quit
```

Reference: [GNU libC: Error Codes](https://www.gnu.org/software/libc/manual/html_node/Error-Codes.html)

#### [[⬆]](#toc) <a name='network'>Networking Questions:</a>

### What is localhost and why would ping localhost fail?

In computer networking, __localhost__ is a hostname that means "this computer". It is used to access the network services that are running on the host via the loopback network interface. Using the loopback interface bypasses any local network interface hardware. The name localhost is reserved for loopback purposes by RFC 6761 (Special-Use Domain Names).

If pinging localhost reports the following error:
```bash
>>> ping localhost
ping: unknown host localhost
```

The following steps may help to identify and solve the prolem:

1. Check the contents of ``/etc/hosts``. It should contain something like this:
```bash
127.0.0.1	localhost

::1     ip6-localhost ip6-loopback
```

2. In case the problem persist, it is necessary to check if ``/etc/nsswitch.conf`` contains the following entry:
```bash
[...]
hosts:   files dns
[...]
```

It would be also useful to verify that the loopback interface is up:
```bash
>>> ip link show lo
1: lo: <LOOPBACK,UP,LOWER_UP> mtu 65536 qdisc noqueue state UNKNOWN mode DEFAULT group default qlen 1
    link/loopback 00:00:00:00:00:00 brd 00:00:00:00:00:00
```

Reference: [Localhost (Wikipedia)](https://en.wikipedia.org/wiki/Localhost)

### What is the similarity between "ping" & "traceroute" ? How is traceroute able to find the hops.

When data is sent over the Internet, it's sent in small blocks of data, called packets. Messages are divided into packets before they are sent, and each packet is then transmitted individually and can even follow different routes to its destination. Once all the packets forming a message arrive at the destination, they are recompiled into the original message.

In order to debug timeouts during a connection to a remote server the utilities **ping** and **traceroute** will help to investigate such issues.

__Ping__ measures the round-trip time for messages sent from the originating host to a destination computer that are echoed back to the source. It  operates by sending Internet Control Message Protocol (ICMP) echo request packets to the target host and waiting for an ICMP echo reply. The program reports errors, packet loss, and a statistical summary of the results, typically including the minimum, maximum, the mean round-trip times, and standard deviation of the mean.

__Traceroute__ is a tool for displaying the route (path) and measuring transit delays of packets across an Internet Protocol (IP) network. The history of the route is recorded as the round-trip times of the packets received from each successive host (remote node) in the route (path); the sum of the mean times in each hop is a measure of the total time spent to establish the connection. Traceroute proceeds unless all (three) sent packets are lost more than twice, then the connection is lost and the route cannot be evaluated. Ping, on the other hand, only computes the final round-trip times from the destination point.

Whilte traceroute (by default) uses UDP packets (on Unix-like systems), it can also uses ICMP and in that it is similar to ping.

References:
* [Traceroute (Wikipedia)](https://en.wikipedia.org/wiki/Traceroute)
* [Ping (Wikipedia)](https://en.wikipedia.org/wiki/Ping_(networking_utility))

### What is the command used to show all open ports and/or socket connections on a machine?

**netstat** can be used for this task: ``netstat -an`` (``-a``, shows both listening and non-listening sockets; ``-n``, show numerical addresses instead of trying to determine symbolic host, port or user names).

### Is 300.168.0.123 a valid IPv4 address?

No, it's not valid. IPv4 addresses may be represented in any notation expressing a 32-bit integer value. In this form, the IP address must be expressed with a maximum of 4 bytes which means that any number part of the address itself has to be limited to the single byte range: 2^8 or 0-255 possible values.

### Which IP ranges/subnets are "private" or "non-routable" (RFC 1918)?

Name         |CIDR block       |Address range                  |Number of addresses | Classful description
-------------|-----------------|-------------------------------|--------------------|-----------------------
24-bit block | 10.0.0.0/8      | 10.0.0.0 – 10.255.255.255     | 16.777.216         | Single Class A.
20-bit block | 172.16.0.0/12   | 172.16.0.0 – 172.31.255.255   | 1.048.576          | Contiguous range of 16 Class B blocks.
16-bit block | 192.168.0.0/16  | 192.168.0.0 – 192.168.255.255 | 65536	            | Contiguous range of 256 Class C blocks.

Reference: [IPv4 (Wikipedia)](https://en.wikipedia.org/wiki/IPv4)

### What is a VLAN?

VLANs permit you to subdivide an Ethernet switch into logically disparate virtual Ethernet switches. This allows a single Ethernet switch to act as though it's multiple physical Ethernet switches. VLANs work by applying tags to network packets and handling these tags in networking systems – creating the appearance and functionality of network traffic that is physically on a single network but acts as if it is split between separate networks. In this way, VLANs can keep network applications separate despite being connected to the same physical network, and without requiring multiple sets of cabling and networking devices to be deployed.

References:
* [VLAN (Wikipedia)](https://en.wikipedia.org/wiki/Virtual_LAN)
* [How do VLANs work? (ServerFault)](https://serverfault.com/questions/188350/how-do-vlans-work)

### What is ARP and what is it used for?

Address Resolution Protocol (ARP) exists solely to glue together the IP and Ethernet networking layers. Since networking hardware such as switches, hubs, and bridges operate on Ethernet frames, they are unaware of the higher layer data carried by these frames [9]. Similarly, IP layer devices, operating on IP packets need to be able to transmit their IP data on Ethernets. ARP defines the conversation by which IP capable hosts can exchange mappings of their Ethernet and IP addressing.

ARP is used to locate the Ethernet address associated with a desired IP address. When a machine has a packet bound for another IP on a locally connected Ethernet network, it will send a broadcast Ethernet frame containing an ARP request onto the Ethernet. All machines with the same Ethernet broadcast address will receive this packet [10]. If a machine receives the ARP request and it hosts the IP requested, it will respond with the link layer address on which it will receive packets for that IP address. N.B., the arp_filter sysctl will alter this behaviour somewhat.

Once the requestor receives the response packet, it associates the MAC address and the IP address. This information is stored in the arp cache. The arp cache can be manipulated with the ``ip neighbor`` and ``arp`` commands.

Reference: [Guide to IP Layer Network Administration with Linux](http://linux-ip.net/html/ether-arp.html)

### What is the difference between TCP and UDP?

**TCP**

* Reliability: TCP is connection-oriented protocol. When a file or message send it will get delivered unless connections fails. If connection lost, the server will request the lost part. There is no corruption while transferring a message.	
* Ordered: If you send two messages along a connection, one after the other, you know the first message will get there first. You don’t have to worry about data arriving in the wrong order.	
* Heavyweight: when the low level parts of the TCP "stream" arrive in the wrong order, resend requests have to be sent, and all the out of sequence parts have to be put back together, so requires a bit of work to piece together.	
* Streaming: Data is read as a "stream", with nothing distinguishing where one packet ends and another begins. There may be multiple packets per read call.	
* Examples: World Wide Web (Apache TCP port 80), e-mail (SMTP TCP port 25 Postfix MTA), File Transfer Protocol (FTP port 21) and Secure Shell (OpenSSH port 22) etc.

**UDP**

* Reliability: UDP is a connectionless protocol. When you a send a data or message, you don't know if it'll get there, it could get lost on the way. There may be corruption while transferring a message.
* Ordered: If you send two messages out, you don't know what order they'll arrive in i.e. no ordered.
* Lightweight: No ordering of messages, no tracking connections, etc. It's just fire and forget! This means it's a lot quicker, and the network card / OS have to do very little work to translate the data back from the packets.
* Datagrams: Packets are sent individually and are guaranteed to be whole if they arrive. One packet per one read call.
* Examples: Domain Name System (DNS UDP port 53), streaming media applications such as IPTV or movies, Voice over IP (VoIP), Trivial File Transfer Protocol (TFTP) and online multiplayer games etc.

Reference: [Internet Protocol suite (Wikipedia)](https://en.wikipedia.org/wiki/Internet_protocol_suite)

### What is the purpose of a default gateway?

A gateway is a network node that serves as an access point to another network, often involving not only a change of addressing, but also a different networking technology. More narrowly defined, a router merely forwards packets between networks with different network prefixes. The networking software stack of each computer contains a routing table that specifies which interface is used for transmission and which router on the network is responsible for forwarding to a specific set of addresses. If none of these forwarding rules is appropriate for a given destination address, the __default gateway__ is chosen as the router of last resort.

References:
* [Gateways (Linux Network Admin Guide)](http://tldp.org/LDP/nag/node30.html#SECTION004430000)
* [Default Gateway (Wikipedia)](https://en.wikipedia.org/wiki/Default_gateway)

### What is the command used to show the routing table on a Linux box?

On modern Linux distributions, the ``ip`` command can be used for this task:
```bash
>>> ip route
192.168.99.0/24 dev eth0  scope link 
127.0.0.0/8 dev lo  scope link 
default via 192.168.99.254 dev eth0
```

Older Linux distributions usually supports the following two commands: ``netstat -rn`` or ``rounte -n``.

### A TCP connection on a network can be uniquely defined by 4 things. What are those things?

The TCP layer on either sides maintains table entries corresponding to the 4-tuple: remote-ip-address, remote-port, source-ip-address, source-port. This 4-tuple uniquely identifies a connection.

### When a client running a web browser connects to a web server, what is the source port and what is the destination port of the connection?

### You have added an IPv4 and IPv6 address to interface eth0. A ping to the v4 address is working but a ping to the v6 address gives you the response sendmsg: operation not permitted. What could be wrong?

### What is SNAT and when should it be used?

__Source Network Address Translation__ (SNAT), usually implemented in Linux with iptables, let the network stack to rewrite the Source IP address in the IP header of the packet. This is necessary, for example, when several hosts have to share an Internet connection. We can then turn on ip forwarding in the kernel, and write an SNAT rule which will translate all packets going out from our local network to the source IP of our own Internet connection. Without doing this, the outside world would not know where to send reply packets, since our local networks mostly use the IANA specified IP addresses which are allocated for LAN networks. If we forwarded these packets as is, no one on the Internet would know that they were actually from us. The SNAT target does all the translation needed to do this kind of work, letting all packets leaving our LAN look as if they came from a single host, which would be our firewall.

### Explain how could you ssh login into a Linux system that DROPs all new incoming packets using a SSH tunnel.

### How do you stop a DDoS attack?

### How can you see the content of an IP packet?

1. ``tcpdump -i eth0 -s0 -n -w /tmp/capture port some_port &``
2. ``tcpdump -r /tmp/capture -A``

### What is IPoAC (RFC 1149)?

[IP Datagrams on Avian Carriers](https://tools.ietf.org/html/rfc1149)

