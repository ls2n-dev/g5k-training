# Connecting for the first time

The primary way to move around Grid'5000 is using SSH. A [reference page on SSH](https://github.com/ls2n-dev/g5k-training/blob/main/2021-01-24/0-about_all_ssh.md) is updated and maintained with advanced configuration options that frequent users will find useful.

<img src="https://www.grid5000.fr/mediawiki/images/Grid5000_SSH_access.png" width=80% height=80%>

As described in the figure above, when using Grid'5000, you will typically do these steps:
- connect by SSH to a frontend access machine `access.grid5000.fr`
- when connected, you will choose a site frontend server to access to that cluster resources, again by SSH.
- on this site frontend, you will submit jobs to schedule computing resources (nodes)

## Connection by SSH

### SSH connection through a web interface

If you want an out-of-the-box solution which does not require you to setup SSH, you can connect through a web interface. The interface is available at https://intranet.grid5000.fr/shell/SITE/. 

For example, to access nantes's site, use: https://intranet.grid5000.fr/shell/nantes/ (DO NOT FORGET THE '/' caracter at the end). To connect you will have to type in your credentials twice (first for the HTTP proxy, then for the SSH connection).

### Connecting to a Grid'5000 frontal access machine

To enter the Grid'5000 network from Internet, you must connect to a frontend access machine `access.grid5000.fr` . You must use login with your g5k credentials that was provided to you when you asked a Grid'5000 account creation. This frontend server is just a simple round robin alias which routes either to node `access-north` which is currently hosted in Lille, or node `access-south` hosted in Sophia-Antipolis. 

```bash
outside% ssh <login>@access.grid5000.fr
```

You will get authenticated using the SSH public key you provided in the account creation form. Password authentication is disabled.

You can modify your SSH keys in the [account management interface](https://api.grid5000.fr/stable/users/).

### Connecting to a Grid'5000 site access machine

Now you are connected inside one of the G5K frontal access node (`access-north` or `access-south`), you can now reach to one the Grid'5000 sites. As you know the G5k platform is structured into sites (Grenoble, Rennes, Nancy, Nantes ...). Each site hosts one or more clusters (homogeneous sets of machines, usually bought at the same time). As you can see below:

![g5k sites](https://www.grid5000.fr/mediawiki/images/Renater5-g5k.jpg)

#### Tips : ssh config file with proxycommand
Configure SSH aliases using the `ProxyCommand` option. Using this, you can avoid the two-hops connection (access machine, then frontend) but establish connections directly to frontends. This requires using OpenSSH, which is the SSH software available on all GNU/Linux systems, MacOS, and also recent versions of Microsoft Windows.

Add or append these following line in your `.ssh/config` (UNIX) or `\Program Files\Git\etc\ssh\ssh_config` (WINDOWS)
```bash
Host g5k
  User login
  Hostname access.grid5000.fr
  ForwardAgent no

Host *.g5k
  User login
  ProxyCommand ssh g5k -W "$(basename %h .g5k):%p"
  ForwardAgent no
```
> Reminder: `login` here is your Grid'5000 username.

> Warning: the `ProxyCommand` line works if your login shell is `bash`. If not you may have to adapt it. For instance, for the fish shell, this line must look like this `ProxyCommand ssh g5k -W (basename %h .g5k):%p`.

Once done, you can establish connections to any machine (first of all: frontends) inside Grid'5000 directly, just by suffixing `.g5k` to its hostname (instead of first having to connect to an access machine). 




