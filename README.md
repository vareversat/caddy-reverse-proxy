# How to make a reverse proxy
This little tutorial will help you (I hope) to create a reverse https proxy to redirect your incomming trafic from internet to a specific service (like on your NAS, access to Plex and your DSM with the same IP)

## Prerequisites
  - A Raspeberry Pi (or any computer you have with Ubuntu 18.04 LTS. Your gamming PC is way to powerfull, sorry...)
  - An internet connection (obviously)
  - ~ 30 mins
 
## Step 1 : Setup your ISP router
You need to redirect your incomming trafic to your reverse proxy server via NAT config (port fowarding). 

  - ***http*** :  `{INTERNET} ------------ 80 [ISP ROUTER] 80 ------------ [PROXY SERVER]` <br>
  - ***https*** : `{INTERNET} ------------ 443 [ISP ROUTER] 443 ------------ [PROXY SERVER]`
  
## Step 2 : Setup caddy
  1. Install ***caddy*** on your server : <br>
      `$ echo "deb [trusted=yes] https://apt.fury.io/caddy/ /" \` <br>
      `| sudo tee -a /etc/apt/sources.list.d/caddy-fury.list`. <br>
  2. Replace the default config file with the one provided : <br>
      `$ cd /etc/caddy` <br>
      `$ rm Caddyfile` **(as root)** <br>
      `$ git clone https://github.com/vareversat/caddy-reverse-proxy.git .` **(don't forget the dot at the end to prevent git to create a sub-dir)**

## Step 3 : Configure your Caddyfile
  This Caddyfile provide the following cofiguration :
  - `https://atlas.dev-o-sud.fr -----> DSM of my Synology NAS` (http://atlas:5000) <br>
  - `https://plex.dev-o-sud.fr -----> My plex server hosted on my NAS` (http://atlas:32400)<br>
  - `https://webdav.dev-o-sud.fr -----> Access to my WebDAV service on my NAS` (http://atlas:5005)<br>
  
  You'll need to change the hostname and the local address with your own and bind your public IP with your domain name
  
  Run `$ caddy stop && caddy start -watch` (-watch print you the logs directly into your terminal)
 
**Et voilà !**

## Troubleshooting
  If you get a `502 Bad Gateway error` make sure that you redirect to **http** and not to **https**. The reverse proxy need to fully handle the certificate check
