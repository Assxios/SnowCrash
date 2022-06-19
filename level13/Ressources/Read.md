# Level13

## Hint

Perhaps you could use `gdb` to bypass some of the checks or change some values.

## Answer

Let's check our current directory:
```
level13@SnowCrash:~$ ll
total 20
dr-x------ 1 level13 level13  120 Mar  5  2016 ./
d--x--x--x 1 root    users    340 Aug 30  2015 ../
-r-x------ 1 level13 level13  220 Apr  3  2012 .bash_logout*
-r-x------ 1 level13 level13 3518 Aug 30  2015 .bashrc*
-rwsr-sr-x 1 flag13  level13 7303 Aug 30  2015 level13*
-r-x------ 1 level13 level13  675 Apr  3  2012 .profile*
```

We have an executable file, let's run it first !
```
level13@SnowCrash:~$ ./level13
UID 2013 started us but we we expect 4242
```

Seems like we have the wrong UID, let's run `strings` and see what we get:
```
level13@SnowCrash:~$ strings level13
[...]
UID %d started us but we we expect %d
boe]!ai0FB@.:|L6l@A?>qJ}I
your token is %s
[...]
```

Seems like that if we somehow go past the UID check we will be able to get the token, also there's a weird string in there, let's run `gdb` and see if we find anything interesting:
```
level13@SnowCrash:~$ gdb ./level13
(gdb) layout ASM
0x804858c <main>                push   %ebp
0x804858d <main+1>              mov    %esp,%ebp
0x804858f <main+3>              and    $0xfffffff0,%esp
0x8048595 <main+9>              call   0x8048380 <getuid@plt>
0x804859a <main+14>             cmp    $0x1092,%eax
0x804859f <main+19>             je     0x80485cb <main+63>
0x80485a1 <main+21>             call   0x8048380 <getuid@plt>
0x80485a6 <main+26>             mov    $0x80486c8,%edx
0x80485ab <main+31>             movl   $0x1092,0x8(%esp)
0x80485b3 <main+39>             mov    %eax,0x4(%esp)
0x80485b7 <main+43>             mov    %edx,(%esp)
0x80485ba <main+46>             call   0x8048360 <printf@plt>
0x80485bf <main+51>             movl   $0x1,(%esp)
0x80485c6 <main+58>             call   0x80483a0 <exit@plt>
0x80485cb <main+63>             movl   $0x80486ef,(%esp)
0x80485d2 <main+70>             call   0x8048474 <ft_des>
0x80485d7 <main+75>             mov    $0x8048709,%edx
0x80485dc <main+80>             mov    %eax,0x4(%esp)
0x80485e0 <main+84>             mov    %edx,(%esp)
0x80485e3 <main+87>             call   0x8048360 <printf@plt>
0x80485e8 <main+92>             leave
0x80485e9 <main+93>             ret 
```

Alright, seems like after `getuid` is executed there is a `cmp` and `je` instruction. We can assume that is where the program checks if we have the right UID, and if we don't it will exit. Therefore let's just break and skip over that check shall we:
```
(gdb) b *main+9
Breakpoint 1 at 0x8048595
(gdb) r
Starting program: /home/user/level13/level13

Breakpoint 1, 0x08048595 in main ()
(gdb) ju *main+63
Continuing at 0x80485cb.
your token is 2A31L79asukciNyi8uppkEuSx
[Inferior 1 (process 26540) exited with code 050]
```

Pretty simple, we could also have replaced the results of the `getuid` function with 4242, but that would have been a bit more complicated. Therefore we picked the easiest option.