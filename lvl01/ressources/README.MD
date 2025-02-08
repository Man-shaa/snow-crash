 shadow sudoers sudoers.d group group- gshadow gshadow- passwd hostname hosts  pm

cat /etc/group
	flag:x:1001:flag00,flag01,flag02,flag03,flag04,flag05,flag06,flag07,flag08,flag09,flag10,flag11,flag12,flag13,flag14

cat /etc/passwd 
	flag01:42hDRfypTqqnw:3001:3001::/home/flag/flag01:/bin/bash

gpasswd uuidd pppd
/rofs/bin/getflag


https://github.com/openwall/john john the ripper github / see README for installation tuto

using 42hDRfypTqqnw (encrypted password) and john the ripper installed via github openwall
-> abcdefg