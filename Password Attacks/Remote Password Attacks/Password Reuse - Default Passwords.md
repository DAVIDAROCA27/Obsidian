## Credential Stuffing

There are various databases that keep a running list of known default credentials. One of them is the [DefaultCreds-Cheat-Sheet](https://github.com/ihebski/DefaultCreds-cheat-sheet). Here is a small excerpt from the entire table of this cheat sheet:

Default credentials can also be found in the product documentation, as they contain the steps necessary to set up the service successfully. Some devices/applications require the user to set up a password at install, but others use a default, weak password. Attacking those services with the default or obtained credentials is called [Credential Stuffing](https://owasp.org/www-community/attacks/Credential_stuffing). This is a simplified variant of brute-forcing because only composite usernames and the associated passwords are used.

We can imagine that we have found some applications used in the network by our customers. After searching the internet for the default credentials, we can create a new list that separates these composite credentials with a colon (`username:password`). In addition, we can select the passwords and mutate them by our `rules` to increase the probability of hits.

#### Credential Stuffing - Hydra Syntax

  Password Reuse / Default Passwords

```shell-session
pepedavidphone@htb[/htb]$ hydra -C <user_pass.list> <protocol>://<IP>
```

#### Credential Stuffing - Hydra

  Password Reuse / Default Passwords

```shell-session
pepedavidphone@htb[/htb]$ hydra -C user_pass.list ssh://10.129.42.197
```

Besides the default credentials for applications, some lists offer them for routers. One of these lists can be found [here](https://www.softwaretestinghelp.com/default-router-username-and-password-list/). It is much less likely that the default credentials for routers are left unchanged. Since these are the central interfaces for networks, administrators typically pay much closer attention to hardening them. Nevertheless, it is still possible that a router is overlooked or is currently only being used in the internal network for test purposes, which we can then exploit for further attacks.

B@tm@n2022!