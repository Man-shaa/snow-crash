ls -la 
nothing !

tried to search into /tmp, /tmp/var, /var/log, didn't find anything

ps aux
102        914  0.0  0.0   3260   892 ?        Ss   16:11   0:00 dbus-daemon --system --fork --activatio
whoopsie  1387  0.0  0.0  24476  3760 ?        Ssl  16:11   0:00 whoopsie

level14@SnowCrash:~$ ps -u level14
  PID TTY          TIME CMD
 2599 ?        00:00:00 sshd
 2600 pts/0    00:00:00 bash
 2716 pts/0    00:00:00 ps

level14@SnowCrash:~$ ls -l /proc/2599
