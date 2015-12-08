# docker-x11-xpra
A docker image proving a basis for X applications accessible via ssh with or 
without xpra and optionally within Xnest/ratpoison


## Usage example

Run with

    docker run -p 2020:22 -d --name x11-xpra reto/x11-xpra 

Copy public key
    
    docker exec -i x11-xpra /bin/bash -c 'cat > /root/.ssh/authorized_keys' < ~/.ssh/id_rsa.pub

Start xclock without xpra

   ssh -X -p 2020 root@localhost xclock

Or start xclock as xpra process 100

   ssh -X -p 2020 root@localhost xpra start :100 --start-child=xclock

And attach to it with
  
   xpra --ssh="ssh -p 2020" attach ssh:root@localhost:100

Or start Xnest as display 200 on a new xpra display 111

   ssh -X -p 2020 root@localhost xpra start :111 --start-child=\"Xnest :200 -ac -geometry 800x600+24\"

Start ratpoison as display manager

   ssh -X -p 2020 root@localhost DISPLAY=:200 ratpoison & 

And xcloxk

   ssh -X -p 2020 root@localhost DISPLAY=:200 xclock &

And attach to it with
  
   xpra --ssh="ssh -p 2020" attach ssh:root@localhost:111

You may need to adapt the keyboard layout

    ssh -X -p 2020 root@localhost DISPLAY=:200 setxkbmap -layout ch

