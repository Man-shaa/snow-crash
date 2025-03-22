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

trying with gdb
0x08048989 <+67>:	call   0x8048540 <ptrace@plt>
   0x0804898e <+72>:	test   %eax,%eax
   0x08048990 <+74>:	jns    0x80489a8 <main+98>
   0x08048992 <+76>:	movl   $0x8048fa8,(%esp)
   0x08048999 <+83>:	call   0x80484e0 <puts@plt>
   0x0804899e <+88>:	mov    $0x1,%eax
   0x080489a3 <+93>:	jmp    0x8048eb2 <main+1388>
 0x080489f9 <+179>:	call   0x8048500 <open@plt>
   0x080489fe <+184>:	test   %eax,%eax
   0x08048a00 <+186>:	jle    0x8048a34 <main+238>

change the ptrace return so it always succeed ? 
or the open which might be the token file


   0x08048989 <+67>:	call   0x8048540 <ptrace@plt>
   0x0804898e <+72>:	test   %eax,%eax

test %eax,%eax checks if eax is 0 but also sets a flag "SF" (sign flag) if the value was negative
changing only eax is enough to bypass the ptrace call :

b *0x0804898e
run
set $eax = 0

now we have an issue probably  with the UID from getuid 
0x08048afd <+439>:	call   0x80484b0 <getuid@plt>

b *0x08048afd
continue
set $eax=3014
continue

doesn't work, need to modify eax during return not at the function's call
so from the beggining :


gdb --quiet /bin/getflag
b *0x0804898e		# breakpoint at test right after ptrace call (got address from disas main)
run
info registers		# eax set to -1
set $eax = 0
info registers		# eax modifed to 0, now that ptrace issue is handled, go to the getuid
disas getuid
   0xb7ee4ccc <+12>:	ret 

b *0xb7ee4ccc		# breakpoint at getuid's return
info registers		# eax set to 2014 which is our user ID
set $eax = 3014		# set it to flag14 ID
info registers eax	# modified to 3014
continue

Check flag.Here is your token : 7QiHafiNa3HVozsaXkawuYrTstxbpABHD8CPnHJ




set $eip = 0x80489a8  # Force jump to bypass anti-debugging
set $eflags = ($eflags & ~0x80) removes the SF flags (that indicates the test call return value was negative)