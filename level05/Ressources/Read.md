HINT:
"You have new mail." :)

ANSWER:
Once we log in we can see the "You have new mail." prompt, let's check the mail:
*/2 * * * * su -c "sh /usr/sbin/openarenaserver" - flag05
So we know there is the openarenaserver that is executed every 2 minutes, let's read it, we can see it does a loop for every file in /opt/openarena/server/ and it will execute their contents:
echo 'getflag > /tmp/flag' > ezpz
then we wait two minutes (or just use watch) and we get:
viuaaale9huek52boumoomioc
