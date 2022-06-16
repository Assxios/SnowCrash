HINT:
Perhaps we could bypass some checks with gdb

ANSWER:
when we first run the program it says we have the wrong UID, so let's try to start it with gdb:
we see this line that getuid is execute and then compared with 4242 therefore let's break before it and jump over it:
b *main+9
r
ju *main+63
and we get:
2A31L79asukciNyi8uppkEuSx
