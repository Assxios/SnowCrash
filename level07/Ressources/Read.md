HINT:
do:
strings level07

ANSWER:
when we do strings level07 we notice that the program uses a getenv futhermore we notice the strings LOGNAME and /bin/echo %s
We also see that when we run the program it prints level07, supposely our LOGNAME.
We can run gdb to verify it and bingo the getenv indeed searches for our LOGNAME so let's modify our environment:
LOGNAME='`getflag`'
and we get:
fiumuikeil55xe9cu4dood66h