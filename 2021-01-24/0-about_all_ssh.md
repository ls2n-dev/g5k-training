# OpenSSH

## What is SSH?

OpenSSH is a powerful collection of tools for the remote control of, and transfer of data between, networked computers. 

OpenSSH is a freely available version of the Secure Shell (SSH) protocol family of tools for remotely controlling, or transferring files between, computers.

Traditional tools used to accomplish these functions, such as telnet or rcp, are insecure and transmit the user’s password in cleartext when used. OpenSSH is a network protocol for operating networking services securely over a network (like internet). It uses encryption standards to securely connect and login to the remote system.

OpenSSH provides a server daemon and client tools to facilitate secure, encrypted remote control and file transfer operations, effectively replacing the legacy tools.

OpenSSH stores a public key in the remote system and private key in the client system. Thes keys are produced as a pair mathematically. When both are applied to a bi-variable function, it will result in a value which will be used to check whether the pair is valid or invalid. 

## Get start with SSH

Source at https://www.openssh.com

### MacOSX

Install with [`brew`](https://brew.sh) by just typing `brew install openssh`

### Linux

Use your favorite installer (`apt` `yum` `dpkg` `emerge` etc.) and find the package related to the keyword `openssh` or `ssh`.

- On [Ubuntu](https://ubuntu.com/server/docs/service-openssh) or [Debian](https://wiki.debian.org/SSH), `apt install openssh-server`
- On Gentoo, `emerge net-misc/openssh`
- On Centos, `yum install openssh-server`

### Windows
Follow only one of the listed installation:
- [Official installation](https://docs.microsoft.com/en-us/windows-server/administration/openssh/openssh_install_firstuse)
- [PowerShell/openssh-portable](https://github.com/PowerShell/OpenSSH-Portable)
- [Putty Client](https://www.puttysshclient.com/) 

You can use [MobaXterm](https://mobaxterm.mobatek.net/) to emulate a Linux Shell Terminal.

## Generate a new SSH Public and Private Key pair

With SSH, you can run commands on remote computers and servers, send files, and generally manage everything you do from one place. When you are working with multiple SSH servers in multiple locations, or if you are just trying to save some time accessing these servers, you'll want to use an SSH public and private key pair. Key pairs basically make logging into remote machines and running commands easier. To do so, you will use the command [`ssh-keygen`](https://www.ssh.com/academy/ssh/keygen?hsLang=en)

```bash
# ssh-keygen -t rsa
Generating public/private rsa key pair.
Enter file in which to save the key (/path/to/your/.ssh/id_rsa): /path/where/you/want/to/save/your/id_rsa
Enter passphrase (empty for no passphrase):
Enter same passphrase again:
Your identification has been saved in /path/where/you/want/to/save/your/id_rsa.
Your public key has been saved in /path/where/you/want/to/save/your/id_rsa.pub.
The key fingerprint is:
SHA256:m34zcBpL5og96ViEujre+xxxxxxxxxxBVv151OtzrOM 
The key's randomart image is:
+---[RSA 3072]----+
|                 |
|          .     .|
|   .     . .   ..|
|  . ..  o   . o .|
|   o...oS.   o o |
|  ..o.o *oo   o. |
| ..o +.BoB     oo|
|. +o.o*.+ +   ..o|
|o=++*+.... o .E. |
+----[SHA256]-----+
```

- check the number of bits
  ```bash
  ssh-keygen -lf <id_rsa>.pub
  ```

## References read for you
- [SSH tutorial related to Grid'5000](https://www.grid5000.fr/w/SSH)
- [SSH Essentials: Working with SSH Servers, Clients, and Keys](https://www.digitalocean.com/community/tutorials/ssh-essentials-working-with-ssh-servers-clients-and-keys)
- [Understand ssh-keygen](https://www.ssh.com/academy/ssh/keygen?hsLang=en)
- [Understand keys in SSH](https://www.ssh.com/academy/ssh/key)
- [SSH.com Academy about SSH](https://www.ssh.com/academy/ssh)
- Strong passwords recommended by
  - [ANSSI helps you to strenghten your password](https://www.ssi.gouv.fr/administration/precautions-elementaires/calculer-la-force-dun-mot-de-passe) (in french)
  - [CERN](https://security.web.cern.ch/recommendations/en/passwords.shtml) ([Password alternatives](https://security.web.cern.ch/recommendations/en/password_alternatives.shtml))
