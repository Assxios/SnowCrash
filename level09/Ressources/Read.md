# Level09

## Hint

Try to run the executable like this:
```
./level09 0000000000000000000000000000000000000000000000
```
Perhaps you will notice a pattern.

## Answer

Let's check our current directory:
```
level09@SnowCrash:~$ ll
total 24
dr-x------ 1 level09 level09  140 Mar  5  2016 ./
d--x--x--x 1 root    users    340 Aug 30  2015 ../
-r-x------ 1 level09 level09  220 Apr  3  2012 .bash_logout*
-r-x------ 1 level09 level09 3518 Aug 30  2015 .bashrc*
-rwsr-sr-x 1 flag09  level09 7640 Mar  5  2016 level09*
-r-x------ 1 level09 level09  675 Apr  3  2012 .profile*
----r--r-- 1 flag09  level09   26 Mar  5  2016 token
```

We have an executable file, let's run it first !
```
level09@SnowCrash:~$ ./level09
You need to provied only one arg.
```

So we need a file to read, let's try to read the `token` file:
```
level09@SnowCrash:~$ ./level09 token
tpmhr
```

Nothing interesting, let's try to random characters:
```
level09@SnowCrash:~$ ./level09 0000000000000000000000000000000000000000000000000000000000000000000000000000000
0123456789:;<=>?@ABCDEFGHIJKLMNOPQRSTUVWXYZ[\]^_`abcdefghijklmnopqrstuvwxyz{|}~
```

There appears to be a pattern in the output, the program seems to take each character of the first argument and adding its own index to it. Therefore we can reverse what the program does with a simple C program:
```C
#include <unistd.h>

void ft_putchar(char c)
{
	write(1, &c, 1);
}

int main(int argc, char **argv)
{
	if (argc != 2) return (1);
	size_t i = 0;
	while (argv[1][i])
    		ft_putchar(argv[1][i] - i++);
	ft_putchar('\n');
}
```

Now let's compile and run our program:
``` 
level09@SnowCrash:/tmp$ gcc main.c; ./a.out `cat ~/token`
f3iji1ju5yuevaus41q1afiuq
```

Yet again we login into `flag09` and get our flag:
``` 
level09@SnowCrash:/tmp$ su flag09
Password:
Don't forget to launch getflag !
flag09@SnowCrash:~$ getflag
Check flag.Here is your token : s5cAJpM8ev6XHw998pRWG728z
```
