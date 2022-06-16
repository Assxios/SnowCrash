HINT:
try to run the program like this ./level09 0000000000000000000000000000000000000000000000
perhaps you will notice a pattern

ANSWER:
once we first run the program we notice that you need to provide an argument, alright let's try to feed it the token, we get:
tpmhr
so nothing obvious so far. let's try running it with a bunch of zero, we get:
0123456789:;<=>?@ABCDEFGHIJKLMNOPQRSTUVWXYZ[\]^_`abcdefghijklmnopqrstuvwxyz{|}~
It appears to be adding the index of the value to its own value therefore we can presume that we can just do the opposite with a simple C program:
#include "unistd.h"

void ft_putchar(char c)
{
    write(1, &c, 1);
}

int main(int argc, char **argv)
{
    if (argc != 2)
        return (1);

    size_t i = 0;
    while (argv[1][i])
        ft_putchar(argv[1][i] - i++);
    ft_putchar('\n');
}
Let's now try to feed the content of the token to our program and we get:
f3iji1ju5yuevaus41q1afiuq
then su flag09 and getflag:
s5cAJpM8ev6XHw998pRWG728z