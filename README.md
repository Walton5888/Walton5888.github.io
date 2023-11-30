# Walton5888.github.io
## Step 1: Create a Digtial Ocean Account
Visit this link to create your account: https://www.digitalocean.com/
## Step 2: Create a VM (Droplet)
* Choose the create VM option
* Choose Ubuntu 20.04 as your OS
* Don't choose any add-ons
* Choose the $6 (1GB) plan
* Set a password
## Step 3: SSH into the VM 
* Access the terminal on your computer
* Type this in: ssh root@<your_ip_address>
* Replace your_ip_address with your actual ip address
* Type in your password, and you should have access to your droplet
## Step 4: Setting up Wireguard:
* Start with the following command to build a wireguard directory: mkdir -p ~/wireguard/
* Then, type in the following command to make the directory for the wireguard configuation: mkdir -p ~/wireguard/config/
* Then, make a docker compose file for wireguard with the following command: nano ~/wireguard/docker-compose.yml
* Paste the following content into the file:
version: '3.3'
services:
  wireguard:
    container_name: wireguard
    image: linuxserver/wireguard
    environment:
      - PUID=1000
      - PGID=1000
      - TZ=America/Menominee
      - SERVERURL=164.90.149.112
      - SERVERPORT=51820
      - PEERS=pc1,pc2,phone1
      - PEERDNS=auto
      - INTERNAL_SUBNET=10.0.0.0
    ports:
      - 51820:51820/udp
    volumes:
      - type: bind
        source: ./config/
        target: /config/
      - type: bind
        source: /lib/modules
        target: /lib/modules
    restart: always
    cap_add:
      - NET_ADMIN
      - SYS_MODULE
    sysctls:
      - net.ipv4.conf.all.src_valid_mark=1
 * Note: you will need to change the server url to your droplet's IP address, and TZ to your timezone.
 ## Step 5: Running Wireguard
 * Type in the following command to enter your wireguard directory: cd ~/wireguard/
 * Type in the following command to run your docker compose file: docker-compose up -d
 * Wireguard should be running now. 
