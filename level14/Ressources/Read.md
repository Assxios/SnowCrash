HINT:
You need to reverse getflag, dont bother trying to use the level13 program sadly it doesn't work.

ANSWER:
When we run strings /bin/getflag we see all the hashed password that are stored. We can try to modify the value of the level13 program to get the flag. However it doesn't work, so we will have to reverse the getflag command.
let's use gdb /bin/getflag if we run it says:
You should not reverse this
However let's just dissamble the main and skip that warning:
layout ASM
b *main+67 # jump the ptrace warning
r
ju *main+1183
and we get:
7QiHafiNa3HVozsaXkawuYrTstxbpABHD8CPnHJ

Here is all adresses of the flags for the other levels:
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
