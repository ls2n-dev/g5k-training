# OpenSSH

## What is SSH?

OpenSSH is a powerful collection of tools for the remote control of, and transfer of data between, networked computers. 

OpenSSH is a freely available version of the Secure Shell (SSH) protocol family of tools for remotely controlling, or transferring files between, computers.

Traditional tools used to accomplish these functions, such as telnet or rcp, are insecure and transmit the user’s password in cleartext when used. OpenSSH is a network protocol for operating networking services securely over a network (like internet). It uses encryption standards to securely connect and login to the remote system.

OpenSSH stores a public key in the remote system and private key in the client system. Thes keys are produced as a pair mathematically. When both are applied to a bi-variable function, it will result in a value which will be used to check whether the pair is valid or invalid. 

The SSH protocol uses encryption to facilitate secure the connection between a client and a server. All user authentication, commands, output, and file transfers are encrypted to protect against attacks in the network and effectively replacing the legacy tools.

![image](https://www.ssh.com/hubfs/Imported_Blog_Media/SSH_simplified_protocol_diagram-2.png)

## Get start with SSH
Source at https://www.openssh.com

## Windows

Modern versions of Windows have SSH available in Powershell. You can test if it is available by typing ssh --help in Powershell. If it is installed, you should see some useful output. If it is not installed, you will get an error. If SSH is not available in Powershell, then you should install MobaXterm as described below.

An alternative is to install MobaXterm from http://mobaxterm.mobatek.net. You will want to get the Home edition (Installer edition). However, if Git Bash works, you do not need this.

Follow only one of the listed installation:
- [Official installation](https://docs.microsoft.com/en-us/windows-server/administration/openssh/openssh_install_firstuse)
- [PowerShell/openssh-portable](https://github.com/PowerShell/OpenSSH-Portable)
- [Putty Client](https://www.puttysshclient.com/) 

### MacOSX

It should come with SSH pre-installed, so you should not need to install anything otherwise install with [`brew`](https://brew.sh) by just typing `brew install openssh`

### Linux

Linux users do not need to install anything, you should be set! Otherwise use your favorite installer (`apt` `yum` `dpkg` `emerge` etc.) and find the package related to the keyword `openssh` or `ssh`.

- On [Ubuntu](https://ubuntu.com/server/docs/service-openssh) or [Debian](https://wiki.debian.org/SSH), `apt install openssh-server`
- On Gentoo, `emerge net-misc/openssh`
- On Centos, `yum install openssh-server`

### G5k WebSSH

And if you are not admin of your laptop or didn't manage to have a SSH Client, just go and connect with the g5k webssh portal of the cluster site, here Nantes at https://intranet.grid5000.fr/shell/nantes/ (Only your g5k login and password authentication method will work.)

## Generate a new SSH Public and Private Key pair

With SSH, you can run commands on remote computers and servers, send files, and generally manage everything you do from one place. When you are working with multiple SSH servers in multiple locations, or if you are just trying to save some time accessing these servers, you'll want to use an SSH public and private key pair. Key pairs basically make logging into remote machines and running commands easier. To do so, you will use the command [`ssh-keygen`](https://www.ssh.com/academy/ssh/keygen?hsLang=en)

We strongly suggest the use of `rsa` cipher at least 2048 bits or better 4096 bits or `ed25519` keys, avoid the use of `DSA` nor `ECDSA` keys. Legacy `RSA` keys are accepted and the use of a strong passphrase is highly recommended.

```bash
# ssh-keygen -t rsa [ </path/where/you/want/to/save/your/id_rsa> ]
Generating public/private rsa key pair.
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

- How to change your passphrase without changing the private key
  ```
  ssh-keygen -f <ssh_private_key> -p
  ```
  
- check the number of bits
  ```bash
  ssh-keygen -lf <ssh_public_key>.pub
  ```

**Exercise:** Create a ed25519 key pair and tell me how many bits are used by default ?
<details><summary>Answer</summary>
<p>
```bash
ssh-keygen -t ed25519 -f foo
256 SHA256:1V2QK6MREVLFpdIq6tAtD2hZoKMRI6m/qIIpVG+MHAc randria@rr-ls2n-mbp-2.home (ED25519)
```
</p>
</details>

## Grid'5000 Credentials
An email is sent automatically to the new user by the Grid'5000 User Management Service with instructions to follow to create a new password and a SSH Public Key with a one-shot link. The URL will show the page below :

![image](https://user-images.githubusercontent.com/2065392/150173872-75d62e89-623f-48dc-bacc-aa4676cad2fe.png)

You need to :
- create a strong password
- cut and paste your new or existing SSH Public Key that you will use to connect to the Grid'5000 frontend portal.

### Add new keys
- Do not EDIT manually the `~/.ssh/authorized_keys` in your g5k session
- Use instead the User Management Portal : https://api.grid5000.fr/stable/users/

## References read for you
- [SSH tutorial related to Grid'5000](https://www.grid5000.fr/w/SSH)
  - [More about SSH for Grid'5000](https://github.com/lnussbaum/slides-lectures/blob/master/ssh/ssh.pdf)
- [SSH Essentials: Working with SSH Servers, Clients, and Keys](https://www.digitalocean.com/community/tutorials/ssh-essentials-working-with-ssh-servers-clients-and-keys)
- [Understand ssh-keygen](https://www.ssh.com/academy/ssh/keygen?hsLang=en)
- [Understand keys in SSH](https://www.ssh.com/academy/ssh/key)
- [SSH.com Academy about SSH](https://www.ssh.com/academy/ssh)
- Strong passwords recommended by
  - [ANSSI helps you to strenghten your password](https://www.ssi.gouv.fr/administration/precautions-elementaires/calculer-la-force-dun-mot-de-passe) (in french)
  - [CERN](https://security.web.cern.ch/recommendations/en/passwords.shtml) ([Password alternatives](https://security.web.cern.ch/recommendations/en/password_alternatives.shtml))
