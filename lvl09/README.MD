level09@SnowCrash:~$ ls -la
-rwsr-sr-x 1 flag09  level09 7640 Mar  5  2016 level09
----r--r-- 1 flag09  level09   26 Mar  5  2016 token

strings level09
[...]
puts
[...]
[^_]
You should not reverse this
[...]
You need to provied only one arg.
00000000 00:00 0
LD_PRELOAD detected through memory maps exit ..
;*2$"$

strace level09 :
[...]
write(1, "You should not reverse this\n", 28You should not reverse this
) = 28
exit_group(1)                           = ?

level09@SnowCrash:~$ ./level09 token
tpmhr

 changed "token" into "tpmhr"

level09@SnowCrash:~$ ./level09 aaaaa
abcde

the program seems to be changing letters such as : argv[1][i] = argv[1][i] + i
and looping on the whole passed argument

level09@SnowCrash:~$ ./level09 "You should not reverse this"
Ypw#wmu|tm*y{�.�u�w��z6����

t o k e n
0 1 2 3 4
t n i b j

tried to reverse with 

```
#include <stdio.h>
#include <unistd.h>
#include <stdlib.h>
#include <string.h>

int main(int ac, char **av)
{
	if (ac !=2)
		return (1);
	for (int i = 0; i < strlen(av[1]); i++)
	{
		char c = av[1][i] - i;
		write(1, &c, 1);
	}
	return (0);
}
```

"You should not reverse this"
Ynsocind[ccgcUeS__P
]PPY%