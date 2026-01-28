Guide on how to setup and build a Wireguard based VPN utilizing public cloud infrastructure.
# Setup Wireguard Server

Instructions for setting up a Wireguard server and client configuration files utilizing a public VPS
## Wireguard Server Install Script
Utilizing the [Angristan Install Script](https://github.com/angristan/wireguard-install) we can easily automate the Wireguard install process. This script will run a guided install to help configure Wireguard and manage users on subsequent runs of the script.
> [!IMPORTANT] 
> This script is ONLY to be ran on the Wireguard server.

```bash
# Download the Script
wget https://raw.githubusercontent.com/angristan/wireguard-install/master/wireguard-install.sh

# Run the script as root
sudo bash wireguard-install.sh
```
Once finished, the script will have installed Wireguard and created the first client configuration file.
## Generate additional client configurations
New users can be generated during subsequent runs of the script. Wireguard requires a dedicated configuration file for each device due to IP address and key overlap. 
> [!IMPORTANT]
> A Wireguard config file CANNOT be used on multiple devices, each device needs its own config file.

```bash
# Re-run the script as root to manage users
sudo bash wireguard-install.sh
```
---
# Wireguard Client Setup
Each devices that is to connect to the VPN will need two things
1. A wireguard application for the device's operating system
2. Be configured with a .conf file downloaded from the VPS to connect to the Wireguard server.

The assumptions for this document is the operation system is running Linux.
## Install required packages
```bash
# Install wireguard and dependencies
sudo apt install wireguard resolvconf curl
```
Curl is optional but helps with verification and troubleshooting

## Download and install the config file
On the client device
```bash
# Download configuration files from the VPS
scp -P <PORT> <USER>@<IP>:/PATH/TO/.CONF

# After downloading, rename .conf files to remove any '-' (dashes)
mv wg0-client-<CLIENTNAME>.conf <CLIENTNAME>.conf

# move .conf files to /etc/wirguard/
sudo mv <CLIENTNAME>.conf /etc/wireguard/
```

## Prevent connection from idling
By default, Wireguard is intended to be a quiet protocol but we can add an extra line to our configuration file to occasionally send a heartbeat packet to keep the connection alive. 
```bash
# Append an extra arguement to the end of the client's configuration file
echo "PersistentKeepalive = 25" >> <CLIENTNAME>.conf
```


The client configuration file is now installed correctly and the wireguard client can be managed using systemd (systemctl).
```bash
# Check the status of the VPN client and specify a config file (without the .conf)
sudo systemctl status wg-quick@<CLIENTNAME>

# Start the wireguard VPN client
sudo systemctl start wg-quick@<CLIENTNAME>

# Stop the wireguard VPN client
sudo systemctl stop wg-quick@<CLIENTNAME>

# Configure the wireguard VPN client to start on boot
sudo systemctl enable wg-quick@<CLIENTNAME>
```

# Verification and Troubleshooting

## Verify Connectivity
Using Ping, we can check our connectivity between connected devices on our VPN network. The client IP address are static and generated during configuration file generation. It is good practice to keep an inventory of which devices have what IP addresses, there should be no duplicate IP addresses.
```bash
# Test if the client is able to reach the server
ping 10.66.66.1

# Test if the client is able to ping other devices connected to the VPN
ping <IP_OF_OTHER_CLIENTS>
ping 10.66.66.2
ping 10.66.66.3
```

## Check Public Fingerprint
Utilizing Curl, we can verify how the internet views us (what is our public IP address and what region are we connecting from). It is useful to do this before and after connecting to the VPN.
```bash
curl ipinfo.io
```
[Browserleaks](https://browserleaks.com/)is another valid option if a browser is available to us on the client device to check more in-depth of our public fingerprint.
