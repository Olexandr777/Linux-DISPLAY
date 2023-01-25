# Linux-DISPLAY

https://www.linuxquestions.org/questions/linux-general-1/x11-forwarding-is-broken-after-sudo-su-%96-526150/page2.html

Quote:
Originally Posted by miedward View Post

When you open an X forwarding connection (or any X connection) information about that session is put in $HOME/.Xauthority which should only be readable by that user and root. Really, you should check and make sure (more about this again later), because if anyone else can read this file, they can hijack your X connection and do all sorts of exciting things like keystroke logging et cetera because all X11 information is sent unencrypted.

So, what you can do (besides just using "su" which preserves the prior users environment, so avoids the problem) is something like the following...

#echo $DISPLAY
localhost:10.0
#su -
#cp /home/username/.Xauthority .
#chmod 600 .Xauthority
#DISPLAY=localhost:10.0
#export DISPLAY
#xterm

This should give you your xterminal on the original client display. There are lots of ways to do this, some probably more secure (and certainly many more elegant) than this, but as long as you REMEMBER TO CHMOD THE COPIED .Xauthority FILE this should be as secure as anything else, I think. Probably should delete it after you are done, just for fun.


This works fine for me. Sudo is necessary when you have access to sudo but do not have the root password.

I think more people might be running as root and then it is not a good idea to "cp /home/username/.Xauthority /root/"

Either do "cat /home/username/.Xauthority >> /root/.Xauthority"
or more correctly do:

su - donax -c " xauth list " > X11auth
xauth add $(<X11auth)

You may avoid this by changing /etc/sudoers so that it does not
change the home directory, or you can manually set
set -a
HOME=/home/username/


And by the way the xhost command is there to test if the DISPLAY
variable and authorization cookies work. Xterm is good, for sure,
but is not always present on e.g. Debian installations, like
xhost is not always available. In order to install basic X11
clients (programs) on Debian, use

aptitude install xbase-clients

:-)

xauth list

xauth add

export DISPLAY
