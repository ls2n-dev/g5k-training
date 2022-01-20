# Discovering, visualizing and reserving Grid'5000 resources

At this point, you should now be connected to a site frontend, as indicated by your shell prompt (`<login>@f<site>: ~ %`). This machine will **ONLY** be used to reserve and manipulate resources on this site, using the *OAR software suite* will be the only way to request resources before using them. 

## Discovering and visualizing
There are several ways to learn about the site's resources and their status:

### On SSH connection
The site's MOTD (message of the day) lists all clusters and their features. 

```bash
Welcome to Grid'5000

** Useful links:
 - users home: https://www.grid5000.fr/w/Users_Home
 - usage policy: https://www.grid5000.fr/w/Grid5000:UsagePolicy
 - account management (password change): https://api.grid5000.fr/ui/account
 - support: https://www.grid5000.fr/w/Support

** Connect to a site:
 - to connect to any of the Grid'5000 sites:
     grenoble lille luxembourg lyon nancy nantes rennes sophia
   (from here) $ ssh <site>
 - mind configuring the ssh aliases (proxycommand) on your workstation to
   connect directly to the Grid'5000 sites from the outside:
   (from outside) $ ssh <site>.g5k
 - or use the Grid'5000 VPN: https://www.grid5000.fr/w/VPN

** Data on access.grid5000.fr (aka access-{north,south}.grid5000.fr):
 - home directories on access machines are not synchronized, access machines
   should not be used to store data.
 - to copy data from/to a site, use the site NFS mount point linked in the home:
   (from outside) $ scp files login@access.grid5000.fr:<site>/
 - or copy directly from/to the site using the ssh alias (proxycommand):
   (from outside) $ scp files login@<site>.g5k:

See https://www.grid5000.fr/w/Getting_Started for more information.
```

Additionally, it gives also the list of current or future downtimes due to maintenance, which is also available from at https://www.grid5000.fr/status/ .

```
** Warning: 1 event in progress
--> #Incident at #Nantes from 2021-09-03@16:30 : air conditioning failure, ecotype nodes unavailable
    https://intranet.grid5000.fr/bugzilla/show_bug.cgi?id=13386
```

**Exercise:** Can you tell me what issue is actually under investigation on site Lyon ?
<details><summary>Answer</summary>
<p>
<ul>
 <li> https://www.grid5000.fr/status/#LYON</li>
 <li> https://intranet.grid5000.fr/status/artifact/#LYON (<a href=https://intranet.grid5000.fr/bugzilla/show_bug.cgi?id=7353>bug#7353</a>)</li>
 </ul>
</p>
</details>

### On the wiki
Site pages on the wiki (e.g. [Nantes:Home](https://www.grid5000.fr/w/Nantes:Home)) contain a detailed description of the site's hardware and network.

**Exercise:** What is the url of the Wiki Page of Lille Site ?
<details><summary>Answer</summary>
<p>
https://www.grid5000.fr/w/Lille:Home
</p>
</details>


### On the status page
The page information links to the resource status on each site, with two different visualizations available:
- the current placement and queued jobs status (see [Nantes's current status](https://intranet.grid5000.fr/oar/Nantes/monika.cgi)) **in LIVE**
- the current and planned resources reservations in a Gantt diagram history (see [Nantes's current status](https://intranet.grid5000.fr/oar/Nantes/drawgantt-svg/)) 

<!--
https://www.grid5000.fr/w/TechTeam:UsagePolicyCheck
-->
