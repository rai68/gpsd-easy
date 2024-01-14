How to install a basic gpsd for this
 
1. install the python file. 
2. find the serial link of your GPS and its baudrate and add them to your config
3. get internet and load the plugin
 


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
