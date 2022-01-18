# OpenSSH

## What is SSH?

The ssh or secure shell is a network protocol for operating networking services securely over a network. It uses encryption standards to securely connect and login to the remote system.

It stores a public key in the remote system and private key in the client system. Thes keys are produced as a pair mathematically. When both are applied to a bi-variable function, it will result in a value which will be used to check whether the pair is valid or invalid. This is the simplest explanation possible. To Learn more, please refer to this page.

## Get start with SSH

Source at https://www.openssh.com

### MacOSX

Install with [`brew`](https://brew.sh) by just typing `brew install openssh`

### Linux

Use your favorite installer (`apt` `yum` `yast` `dpkg` `emerg` etc.) and find the package related to the keyword `openssh` or `ssh`.
Install only the client-ssh.

### Windows
Follow only one of the listed installation:
- [Official installation](https://docs.microsoft.com/en-us/windows-server/administration/openssh/openssh_install_firstuse)
- [PowerShell/openssh-portable](https://github.com/PowerShell/OpenSSH-Portable)

## Generate a new ssh key pair
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
- Strong passwords recommended by
  - [ANSSI helps you to strenghten your password](https://www.ssi.gouv.fr/administration/precautions-elementaires/calculer-la-force-dun-mot-de-passe) (in french)
  - [CERN](https://security.web.cern.ch/recommendations/en/passwords.shtml) ([Password alternatives](https://security.web.cern.ch/recommendations/en/password_alternatives.shtml))
