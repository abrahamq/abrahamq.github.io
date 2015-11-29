#How I set up my arch machine to use it as a node serve: 

First up, I installed ufw, a firewall that is supposed to be easy to use- the name literally means 
"Uncomplicated FireWall". Here are the commands I ran to get ufw set up: 

sudo pacman -Sy ufw 
sudo ufw allow SSH    - so my current ssh connection doesn't get cut off :) 
sudo ufw allow 80/tcp - allow net trafic to hit the machine 

sudo ufw enable 
sudo systemctl enable ufw -set to run at boot 

--- and that's the end of ufw configuration

Next up I installed nginx: 

sudo pacman -Sy nginx 
sudo systemctl enable nginx.service


--- I already had node installed so I didn't have to do anything about that, but you 
might have to make sure that node is installed and up to date. 

next up, I configured nginx. The node app is listening on port 8080, so we have to tell 
nginx to forward port 80 traffic to 8080. 

do: sudo vim /etc/nginx/ngnix.conf

and now we're going to modify one of the server blocks to point to our own location: Here's what 
my server blocks look like for this server, which has a domain of abrahamq.mit.edu

    server {
        listen       80;
        server_name  abrahamq.mit.edu;

        #charset koi8-r;

        #access_log  logs/host.access.log  main;

        location / {
            proxy_pass http://localhost:8080;
        }
    } 


next up, I installed pm2, a process manager for node. It will make sure that node gets restarted if it 
if the app crashes. 

npm install -g pm2

Now I pulled in the node project with git and did: 

pm2 start <app.js> (in reality I have a process.json that describes some more stuff that I want pm2 to do). 

Now you should be able to see the app at your domain. 
