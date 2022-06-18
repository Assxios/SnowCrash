# Level00

## Hint

Perhaps watching [the video](https://elearning.intra.42.fr/notions/snow-crash/subnotions/snow-crash/videos/snow-crash) on the intra would be a good idea.

## Answer

Let's see what we have here:
```
level00@SnowCrash:~$ ll
total 12
dr-xr-x---+ 1 level00 level00  100 Mar  5  2016 ./
d--x--x--x  1 root    users    340 Aug 30  2015 ../
-r-xr-x---+ 1 level00 level00  220 Apr  3  2012 .bash_logout*
-r-xr-x---+ 1 level00 level00 3518 Aug 30  2015 .bashrc*
-r-xr-x---+ 1 level00 level00  675 Apr  3  2012 .profile*
```

So nothing interesting. I spent a lot of time trying things without success, then noticed there's [a video](https://elearning.intra.42.fr/notions/snow-crash/subnotions/snow-crash/videos/snow-crash) on the intra that I could watch.

After watching, we know that we need to find a file that is owned by `flag00`:
```
level00@SnowCrash:~$ find -O3 / -user "flag00" 2>/dev/null
/usr/sbin/john
/rofs/usr/sbin/john
```

Great, we found something, let's see what's inside:
```
level00@SnowCrash:~$ cat /usr/sbin/john
cdiiddwpgswtgt
```

Cool let's see if this is our token / flag:
```
level00@SnowCrash:~$ su flag00
Password:
su: Authentication failure
level00@SnowCrash:~$ su level01
Password:
su: Authentication failure
```

Hmpf it doesn't work, after a while we discovered that the token is encrypted with ceasar cipther, so let's bruteforce it !<br>
Let's use [decode.fr](https://www.dcode.fr/chiffre-cesar) to bruteforce the cipher, we notice that with the key 15 we get:
```
nottoohardhere
```

Great, let's try to login again:
```
level00@SnowCrash:~$ su flag00
Password:
Don't forget to launch getflag !
flag00@SnowCrash:~$ getflag
Check flag.Here is your token : x24ti5gi3x0ol2eh4esiuxias
```
