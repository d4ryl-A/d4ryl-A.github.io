---
title: 'TryHackMe - Dodge'
categories: [TryHackMe]
tags: [web, vhost, subdomain, ufw, firewall, ftp, sudo, apt]
render_with_liquid: false
img_path: /images/thm_dodge/
---

![Tryhackme Room Link](thm_dodge.png){: width="600" height="150" .shadow }
_<https://tryhackme.com/room/dodge>_

## Initial enumeration

### Nmap Scan

```console
$ nmap -T4 -n -sC -sV -Pn -p- 10.10.64.72
Nmap scan report for 10.10.64.72
Host is up (0.085s latency).
Not shown: 65532 filtered tcp ports (no-response)
PORT    STATE SERVICE  VERSION
22/tcp  open  ssh      OpenSSH 8.2p1 Ubuntu 4ubuntu0.3 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey: 
|   3072 12:22:46:5a:8c:3a:53:16:60:78:3e:79:e1:66:ba:7d (RSA)
|   256 af:3a:3b:76:01:69:67:42:4a:53:ee:48:60:7c:6e:15 (ECDSA)
|_  256 36:87:fb:1f:3f:3c:30:cc:b5:61:23:9d:db:01:13:d9 (ED25519)
80/tcp  open  http     Apache httpd 2.4.41
|_http-server-header: Apache/2.4.41 (Ubuntu)
|_http-title: 403 Forbidden
443/tcp open  ssl/http Apache httpd 2.4.41
|_http-title: 403 Forbidden
| tls-alpn: 
|_  http/1.1
|_ssl-date: TLS randomness does not represent time
| ssl-cert: Subject: commonName=dodge.thm/organizationName=Dodge Company, Inc./stateOrProvinceName=Tokyo/countryName=JP
| Subject Alternative Name: DNS:dodge.thm, DNS:www.dodge.thm, DNS:blog.dodge.thm, DNS:dev.dodge.thm, DNS:touch-me-not.dodge.thm, DNS:netops-dev.dodge.thm, DNS:ball.dodge.thm
| Not valid before: 2023-06-29T11:46:51
|_Not valid after:  2123-06-05T11:46:51
|_http-server-header: Apache/2.4.41 (Ubuntu)
Service Info: Hosts: default, ip-10-10-64-72.eu-west-1.compute.internal; OS: Linux; CPE: cpe:/o:linux:linux_kernel
```
Open ports:
| 22 | SSH |
|---|---|
| 80 | HTTP |
| 443 | HTTPS |

### Searching for subdomains

By visiting the website, we can see that `https` worked and checking the certificate, we can see a lists of subdomains.

