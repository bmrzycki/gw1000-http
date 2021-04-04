# Ecowitt GW1000 HTTP REST API front-end server
The [Ecowitt GW1000](https://www.ecowitt.com/shop/goodsDetail/16) is an inexpensive Wifi enabled temperature/pressure/humidity gateway and sensor with support for external temperature/humidity probes. By default you must access the data via their external website or a phone/tablet app. This server gives you access via a more traditional REST API over HTTP to read temperature, pressure, and humidity values using plain text or JSON. When used in conjunction with [Homebridge](https://homebridge.io/) and a plugin like [homebridge-http-temperature-sensor](https://github.com/Supereg/homebridge-http-temperature-sensor) you can enable all temperature and humidity values in Apple's HomeKit.

The GW1000 supports a large number of external devices but this server currently only supports the outdoor sensor (WH32) and the multi-channel sensor (WH31).

## Installation
The code is Python 3.x and requires no external libraries to run. You can copy it anywhere you like or even run it from the cloned repo. If you run the server as a daemon it's a good idea to redirect stderr/stdout to
log files.

## Usage
By default the server uses HTTP port `9000` and listens on all interfaces and attempts to locate the GW1000 on your local network via UDP broadcast. You can also force the IP and PORT of the GW1000 on the command line. To reduce traffic to the GW1000 data is cached inside the server for a `poll` interval in seconds and can also be altered on the comand line at startup. 

## API

The root `/` URI returns information about every known device. If only a single device is desired one can add to the URI `/device_name`. Any per-device key can be accessed in the same way: `/device/key`.  For example, if you only wanted the external sensor channel_1's temperature: `http://localhost:9000/channel_1/temperature`.

By default all queries are returned as MIME type `text/plain`. You can request JSON for easier programming: `http://localhost:9000/channel_1/temperature?format=json`.

Any unknown device or key will return an HTTP `404` error.

# Units
The units of all values are unaltered from the Ecowitt which are SI units. The client is expected to convert to any other unit. The default units:
* temperature: Celsius -> C
* humidity: percentage from 0-100 -> %
* pressure: hectopascal -> hPa
