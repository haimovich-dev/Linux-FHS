# Linux-FHS
In this repository I will learn the Linux Filesystem Hierarchy Standard to enhance my troubleshooting skills

## Preface
The FHS (Filesystem Hierarchy Standard) was made for some reasons which are:
- Make it easier for software developers to set the used directories by the app, this partly implements the cross-platform concept for different linux distros.
- Make it easier for Administrators/Users to navigate between different systems, this gives easier migration or learning different linux distros

## `/`
This is the root directory, it is the top level directory which identifies the mounted partition where the OS is installed. every other directory under this one is like a branch of a tree when the root directory is the tree trunk.

## `/bin`
This directory stores binaries
- Stores binaries which are more known as 'commands' that are used by the administrator or the user.
- The binaries that are stored there can be used when the OS is booted in a single-user mode.
- The binaries also can be used as a part of a script.
- No other directories should be in the /bin directory, avoid creating directories unde /bin like /bin/sub-directory. it is against the conventions.

## `/boot`
This directory has all the necessary files required by the booting process of the system, you will find there the `grub` and the `efi/bios(depends on the hardware)`. The contents in this directory will difer depending on the distro you are using, for example in Ubuntu there is no dedicated folder for booting entries, but in RedHat there is a sub-directory `/boot/loader/entries` that contains .conf files when each file represents an entry.

But the convention is that the `/boot` directory will have all the required file by the bootloader to load the OS, for example every `/boot` directory will have the kernel which is named as `vmlinuz` which is the kernel itself.

Also the configuration files are stores under `/etc`, and binaries that are necessary to load files for the bootloader are stored under `/sbin`

## `/dev`
This directory has all the devices that are connected to the machine, also has special files.

Nothing special for this directory, when you inject a usb-drive for example into your computer you will see it under /dev. This directory will have links, directories, block devices, and character devices.

## `/etc`
A known directory, probably used on the first stages on learning linux. This directory contains static configuration files that define how applications will operate, for example if you have nginx installed then the configurations of this programm will be stored under `/etc`, even though it is conventional to store configuration files right under `/etc` it is highly recommended to add a sub-directory for each application to keep it order.

## `/home`
This directory contains the users directories, its said that no programm should rely on that because it will differ between systems, instead the programm should query for the users home directory.

And I can understand that, because when you create a user you can specify a different home directory on creation.

## `/lib`
This directory stores libraries that are used by the binaries that are stored in `/bin` and `/sbin`, and also it stores libraries that are required for the boot process. for example if a machine has SSH installed then ssh would reqire the library libcrypto to support different encryption algorithms to generate private/public keys, encrypt traffic and other things, so the library libcrypto will reside under `/lib` in my case it was under `/lib/x86_64-linux-gnu`.

## `/media`
This directory is used aa a starting point for mounted removable devices like flash-drives, cdroms, etc. Nothing special just know that when mounting some device mount it into the `/media` directory.

## `/mnt`
Mostly like the `/media` directory just for temporary filesystems used by administrators.

## `/opt`
In this directory you install third-party software or just some other software that is not related to the system, for example you decide to test another version of a software you already have, lets take OpenSSH for example, APT on Ubuntu 24.04.2 supports the version 9.6p1 which has some CVEs, so before upgrading manually to 10.0p2 I want to install it isolated and test if it works, so I will create `/opt/openssh-10-0-p2` and install it there.

I have encountered this problem in my previous project "SSH-Hardening" when I tried to install the dependent libraries I strugled on understanding the location of those libraries.

## `/root`
Just the home directory for the root user

## `/run`
In this directory programms store their information since the system has booted, and on the next boot these files are cleared. If a programm has more then one file for logs, it's required to create a sub-directory for its application under `/run`. and older version for this convention was `/var/run` it may still be used but cannot be used simultaneously with each other.
Under this directory you can find the .pid files which describe the process ID since the system has started.

## `/sbin`
This directory is similar to the `/bin` but in `/sbin` only administrative binaries are stored, or binaries that are used only with root privileges, In this directory nos sub-directories should exist, and for some reason it is stated that `/sbin` was not made due to security reasons, only to organize the binaries.

## `/srv`
As of what I undestood the `/srv` directory stands for service and this directory contains data that is served by different services. For example a web server provides a services data that is stored under `/srv/web` for example images, html files, etc.

## `/tmp`
Stands for temporary, if any program uses temporary files, they should be stored under `/tmp` and not make any assumptions that any file exists in this directory.

## `/usr`
This one is a big section, the legacy reason for that directory existance is to provide a location for read only data for users, different binaries that are used by users were stored under `/usr/bin` the same with `/usr/lib` and `/usr/sbin`. back then `/usr` was not mounted in the booting process so there was a need in storing binaries in the `/bin` directory to troubleshoot the machine in a single user mode and keep it minimal.

As the time passed, developers decided to migrate to `/usr` and mount it while boot with the help of 'initramfs' that allowed to load the `/usr` early while the booting process in a safe way, and avoid duplications because the same binary could reside under `/bin` and `/usr/bin`.

In addition for everything said above, there is a standard that defines the need is the next sub-directories:
- `/usr/bin` : most user commands are stored in this location
- `/usr/lib` : system libraries
- `/usr/local` : additional local FHS hierarchy
- `/usr/sbin` : binaries used by the system that are not must have
- `/usr/share` : files that are not dependant on the machines CPU architectures like PDF files for example.

There are more sub-directories in `/usr` and most of them optional
- `/usr/games` : just remove it
- `/usr/include` : this one is a must have, contains headers for C programms.
- `/usr/src` : stores the source code of any given software
- `/usr/libexec` : something irrelevant
- `/usr/lib[qual]` : you could also see it as a link under `/` and this directory contains libraries dependant on the architecture like 64 or 32.

And finally the links like `/sbin > /usr/sbin` were left there to maintain legacy software and scripts that were using `/bin`/`/sbin`/`/lib` in their code.

## `/var`




# Bibliography
The entire documentation was taken from [Linux Foundation](https://refspecs.linuxfoundation.org/FHS_3.0/fhs/index.html)
