# B2BR NOTES

## Why Debian?

There are a lot of reasons to chose `Debian` as my operating system as

- A User
- A Developer
- Even in etreprise envirenments
- is a free Software
- is stable and secure
- has extensive Hardware Support
- offers flexible installation
- huge number of software packages
- long term support

It is also widely used as a server operating system because it is secure, stable
and reliable and comes with frequent and regular updates.

## What is the Difference between `GB` and `GiB`

- GB is a gigabyte, `1GB --> 1_000_000_000 bytes`
- GiB is a gibbyte, `1GiB --> 1_073_741_824 bytes`

## Linux Phoilosophy

Linux flollows five core principles:

- `Everything is a file`
- `Small, single-purpose programs`
- `Ability to chain programs together to perform complex tasks`
- `Avoid captive user interfaces`
- `Configuration data stroed in a text file`

## File System in Unix

| Path     | Description                                                                                                                                                                                                                                                           |     |     |
| -------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | --- | --- |
| `/`      | The top-level directory is the root filesystem and contains all of the files required to boot the operating system, before other filesystems are mounted. After Boot all of the other filesystems are mounted at standard mount points as subdirectories of the root. |     |     |
| `/bin`   | Contains essential command binaries                                                                                                                                                                                                                                   |     |     |
| `/boot`  | Consists of the static Bootloader, kernel executable, and files required to boot the OS                                                                                                                                                                               |     |     |
| `/dev`   | Contains device files to facilitate access to every hardware device attached to the system                                                                                                                                                                            |     |     |
| `/etc`   | Local system Configuration files. Configuration files for installed applications may be saved here as well.                                                                                                                                                           |     |     |
| `/home`  | Each user on the system has a subdirectorie here for storage.                                                                                                                                                                                                         |     |     |
| `/lib`   | Shared library files that are required for system boot.                                                                                                                                                                                                               |     |     |
| `/media` | External removable media devices such a usb drives are mounted here.                                                                                                                                                                                                  |     |     |
| `/mnt`   | Temporary mount point for regular filesystems.                                                                                                                                                                                                                        |     |     |
| `/opt`   | Optional files such as third-party tools can be saved here.                                                                                                                                                                                                           |     |     |
| `/root`  | The home directory for the root user.                                                                                                                                                                                                                                 |     |     |
| `/sbin`  | This directory contains executables used for system administration                                                                                                                                                                                                    |     |     |
| `/tmp`   | The operating system and many programs use the this directory to store temporary files. This directory is generally cleared upon system boot and may be deleted at other times without any warning.                                                                   |     |     |
| `/usr`   | Contains executables, libraries, man files, etc.                                                                                                                                                                                                                      |     |     |
| `/var`   | This directory contains variable data files such as log files.                                                                                                                                                                                                        |     |     |

## Debian package Manager

Debian uses an Advanced Package Tool `apt`

`apt` is a package management system to handle sftware updates and security patches.

### What is the difference between `apt` and `aptitude`

`apt` and `aptitude` both are package managers, first one offers a command-line interface,
While the other offers user interface

## What is SSH: Secure Shell

SSH stands for Secure Shell, and it’s a protocol that allows
you to connect to a remote computer securely over an unsecured network. SSH provides a secure channel between two computers, ensuring that data transferred between them is encrypted and protected from attackers.

In simple terms, SSH lets you log in to another computer (like a server) over the internet
and control it just like you would if you were physically in front of it.

### how to configure ssh

1. first go to the `sshd_config` which is the ssh configuration file

```bash
sudo vim /etc/ssh/sshd_config

```

1. Locate the Port Directive. Find the line that starts with Port. It should say Port 22 by default.
2. Change the Port Number. Edit the line to reflect your desired port number.
3. Save and close the file
4. Add port to `ufw`
5. restart ssh using this command

```bash
sudo systemctl restart sshd

```

## LVM

**LVM**: Logical Volume Manager

An alternative method of managin storage systems than the traditional partition-based

IN **LVM** instead of creating partitions, you create logical volumes.

> [!info] Exception
> One exception is that you can not use logical volumes for `boot`, That because
> `GRUP` (boot loader for Linux) cant't read from logical volumes

### Components of LVM

There are three main components to `LVM`

