# Level14

## Hint

Reversing the `getflag` command will give you the flag.

## Answer

Let's check our current directory:
```
level14@SnowCrash:~$ ll
total 12
dr-x------ 1 level14 level14  100 Mar  5  2016 ./
d--x--x--x 1 root    users    340 Aug 30  2015 ../
-r-x------ 1 level14 level14  220 Apr  3  2012 .bash_logout*
-r-x------ 1 level14 level14 3518 Aug 30  2015 .bashrc*
-r-x------ 1 level14 level14  675 Apr  3  2012 .profile*
```

So we have absolutely nothing. We spent a lot of time looking for hidden files but nothing came up, so we simply decided the reverse the `getflag` command. Perhaps there is a smarter way to get the flag but this is what we did:
```
level14@SnowCrash:~$ gdb getflag
(gdb) r
Starting program: /bin/getflag
You should not reverse this
[Inferior 1 (process 26792) exited with code 01]
```

Although it says that, we're still going to reverse the command, however let's run `strings` to check if we can find anything interesting:
```
level14@SnowCrash:~$ strings /bin/getflag
[...]
Check flag.Here is your token :
You are root are you that dumb ?
I`fA>_88eEd:=`85h0D8HE>,D
7`4Ci4=^d=J,?>i;6,7d416,7
<>B16\AD<C6,G_<1>^7ci>l4B
B8b:6,3fj7:,;bh>D@>8i:6@D
?4d@:,C>8C60G>8:h:Gb4?l,A
G8H.6,=4k5J0<cd/D@>>B:>:4
H8B8h_20B4J43><8>\ED<;j@3
78H:J4<4<9i_I4k0J^5>B1j`9
bci`mC{)jxkn<"uD~6%g7FK`7
Dc6m~;}f8Cj#xFkel;#&ycfbK
74H9D^3ed7k05445J0E4e;Da4
70hCi,E44Df[A4B/J@3f<=:`D
8_Dw"4#?+3i]q&;p6 gtw88EC
boe]!ai0FB@.:|L6l@A?>qJ}I
g <t61:|4_|!@IF.-62FH&G~DCK/Ekrvvdwz?v|
Nope there is no token here for you sorry. Try again :)
[...]
```

Woah we have the exact same weird string `boe]!ai0FB@.:|L6l@A?>qJ}I` from level13. After that we went back on the level13 and realised something: there is a `ft_des` command which takes this exact weird string and turns it into our flag in the level13 executable. How convinient ! Perhaps we could replace it with `g <t61:|4_|!@IF.-62FH&G~DCK/Ekrvvdwz?v|` which appears to be the hashed version of the flag for level14. However after trying it, it seems like it doesn't work so let's give up on that idea.

Let's go back to `gdb` with getflag and read a bit of decompiled ASM code, it's extremely long so I'm not going to explain it. However we do know that `ft_des` allows us to decrypt the flag, and we notice that this `ft_des` function is called multiple times in a row in the level14 executable. We're going to assume it does the same thing as the one in the level13 but that it will work with the hashed version of all the flags we found with `strings`. Perhaps we could jump after it's execution and see what we get:
```
(gdb) b* main+67
Breakpoint 1 at 0x8048989
(gdb) r
Starting program: /bin/getflag

Breakpoint 1, 0x08048989 in main ()
(gdb) ju *main+704
ju *main+704
Continuing at 0x8048c06.

0xb7ea9151 in ?? () from /lib/i386-linux-gnu/libc.so.6
(gdb) x/s 0x80490b2
x/s 0x80490b2
0x80490b2:       "I`fA>_88eEd:=`85h0D8HE>,D"
```

How interesting, we just proved that `ft_des` indeed uses the weird strings as arguments for level14 aswell. So we can just jump to the result of `ft_des` and we should get the our flag *(along with all the other flags)*. Here is the list of all the flags:

```
0 - 0x8048bf3 <main+685>
1 - 0x8048c17 <main+721>
2 - 0x8048c3b <main+757>
3 - 0x8048c5f <main+793>
4 - 0x8048c83 <main+829>
5 - 0x8048ca7 <main+865>
6 - 0x8048ccb <main+901>
7 - 0x8048cef <main+937>
8 - 0x8048d13 <main+973>
9 - 0x8048d37 <main+1009>
10 - 0x8048d5b <main+1045>
11 - 0x8048d7f <main+1081>
12 - 0x8048da3 <main+1117>
13 - 0x8048dc4 <main+1150>
14 - 0x8048de5 <main+1183>
```

So let's go ahead and do that:
```
level14@SnowCrash:~$ gdb getflag
(gdb) b *main+67
Breakpoint 1 at 0x8048989
(gdb) r
Starting program: /bin/getflag

Breakpoint 1, 0x08048989 in main ()
(gdb) ju *main+1183
Continuing at 0x8048de5.
7QiHafiNa3HVozsaXkawuYrTstxbpABHD8CPnHJ
[Inferior 1 (process 27147) exited normally]
```