![image](https://github.com/d4ryl-A/d4ryl-A.github.io/assets/129752764/8acc8578-98e9-4549-98a0-75256dfff54d)

We then add these to `/etc/hosts`
```
10.10.64.72 dodge.thm www.dodge.thm blog.dodge.thm dev.dodge.thm touch-me-not.dodge.thm netops-dev.dodge.thm ball.dodge.thm
```
{: file="/etc/hosts" }

### Enumerating the Subdomains

We will use `feroxbuster` to look for any directory

Command:

```
feroxbuster -u https://netops-dev.dodge.thm/ -w /usr/share/wordlists/dirb/big.txt -x php,txt,html,js,pdf -k -s 200
```

![image](https://github.com/d4ryl-A/d4ryl-A.github.io/assets/129752764/0113e062-d121-4f56-9033-86e1faef42ce)

When checking and inspecting the 'firewall.js` page, there is an interesting path named `firewall10110.php`.

![image](https://github.com/d4ryl-A/d4ryl-A.github.io/assets/129752764/cd77b749-19a0-4726-9688-cfdb09fd1100)

And visiting the page, we can then see a page where we can update the firewall rules of the machine.

![image](https://github.com/d4ryl-A/d4ryl-A.github.io/assets/129752764/a4c4776c-4681-4eb0-ae4b-8271db2f47e2)

We see that there are 4 ports and one is not allowed, which is port 21 or FTP, we can then try and change that to allow list.

Command:

```
sudo ufw allow 21
```

![image](https://github.com/d4ryl-A/d4ryl-A.github.io/assets/129752764/5394bf1c-7a9f-482f-95a7-0c1b95292859)

It Worked! 

And now we can connect to FTP and using the `anonymous` login.

![image](https://github.com/d4ryl-A/d4ryl-A.github.io/assets/129752764/63ac19e2-0469-4beb-bb3a-9d70534e21b7)

We can see the `user.txt` but unfortunately we cannot download or see it.

![image](https://github.com/d4ryl-A/d4ryl-A.github.io/assets/129752764/ba5d7e51-99de-404f-8fa3-ec8090f1da3c)

Further enumerating the FTP server and by issuing `ls -la` command to show the hidden files, we can see an `id_rsa` or the ssh key.

![image](https://github.com/d4ryl-A/d4ryl-A.github.io/assets/129752764/908cce4a-54eb-4222-a851-caa8b368a848)

By issuing the `mget *` command to download all the available files, we successfully download the `authorized_keys` and `id_rsa_backup`.

![image](https://github.com/d4ryl-A/d4ryl-A.github.io/assets/129752764/c248a651-8b37-4121-bfd7-bdd4476c10c7)

Going back to our machine and by doing a `cat authorized_keys`, we can see an account named `challenger`

![image](https://github.com/d4ryl-A/d4ryl-A.github.io/assets/129752764/6f5b532a-d41b-45c4-8a92-8d85aa1ad832)

We can test and see if the `id_rsa` that we got will work for this user.

Command will be `ssh -i id_rsa_backup challenger@target_IP`

And it worked and we are connected to our target machine.

![image](https://github.com/d4ryl-A/d4ryl-A.github.io/assets/129752764/fd5f8997-e7e0-429b-af0d-8f1d0aba4ec6)

And then we can now retrieve the user's flag.

![image](https://github.com/d4ryl-A/d4ryl-A.github.io/assets/129752764/cbb7b7bc-c93f-43fb-9d74-25852b20eeb1)

By going to `/var/www/notes/api` we can see the `config.php` but unfortunately no password for the database.

![image](https://github.com/d4ryl-A/d4ryl-A.github.io/assets/129752764/fcd2d698-42b3-48a2-ac0b-b4428bb2ca39)

Moving on, by looking at `login.php` there is another visible credential.

`gabriela:^5hf5w&CAt9sPr@`

And inside `posts.php`, there is a `base64` encoded information.

![image](https://github.com/d4ryl-A/d4ryl-A.github.io/assets/129752764/a3c3786e-9f6a-4a2e-9e58-f56116770ea8)

Decoding it gave us another credential

`cobra:mz4%o7BGum#TTu`

and by doing `su cobra` and entering the password.

![image](https://github.com/d4ryl-A/d4ryl-A.github.io/assets/129752764/b96e9438-efb5-4b28-9ff4-d5b5d83f073c)

And now we got a user with password, we will then issue a `sudo -l` command, to check if this user has an ability to run any command as `sudo`

![image](https://github.com/d4ryl-A/d4ryl-A.github.io/assets/129752764/6520ea62-03dd-467f-9cf1-b25d63c360ba)

We can then see that the user `cobra` may run the command `/usr/bin/apt`

By looking at GTFObins website, we can see that the `apt` command is in there and we can get a root shell.

![image](https://github.com/d4ryl-A/d4ryl-A.github.io/assets/129752764/6f996505-15d8-48ea-be1d-7d49e2489051)

By following the command and issuing it to our target machine using the `cobra` user.

![image](https://github.com/d4ryl-A/d4ryl-A.github.io/assets/129752764/b9bf61c9-2c38-4146-8bc6-49d6bc7c02d8)

We got a root!

We can then retrieve the root's flag.

![image](https://github.com/d4ryl-A/d4ryl-A.github.io/assets/129752764/f8b5c899-3f2b-4b55-a4b3-3159f31e0db4)