1. Physical Volumes: Physical Hard Drive
2. Volume Groups: A group that i can manage the physical Volumes with
3. Logical Volumes: is a virtual partition created by the logica volume manager, Created within a volume group

### Why use LVM

The main advantage of `LVM` is how easy it is to **resize a logica volume** or volume group.

## SUDO: Super User Do

**sudo** is a program for unix computers that enables users to run programs with the security previleges of another user,
by default the _superuser_

`sudo` uses the current user's password in default config, `sudo` stands for `super user do`.

`su` uses the password of the user you're trying to run as or switch to, `su` stands for `substitute user`.
By default the user account you switch to is the root account but it can be any account on the system.

For example

```bash
su - # switching to root and you need the root password
```

The `-` switch provides you with root's envirenments _path and shell variables_

```bash
su zajaddad # switching to zajaddad and you need the zajaddad's password
```

To summarize:

- `sudo` allows you to execute a command as another user with the `-u` asking for the given user password,
  by default runs the commands as the **root**, will work if the current user in the sudoers group

- `su` substitute user identity allows the user to switch to other user using the password of the last.

### Sudo Configuration

Sudo's configuration file is located in `/etc/sudoers` , and should **always** be edited with the `visudo`
using `visudo` it ensures that there is no syntax errors

To see `sudo` settings use `sudo -ll` or `sudo -lU user` to a specific user.

To allow a user to gain full root privileges when they precede a command with `sudo`, add the following line:

```bash
USER_NAME   ALL=(ALL:ALL) ALL
```

To Add strong configuration to our sudo group we need to create `/etc/sudoers.d/sudo_config`

`sudo_config` should look like this:

```bash
Default passwd_tries=3 # Authentication using sudo to be limited to 3 attempts
Default badpass_message="Sorry, Try again." # Custom message to be displayed  in wrong password
Default logfile="/var/log/sudo/sudo_config" # Path to configuration file
Default log_input, log_output # TTY enabled
Default iolog_dir="/var/log/sudo" # TTY enabled
Default secure_path="/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin:/snap/bin" # Paths that can be used by sudo
```

## UFW: Uncomplicated Firewall

A firewall is a piece of software that protects and network of a PC/Server from unauthorized access
The most basic firewalls look that port traffic coming to a device and step usolicited traffic, any traffic that
is trying to access a port that shouldn't be accessible.

More advanced Firewalls are called _NGFWs Next-Gen Firewall_ or _WAFs Web Application Firewall_ that actively
look at packets and traffic looking for signs of malicious activity and stop it before it goes to a server.

This is just a simple definition of firewalls, in our case we use `ufw`

### ufw configuration

```bash
sudo ufw enable # To Enable ufw
sudo ufw status verbose # To check ufw status
```

Now to configure `ufw` to allow connections through port 4242, run the command

```bash
ufw allow 4242 # Ensures that traffic on port 4242 is permitted
```

## User

The subject requests that a user with the login of the student being evaluated is present
on the virtual machine. and it has been added and that it belongs to the `sudo` and `user42` groups.

### Add User To Groups

1. Create a user

```bash
sudo useradd username
```

2. Create The `user42` group

```bash
sudo groupadd group_name
```

3. Add user to group

```bash
sudo usermode -aG group_name username
```

4. To verify user's group membership

```bash
sudo groups username
```

## Password Policy Settings

Edite login.defs located at `/etc/login.defs`

located and Change Variables

- PASS_MAX_DAYS 99999 -> PASS_MAX_DAYS 30
- PASS_MIN_DAYS 0 -> PASS_MIN_DAYS 2

### PAM For more secure password

**PAM**: Linux Pluggable Authentication Modules
**PAM** is a suite of libraries that allow a Linux system administrator to configure methods to authenticate users

To install it run `sudo apt-get install libpam-qwquality -y`

configuration file is located at `/etc/pam.d/common-password`

Locate `retry=3` and add

```bash
minlen=10           # Sets minimum lenght to be 10
ucredit=-1          # At least 1 uppercase character
dcredit=-1          # At least 1 digit
lcredit=-1          # At least 1 lowercase
maxrepeat=3         # Limits the repetition of the characters to 3
reject_username     # Password without username in it
difok=7             # New Password should'n have 7 characters from the previous password
enforce_for_root    # All command would be enforced for the root also
```
