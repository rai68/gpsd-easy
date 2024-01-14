How to install a basic gpsd for this
 
1. install gpsd/clients `sudo apt install gpsd gpsd-clients`
2. find the serial link of your GPS and its baudrate and add them to the file, comment every other line out.
 
## Example /etc/default/gpsd
```
GPSD_OPTIONS="-n -N -b"
BAUDRATE="9600" #<----- Baudrate edit this
MAIN_GPS="/dev/ttyS0" #<----- /dev/ edit this to be your device, if its serial based you will need to enable serial in `sudo raspbi-config` > Interfaces > Serial > No > Yes > Finish > Reboot
PPS_DEVICES=""
USBAUTO="false" #<----- set this to true, if you are using a USB based adapter and you might unplug it/replug it.
GPSD_SOCKET="/var/run/gpsd.sock"
/bin/stty -F ${MAIN_GPS} ${BAUDRATE}
/bin/setserial ${MAIN_GPS} low_latency
```
3. next run these commands, stops the current gpsd process if it started > `sudo systemctl stop gpsd.socket gpsd.service && sudo systemctl disable gpsd.socket` 
4. Create the gpsd.service > `sudo nano /etc/systemd/system/gpsd.service`
## Example /etc/systemd/system/gpsd.service
```
[Unit]
Description=GPS (Global Positioning System) Daemon for pwnagotchi
Requires=gpsd.socket
[Service]
EnvironmentFile=/etc/default/gpsd
ExecStart=/usr/sbin/gpsd $GPSD_OPTIONS $MAIN_GPS $PPS_DEVICES
[Install]
WantedBy=multi-user.target
Also=gpsd.socket
```
5. reload daemon `sudo systemctl daemon-reload`
6. start service and enable on boot `sudo systemctl enable gpsd.service && sudo systemctl start gpsd.service`
7. assuming no errors, tada enable plugin with default values below n it should work


# How to install gpsd with PPS time syncing (requires a non USB GPS that has a PPS pin, but will give you sub microsecond time accuracy (1.000_0##_###s))
to be written


## Example config
### Required, require but if they are not given the values below are defaults
```
main.plugins.gpsdeasy.enabled = true
main.plugins.gpsdeasy.host = '127.0.0.1'
main.plugins.gpsdeasy.port = 2947
main.plugins.gpsdeasy.device = '/dev/ttyS0' #<--- change to your device
main.plugins.gpsdeasy.baud = 9600 #<--- change to fit yuor device
```
# Optional, below values are defaults if not specified
```
main.plugins.gpsdeasy.fields = ['fix','lat','lon','alt','speed'] #<-- Any order or amount, you can also use custom values from POLL.TPV; on gpsd documents (https://gpsd.gitlab.io/gpsd/gpsd_json.html#_tpv)
main.plugins.gpsdeasy.speedUnit = 'kph' #or 'mph' or 'ms' #(m/s)
main.plugins.gpsdeasy.distanceUnit = 'm' #or 'ft'
main.plugins.gpsdeasy.bettercap = true #<--- report to bettercap
main.plugins.gpsdeasy.topleft_x = 130
main.plugins.gpsdeasy.topleft_y = 47
main.plugins.gpsdeasy.auto = true #<--- auto setup systemd service for gpsd. use false if using custom service and you know what you are doing.
```
