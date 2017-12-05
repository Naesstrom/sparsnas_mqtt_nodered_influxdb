# sparsnas_mqtt_nodered_influxdb
A node-red flow to transfer from Sparsnas mqtt script to influxdb

This uses the mqtt script from  @tubalainen found [here](https://github.com/tubalainen/sparsnas_decoder) and splits up the json string to each value and puts it in a influxdb database.

## Things you need
- Lots of things, look at the link above to get the mqtt interface working.
- A node-red installation
- A influxdb database

## Setup
1. Create a new Database in your influxdb install named sparsnas _(or name it something else but remember it for later or just skip it, I like to keep them separated)_
2. Import the node-red flow and it should look something like this:
![node red flow](https://i.imgur.com/DEUuXMx.png)
3. In each of the functions replace the IP adress in _"http://192.168.1.15:8086/write?db=sparsnas"_ with the correct adress to your influxdb adress _(This is also where you need to change the db name if you didn't follow my suggestion in 1.)_
4. That's it. Now it should start to populate your database with the values from your Sparsnäs energy meter.

## Bonus

So why would you want to go through with this... well because you can make all sorts of fun stuff in graphana with the data. The functions in the node red flow looks like this:
```
msg.url ="http://192.168.1.15:8086/write?db=sparsnas";
msg.payload = "sparsnas" + ",measurement=Watt " + " value=" + msg.payload.Watt;
return msg;
```
First row is just the http post adress for the influxdb with the targeted database

The second row sets the payload to be the sparsnas measurement, adds a **tag** that's the name of measurement _(ie. Watt, kWh, Battery or FreqErr)_ followed atlast by the value.
That way even if you don't use a separate DB you won't get them mixed up with other values and you can easily select what measurements you want to show like this:
![graphana image](https://i.imgur.com/SNb6yiw.png)
