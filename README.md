# TICK-Stack-docker-compose

Docker compose files to set up the TICK stack.

The TICK stack consists of 4 programs:
(T)elegraf - this is a daemon which runs on any computer on your network and sends metrics to your Influx DB instance running on your server
(I)nflux - a time-series database which gathers your metrics and is capable of aggregating them via time based queries
(C)hronograf - a web-based app which is used to query the data in Influx and display it in custom notebooks/dashboards. It can also be used to explore your Influx database and to set up alarms in Kapacitor
(K)apacitor - manages custom alarms which you can set up for metrics in Influx

Apart from Telegraf, you can also send custom data to Influx from your own applications using the web API exposed by it. This would allow you to track anything you like (e.g. if you create your own weather station you can track everything weather related and aggregate the data per second/minute/hour/day/week/month or any timespan really).

## Server

The files in the server directory consist of the docker-compose file to get the stack up and running on your server, the influx meta configuration file which configures the save location for the Influx DB data files, the init Influx query file which creates the metrics database for Telegraf when Influx initialises, and the telegraf configuration file which configures what is tracked on the machine. A more in-depth file can be found under the Node files.

When running docker-compose, export the HOSTNAME as an environment variable so that the correct host name is shown in Chronograf and also set the influxdb username and password in the docker-compose file. You will need these in order to connect it to Chronograph.
```
export HOSTNAME=my-hostname
export INFLUX_USERNAME=admin
export INFLUX_PWD=mySecurePassword
docker-compose up -d
```

You can access Chronograf by going to `http://{IP_ADDRESS_OF_SERVER}:8088` in a browser.

## Node

The files in the Node directory are designed to be run on any other computer on your network, other than the TICK server. This will allow other computers to send metrics to the TICK server so you can see how all your computers on the network are performing.

When running, enter your server's IP address and node's hostname as environment variables:
```
export HOSTNAME=my-hostname
export SERVER_IP=192.168.1.100
docker-compose up -d
```

Note: The server and node telegraf configs are a little different. Please edit them to your desired config.
