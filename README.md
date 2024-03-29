# 🚨 This guide is deprecated. Please visit [this](https://github.com/Gabisonfire/knightcrawler) repository to setup KnightCrawler. 🚨
## [Torrentio-sh](https://github.com/Gabisonfire/torrentio-scraper-sh) prerequisites:

 1. Linux on a machine (Preferably Ubuntu or Debian based and a server based distribution with SSH enabled). [Here's a great video tutorial by TechHut on how to do this.](https://www.youtube.com/watch?v=K2m52F0S2w8) 
 
 2. A static local IP on your Linux machine. [Here's a great video tutorial.](https://www.youtube.com/watch?v=fayx4jWqyWk) (your router's IP is usually `http://192.168.0.1` or `http://192.168.1.1`)
 
 3. A [DuckDNS](https://www.duckdns.org/) account with ports 80 TCP and 443 TCP forwarded on your router. [Here's a great video tutorial](https://www.youtube.com/watch?v=B9jH8QPsVOw). ****Note: This is only required for remote access outside of your home wifi.****
 
 4. Patience lol

### ⚠️ BE SURE TO COPY AND PASTE THE BLOCKS OF COMMANDS 1 BLOCK AT A TIME AND WAIT BEFORE EACH ONE IS DONE BEFORE STARTING ANOTHER ⚠️

Add the Docker apt repository or see the [Docker installation guide](https://docs.docker.com/engine/install/ubuntu/) on Docker's website.

```bash
for pkg in docker.io docker-doc docker-compose docker-compose-v2 podman-docker containerd runc; do sudo apt-get remove $pkg; done
sudo apt-get update
sudo apt-get install ca-certificates curl
sudo install -m 0755 -d /etc/apt/keyrings
sudo curl -fsSL https://download.docker.com/linux/ubuntu/gpg -o /etc/apt/keyrings/docker.asc
sudo chmod a+r /etc/apt/keyrings/docker.asc
echo \
  "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.asc] https://download.docker.com/linux/ubuntu \
  $(. /etc/os-release && echo "$VERSION_CODENAME") stable" | \
  sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
sudo apt-get update
```
Install Docker:
```bash
sudo apt-get install docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin docker-compose
```
Add your user to the `docker` group or see the [post-installation](https://docs.docker.com/engine/install/linux-postinstall/) step on Docker's website:
```bash
sudo groupadd docker
sudo usermod -aG docker $USER
```
Reboot to finalize the Docker install:
```bash
sudo reboot
```

## Install Torrentio-sh

Clone the Torrentio-sh repository:
```bash
sudo apt update
sudo apt install -y git
cd ~
git clone https://github.com/Gabisonfire/torrentio-scraper-sh
cd torrentio-scraper-sh
docker compose up -d
```
You can view your Torrentio-sh instance in a browser at  `http://yourip:7000`.

You can view your machine's IP with `ifconfig`  or `ip a`.

## Enable auto update

You can enable auto update for your Torrentio-sh instance by running these commands:

```bash
docker run -d \
  --name watchtower \
  -v /var/run/docker.sock:/var/run/docker.sock \
  containrrr/watchtower
```

## Make Torrentio-sh open to the internet. (External access)

### Setup DuckDNS

 1. Make a new subdomain on DuckDNS.org.
 2. Click `install` in the upper left corner.
 3. Select your newly created domain from step 1 in the drop-down box.
 4. Proceed to copy all commands in order just like you have in this tutorial.

Don't forget to forward ports TCP 80 and TCP 443 to your Linux machine's IP address before continuing. [Here's a great video tutorial.](https://www.youtube.com/watch?v=B9jH8QPsVOw)

### Next setup the reverse proxy. 
This is what sends Torrentio-sh uses to talk to other devices across the internet. We will use Caddy as a reverse proxy.

Start by downloading the Caddy docker-compose.yaml file and starting the stack:
```bash
cd ~
git clone https://github.com/ben-2357/Torrentio-sh-Setup-Guide
mv ~/Torrentio-sh-Setup-Guide/Caddy ~/Caddy
sudo rm -r Torrentio-sh-Setup-Guide
```
Replace `subdomain.duckdns.org` ,with your previously chosen DuckDNS subdomain, in the Caddyfile.
```bash
nano ~/Caddy/Caddyfile
```
Now hit `Ctrl + x` then `y` then `enter`

Finally start Caddy and visit your Torrentio-sh instance at `yoursubdomain.duckdns.org`:
```bash
cd ~/Caddy
docker compose up -d
```

 Great job for making it through the whole guide.  
# 🎉 Happy streaming 🎉
