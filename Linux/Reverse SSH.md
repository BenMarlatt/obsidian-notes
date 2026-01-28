A reverse SSH tunnel is a method where you create a secure connection from a remote server back to your local machine. It allows someone to connect to your local machine through the remote server, effectively bypassing firewalls or NAT. This is useful for giving access to a service on your local machine that isn't directly accessible from the outside internet.

SYNTAX:

```bash
ssh -f -N -R <REVERSE PORT>:localhost:<LOCAL PORT> <VPS SSH INFO>
```


OPTIONS:

    -f
        Run in the background
    -N
	    No remote commands expected
    -R
	    Create a Reverse tunnel
   

**EXAMPLES:**

```bash
# Example Details:
# RPI - SSH Port=22
# VPS - SSH Port=2222; User=Muppet; IPv4=123.123.123.123
# Desired Reverse Port=9001

# From RPI, establish a reverse SSH port (port 9001) on VPS (port 2222) that redirects back to RPI (port 22). This opens port 9001 on the VPS and exposes port 22 on the RPI on the new port.

ssh -f -N -R 9001:localhost:22 Muppet@123.123.123.123 -p 2222

```

**Reverse SSH Script**
```
#!/bin/bash
# prompt colors
    red=$'\e[1;31m'
    green=$'\e[1;32m'
    bold=$'\e[1m'
    reset=$'\e[0m'
# status indicators
    greenplus='\e[1;33m[++]\e[0m'
    greenminus='\e[1;33m[--]\e[0m'
    redminus='\e[1;31m[--]\e[0m'
    redexclaim='\e[1;31m[!!]\e[0m'
    redstar='\e[1;31m[**]\e[0m'
    blinkexclaim='\e[1;31m[\e[5;31m!!\e[0m\e[1;31m]\e[0m'
    fourblinkexclaim='\e[1;31m[\e[5;31m!!!!\e[0m\e[1;31m]\e[0m'
clear

read -p "Pi SSH Port: " PIPORT
read -p "Pi User: " PIUSER
read -p "$red NEW $reset Cloud Redirect Port: " NEWPORT
read -p "Cloud User: " CLOUDUSER
read -p "Cloud IP: " CLOUDIP
read -p "Cloud SSH Port: " CLOUDPORT


clear
echo -e "$redstar Please confirm the following: $redstar"
echo -e "Pi SSH Port:$bold $PIPORT $reset"
echo -e "Pi User:$bold $PIUSER $reset"
echo -e "NEW Cloud Redirect Port:$bold $NEWPORT $reset"
echo -e "Cloud User: $bold $CLOUDUSER $reset"
echo -e "Cloud IP: $bold $CLOUDIP $reset"
echo -e "Cloud SSH Port: $bold $CLOUDPORT $reset"
echo " "
read -n 1 -r -s -p $"$green Press enter to continue if the values above are correct. Otherwise Ctrl + c to reenter $reset "
clear
echo -e "Copy/Paste the following to build a reverse SSH tunnel from your Pi to the cloud VPS:"
echo " "
echo -e "$green ssh -f -N -R $NEWPORT:localhost:$PIPORT $CLOUDUSER@$CLOUDIP -p $CLOUDPORT $reset"
echo " "
echo -e "After execution, SSH into the Cloud VPS:"
echo " " 
echo -e "$green ssh $CLOUDUSER@$CLOUDIP -p $CLOUDPORT $reset"
echo " " 
echo -e "After SSHing into Cloud VPS, run the following to access the Remote Pi via reverse SSH tunnel:"
echo " "
echo -e "$green ssh $PIUSER@localhost -p $NEWPORT $reset"
```