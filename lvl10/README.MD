level10@SnowCrash:~$ ls -laRh
-rwsr-sr-x+ 1 flag10  level10  11K Mar  5  2016 level10
-rw-------  1 flag10  flag10    26 Mar  5  2016 token


level10@SnowCrash:~$ ./level10 hey hello
You don't have access to hey

level10@SnowCrash:~$ echo "Hello World" > /tmp/test.txt
level10@SnowCrash:~$ ./level10 /tmp/test.txt hey
Connecting to hey:6969 .. Unable to connect to host hey

strings ./level10 hey helo
socket
connect
%s file host
	sends file to host if you have access to it
Connecting to %s:6969 .. 
Unable to connect to host %s
.*( )*.
Unable to write banner to host %s
Connected!
Sending file ..
[...]
strings: 'hey': No such file
strings: 'helo': No such file

level10@SnowCrash:~$ strace ./level10 hey helo
access("hey", R_OK)                     = -1 ENOENT (No such file or directory)
write(1, "You don't have access to hey\n", 29You don't have access to hey
) = 29

- launch a server to listen to port 6969 (with nc ?)
- create a file with permission for the access check, and delete it before the opens so it opens the token instead using flag10's permissions 
- get content from nc listener
