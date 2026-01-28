# Install Kismet and Dependencies
```bash
sudo apt install kismet gpsd
```

## Raspberry PI Install
> [!CAUTION]
> The Raspberry Pi OS does not include Kismet in apt's default repositories. It must be added manually. Details for specific distributions can be found [here](https://www.kismetwireless.net/packages/).

```bash
# Download and install Kismet repo key.
wget -O - https://www.kismetwireless.net/repos/kismet-release.gpg.key --quiet | gpg --dearmor | sudo tee /usr/share/keyrings/kismet-archive-keyring.gpg >/dev/null

# Update apt's repos to include kismet's debian trixie releases.
echo 'deb [signed-by=/usr/share/keyrings/kismet-archive-keyring.gpg] https://www.kismetwireless.net/repos/apt/release/trixie trixie main' | sudo tee /etc/apt/sources.list.d/kismet.list >/dev/null

# Update to allow apt to search the new repos
sudo apt update

# Install Kismet
sudo apt install kismet
```
---
# Setup GPSD

## Configure GPSD
Plug in the USB GPS device and check to see what device it attaches to on the file system (ttyUSBX) it will look something like this. `[92259.137738] usb 1-2.2: pl2303 converter now attached to ttyUSB0`

Edit GPSD configuration file at /etc/default/gpsd and add the full path to where the device is attached to (/dev/ttyUSBX). Also add an extra line `START_DAEMON="true"` Your file should look like this in the end
```bash
#/etc/default/gpsd
DEVICES="/dev/ttyUSB0"
GPSD_OPTIONS=""
USBAUTO="true"
START_DAEMON="true"
```

## Start GPSD
```
# Start GPSD
sudo systemctl start gpsd

# Optionally, start GPSD on boot
sudo systemctl enable gpsd
```

We can verify GPSD is working using using the `cgps` command. This will display GPS time, location, and connected satellites. When cold starting a GPS device, it can take up to 20 minutes to get a full 3D fix. If it is working, there should be GPS info being logged at the bottom.

```bash
# Run cgps to verify functionality
cgps
```
---
# Setup Kismet

## Configure kismet_site.conf
In the /etc/kismet/ directory, we can create a new file called `kismet_site.conf` to add additional user settings to kismet. This allows us to customize our experience and change defaults to include:
1. sources devices - WiFi, Bluetooth, RTL-SDR
2. gps - what method to use to get GPS data
3. Logging location - Where to save Kismet data
Multiple sources can be added but only one device can be attached to a source.

```bash
# kismet_site.conf

# Add wlan0 as a WiFi datasource with default settings
source=wlan0

# Add wlan0 but monitor only a single channel (6)
source=wlan0:channel_hop=false,channel=6

# Add wlan0 but monitor only the 3 2.4ghz non-overlapping channels
source=wlan0:channels="1,6,11"

# Add wlan0 but scan only the 2.4ghz WiFi channels
source=wlan0:band24ghz=true

# Add hci0 as a Bluetooth datasource
source=hci0:type=linuxbluetooth

# Use gpsd as a GPS source
gps=gpsd:host=localhost,port=2947

# Setup logging filetype and location
log_types=kismet
log_prefix=/home/<USER>/kismet_files # This directory MUST exist first
```


## Start Kismet
```
# Start Kismet
sudo systemctl start kismet

# Optionally start Kismet on boot
sudo systemctl enable kismet
```

Open a browser and navigate to http://localhost:2501 or http://<YOUR_IP>:2501
# Triage Data
Kismet will generate a sqlite3 database file with the .kismet extension in the directory specified in the log_prefix configuration option. This .kismet file stores the most amount of data versus the other file formats Kismet supports. This allows us to convert the .kismet file to other formats but takes up much more space

## Fix Corrupted Data
A .kismet-journal file may be created to store temporary data while Kismet is running. If kismet is ended gracefully (sudo systemctl stop kismet or a proper shutdown), this journal file will be cleaned up properly but if kismet is ended abruptly (killall -9 kismet or powerloss) the journal file may still be on the file system and the .kismet file may be corrupted. to fix this we can run one of two commands.

```bash
# Use sqlite3 to fix kismet file
sudo sqlite3 <FILENAME>.kismet 'VACUUM;'

# Use kismetdb_clean to fix kismet file
sudo kismet_clean --in <FILENAME>.kismet
```

## Data Conversion

a .Kismet formatted file can be converted to multiple different fromat including KML and PCAP. KML is useful to import into mapping software like google earth or GPS prune. PCAP is useful to import into wireshark to see the raw packets if the source supports it. In the case of WiFi and channel hopping we may be capturing partial conversations due to the channel changing. PCAP capture is best when channel hopping is turned off.

```bash
# Convert .kismet file to .kml
sudo kismetdb_to_kml --in <FILENAME>.kismet --OUT <FILENAME>.kml

# Convert .kismet file to .pcap
sudo kismetdb_to_pcap --in <FILENAME>.kismet --OUT <FILENAME>.pcap
```

## Replay Capture

Kismet can replay a capture as if it was capturing it again real time.
```bash
sudo kismet -c <FILENAME>.kismet
```