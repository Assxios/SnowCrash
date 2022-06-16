HINT:
Perhaps a symbolic link could help

ANSWER:
When we first start the program it specifies that it needs a file to read, let's try with the token file and we get:
You may not access 'token'
ok let's try with another file then, for example our .bashrc, we get:
level08: Unable to open .bashrc: Permission denied
a different error message that's weird, let's check what's going on with gdb, we notice that it checks if the file is called token therefore let's modify the name. But we can't do mv nor cp so our only option is a symbolic link in the /tmp/:
ln -s ~/token /tmp/uwu
we get:
quif5eloekouj29ke0vouxean
then su flag08 and getflag:
25749xKZ8L7DkSCwJkT9dyv6f
