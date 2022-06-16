HINT:
perhaps looking at Linux system's configuration files can help

ANSWER:
do:
cat /etc/passwd | grep "flag01"
we find:
42hDRfypTqqnw
not the correct answer so we use john
echo "42hDRfypTqqnw" > hack and john --show hack
we get abcdefg
we do su flag01 it works then getflag and we get:
f2av5il02puano7naaf6adaaf