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

./a.out "You should not reverse this"
Ynsocind[ccgcUeS__P
]PPY%

./test "Youshouldnotreversethis"
Ynspdjoe\eeifXhVbbSaTT]% 

./a.out "f4kmm6p|=�p�n��DB�Du{��"
f3iji1ju5浲dⱮ^ޭ�۪�-*֥�(X]П�͜�

token contains non printable character so tried opening the file as a binary file and do the same reverse transformation :


#include <stdio.h>
#include <stdlib.h>

int main(int ac, char **av) {
	if (ac != 2) {
		printf("Usage: %s <file>\n", av[0]);
		return 1;
	}
    FILE *fp = fopen(av[1], "rb"); // Open in binary mode
    if (!fp) {
        perror("fopen");
        return 1;
    }

    char token[100]; // Buffer to store token
    int len = fread(token, 1, sizeof(token) - 1, fp);
    fclose(fp);

    if (len <= 0) {
        printf("Failed to read token.\n");
        return 1;
    }
    token[len] = '\0';

    // Decode
    for (int i = 0; i < len; i++) {
        printf("%c", token[i] - i);
    }
    printf("\n");

    return 0;
}

Also copied token from the VM :
scp -P 4243 level09@127.0.0.1:/tmp/token.txt .

and it works !

./a.out "token.txt"
f3iji1ju5yuevaus41q1afiuq�
