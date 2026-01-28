
## Contents
1. [[#BT Interface Manipulation]]
2. [[#blue_hydra]]
3. [[#blue_sonar]]
4. [[#bluelog]]
5. [[#blueranger]]

### BT Interface Manipulation
```bash
# List all BT interfaces
sudo hciconfig --all

# Place specified interface into up/operational mode

sudo hciconfig <INTERFACE> up

# Confirm interface is up
sudo hcitool dev
```

### blue_hydra

Utility to scan for Bluetooth devices

SYNTAX:

```bash
sudo ./path/to/blue_hydra 
```

EXAMPLES:

```bash
# Install bluehydra
sudo apt install blue_hydra

# Record bluetooth device info to a sqlite3 database
sudo blue_hydra

# Triage output with GUI
sqlitebrowser blue_hydra.db

# Triage output with CLI 
sqlite3 blue_hydra.db 'select address from blue_hydra_devices'

# Also, for Device names, consider outputting the device name
sqlite3 blue_hydra.db 'select name, address from blue_hydra_devices'


# sqlite note1: Learn tables within DB
sqlite3 blue_hydra.db '.tables'
# sqlitenote2: Learn columns within table
sqlite3 blue_hydra.db 'PRAGMA table_info(blue_hydra_devices)'

```

### blue_sonar

Active Bluetooth location utility that reports RSSI of specified device

SYNTAX:

```bash
sudo ./blue_sonar -t <TARGET> -s <INTERVAL> -i <INTERFACE> 
```

OPTIONS:

    --target <TARGET> (-t <TARGERT>)
        Specify target BD_ADDR
    --sleep <INTERVAL> (-s <INTERVAL>
        Specify sleep period in seconds (default is 1)
    --interface <INTERFACE> (-i <INTERFACE>)
        Specify interface 
    

EXAMPLES:

```bash
# Actively ping aa:bb:cc:11:22:33 device with hci interface
sudo ./blue_sonar -t aa:bb:cc:11:22:33 -s .25 -i hci1 
```

### bluelog

Utility to scan for Bluetooth devices

SYNTAX:

```bash
bluelog -i <INTERFACE> [OPTION]
```

OPTIONS:

    -i
        Specify interface
    -o
        Specify output file name
    -v
        Add verbosity (print to scren)
    -n
	    Write device name to log
    -m
	    Write device manufacturer to log

EXAMPLES:

```bash
# Install bluelog
sudo apt install bluelog

# Scan for Bluetooth device with hci1, log output to devices.txt, print to screen, and include BDADDR and Device name within log
bluelog -i hci1 -o devices.txt -v -nm 
```


### blueranger

Active Bluetooth location utility that reports RSSI of specified device

SYNTAX:

```bash
blueranger <INTERFACE> <BDADDR>
```


EXAMPLES:

```bash
# Install blueranger
sudo apt install blueranger

# Actively ping aa:bb:cc:11:22:33 device with hci interface
blueranger hci1 aa:bb:cc:11:22:33
```

