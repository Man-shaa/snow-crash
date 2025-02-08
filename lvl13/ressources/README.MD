level13@SnowCrash:~$ ls -la
-rwsr-sr-x 1 flag13  level13 7303 Aug 30  2015 level13

level13@SnowCrash:~$ strings level13
[^_]
0123456
UID %d started us but we we expect %d
boe]!ai0FB@.:|L6l@A?>qJ}I
your token is %s
;*2$"$

env : 
SSH_CLIENT=10.0.2.2 44188 4242

level13 is executed with flag13 permission but it fails because the calling UID is not 4242
but 2013 (level13 UID)

cat /etc/passwd : 
no 4242 UID provided to any user
level13:x:2013:2013::/home/user/level13:/bin/bash
flag13:x:3013:3013::/home/flag/flag13:/bin/bash

change the getuid call for it to return 4242 ?

tried with a custom fake_getuid.c but didn't manage to make it work

Using gdb we can modify direclty the return value of getuid call :
`gdb -q ~/level13`

- break getuid
- run
- return (int) 4242
 	(confirm with y)
- continue

and we obtain the token
