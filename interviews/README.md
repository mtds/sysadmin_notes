# Sysadmin Interview Questions

The questions reported here comes from the [Linux Sysadmin Interview](https://github.com/chassing/linux-sysadmin-interview-questions) repository published on Github.

I took the liberty to reorganize or remove some of the questions, whenever I deemed it necessary.
 
## <a name='toc'>Table of Contents</a>

  1. [Simple Linux Questions](#simple)

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

