ls -la /home/user/level02
----r--r-- 1 flag02  level02 8302 Aug 30  2015 level02.pcap

.pcap : network traffic logs
`tcpdump -qns 0 -A -r level02.pcap` to print the content witht some formating

[...]
06:23:48.730424 IP 59.233.235.223.12121 > 59.233.235.218.39247: tcp 35
E..W..@.@..I;...;.../Y.O...)...............
..R}...^.

Login incorrect
wwwbugs login: 
[...]

apparenly someone trying to log to "wwwbugs" account from remote as it's in this .pcap file which 
is logging all network traffic 


using `hexdump -C level02.pcap` we can examine the raw packet data

level02@SnowCrash:~$ cat -n /tmp/output | grep -n -i pass
249:00000f80  50 61 73 73 77 6f 72 64  3a 20 4e 66 26 4e 61 74  |Password: Nf&Nat|


Nf&Nat --- didn't found anything with this

using wireshark on the file :+ follow TCP stream :

..%..%..&..... ..#..'..$..&..... ..#..'..$.. .....#.....'........... .38400,38400....#.SodaCan:0....'..DISPLAY.SodaCan:0......xterm.........."........!........"..".....b........b....	B.
..............................1.......!.."......"......!..........."........"..".............	..
.....................
Linux 2.6.38-8-generic-pae (::ffff:10.1.1.2) (pts/10)

..wwwbugs login: l.le.ev.ve.el.lX.X
..
Password: ft_wandr...NDRel.L0L
.
..
Login incorrect
wwwbugs login: 

clicking on a char in the follow TCP Stream window displays the package for this character,
we can see that the "." char's data is "7f" which is DEL on ascii table (hexa)
so we can remove all the deleted chars from ft_wandr...NDRel.L0L.
Which gives ft_waNDReL0L

password : ft_waNDReL0L