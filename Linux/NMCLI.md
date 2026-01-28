
# Overview 
The linux tool `nmcli` is a command-line tool used to control the NetworkManager (NM) service and manage network connections on Linux systems. It can be used for tasks such as creating, editing, deleting, and activating network connections, as well as monitoring and displaying device and connection status. It is a powerful tool that can perform the same functions as graphical network managers.

# General Usage
NMCLI is a large and robust command that includes many sections to groups functions together and different commands for each section.
```bash
nmcli [OPTIONS...] { SECTION } [COMMAND] [ARGUMENTS...]
```

# Section

nmcli groups similar functionality into different sections to organize the program. Each section may also include additional commands. Sections may also be referred to as Objects

## help


## general

### status
View the status of NetworkManager

```bash
nmcli general status
```

### hostname
View the current hostname or change the hostname to a new one if one is specified.

```bash
nmcli general hostname [HOSTNAME]
```

## networking
Call NetworkManager to view networking status and enable or disable networking.

### on
Enable NetworkManager control of networking functions
```bash
nmcli networking on
```
### off
Disable NetworkManager control of networking functions and deactivate adapters
```bash
nmcli networking off
```
### connectivity
Check the connectivity state of NetworkManager to check for internet access and optionally specifify check to continously check. 

> [!NOTE]
> Connectivity can be in one of 5 states
> **none** - No network is connected
> **portal** - Network connectivity but internet access blocked by captive portal
> **limited** - Network connectivity but no access to the internet
> **full** - Network connectivity with internet access
> **unknown** - Unable to determine network connectivity status

```
nmcli networking connectivity [check]
```
## radio
Manage NetworkManager controlled radios like WiFi and Cellular modems.

### wifi
Enable, disable, and view the status of WiFi functionality

```
nmcli radio wifi [on | off]
```

### wwan
Enable, disable, and view the status of Cellular functionality

```
nmcli radio wwan [on | off]
```


## connection

## device



## Status Checks

```bash
iwconfig # Check available radios
sudo systemctl status NetworkManager # Confirm NM is running
sudo systemctl start NetworkManager # Start NM if not running
sudo nmcli # Print NM general status of all adapters (ip addresss, connection state, etc.) 
sudo nmcli device # Get NM Device Status (Device, Type, State, Connection) 
```

## Manipulate the Radio up/down
```bash
sudo nmcli radio wifi off # Similar to ip link set wlan0 down
sudo nmcli radio wifi on # Similar to ip link set wlan0 up
```

## Connect to a new SSID (Create a .nmconnection/profile) 
```bash
sudo nmcli device wifi # Get WLAN scan report (BSSID, SSID, Mode, Channel, Rate, Signal/Bars, Security) 
sudo nmcli device wifi rescan # Refresh WLAN scan report
sudo nmcli device wifi connect <profile> -a   # Connect to specified SSID; ask for password (do not print to screen/history)
sudo nmcli device; iwconfig # Confirm association to SSID, IP address, etc.
ls /etc/NetworkManager/system-connections/ # Observe that a new .nmconnection was created

```

## Disconnect using a WiFi Profile
```bash
sudo nmcli connection down <profile>
sudo nmcli dev; iwconfig # Confirm disassociation from SSID, etc.
```

## Connect Using a Wifi Profile 
```bash
## Connect to a .nmconnection/profile
sudo nmcli connection # View all available (profiles)
sudo nmcli connection up <profile> # Connect to profile 
# OR sudo nmcli connection up <profile> ifname wlan0 # Specify the radio to associate to a network 
```

## Delete a .nmconnection/profile
```bash
sudo nmcli connection delete <profile>
```

## Edit & Reload a .nmconnection/profile
```bash 
sudo nmcli connection modify <profile> 802-11-wireless.ssid <New SSID> # Change SSID, OR
sudo nmcli connection modify <profile> 802-11-wireless-security.psk <New Passphrase> # Change Passphrase, OR
sudo nmcli connection modify <profile> 802-11-wireless.cloned-mac-address random # Enable MAC Spoofing
sudo nmcli connection reload # 'Restart' NetworkManager with the updates
```
