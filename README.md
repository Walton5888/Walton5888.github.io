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
 ## Step 6: Connecting Wireguard VPN to a Smartphone.
 * Go to your phone's app store and download the Wireguard app.
 * Return to your terminal on your computer.
 * Type this into your terminal to generate both the public and private keys: wg genkey | tee privatekey | wg pubkey > publickey
 * Then, type this in to create your wireguard configuration file: nano wg0.conf
 * In the file, put your credentials (public, private key) in the file. Your file should look something like this: 
[Interface]
PrivateKey = <server_private_key>
Address = 10.0.0.1/24
ListenPort = 51820

[Peer]
PublicKey = <client_public_key>
AllowedIPs = 10.0.0.2/32
* Note: Put your own keys into the PrivateKey and Public Key sections
* To get the encoding capabilites to generate a QR code for your app, type in this command: sudo apt-get install qrencode
* Type in this command to generate the QR code: qrencode -t ansiutf8 < wg0.conf
* Open the Wireguard app on your phone
* Place your phone's camera at the QR code in your terminal.
* You should now have access to the Wire Guard tunnel on your phone.
* Ensure that the VPN is activated on your smartphone by checking your phone's settings.
![IMG_2130](https://github.com/Walton5888/Walton5888.github.io/assets/110494531/1558f90c-db2e-48e9-ba0e-6d2bf18553c7)

