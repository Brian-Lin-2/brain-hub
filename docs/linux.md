# Linux

## sudo

When a Linux user uses sudo, they automatically gain root level access temporarily for that one command.

## cronie

Popular Linux package for creating cron jobs

## SSH

Secure Shell is a network protocol that lets users connect to a remote computer over an unsecured network. It accomplishes this by providing encrypted communication from your local terminal to the remote computer.

### Configuration

`ssh_config` - This applies only to the specific user's config file
`sshd_config` - This applies to the entire server itself. Whatever you modify here, will affect every user

### RSA Keys

A common method for SSH is to set up RSA keys to allow for ease of access from a user's local laptop to the SSH server

`ssh-keygen -t rsa -b 4096 -C <EMAIL>` - This helps generate asymmetric keys (`id_rsa` is your private, `id_rsa.pub` is your public)
`ssh-copy-id user@remote-server` - This automatically copies the public RSA key you made for your laptop to the remote server

## Systemd

In Linux, `systemd` is the system and service manager fo Linux. It's responsible for everything in Linux. `systemctl` is the CLI tool that lets you control `systemd`

## SELinux

Security-Enhanced Linux is a security architecture built directly onto the Linux kernel. It's a strict, low-level security guard that dictates what processes can do.

It introduces `MAC (Mandatory Access Control)`, which makes the system-wide security policy block everything regardless of the user permissions.

SELinux is popular in industry to put a hard lock on Linux VMs and ensure that even if they're compromised, the hacker can't do any significant damage.
