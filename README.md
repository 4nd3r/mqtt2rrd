# mqtt2rrd

Collect numeric values from MQTT to round-robin databases and create graphs.

This is work in progress-ish, but already serves the purpose. You have been warned.

## Dependencies

* `sh` with some GNU utils
* `mosquitto_sub`
* `rrdtool`

## Environment variables

* `MQTT2RRD_BROKER` =  `localhost`
* `MQTT2RRD_DATABASES` = `rrd`
* `MQTT2RRD_GRAPHS` = `public`
* `MQTT2RRD_DEBUG`
* `MQTT2RRD_DRYRUN`

## Configuring topic and database

```sh
$ mqtt2rrd config
Usage: mqtt2rrd config <mqtt_topic> <database_name> <GAUGE|COUNTER>
```

One database per topic.

No whitespace or use quoting, but `<mqtt_topic>` and `<database_name>` will be "normalized" anyway (see below).

## Databases directory layout

```sh
$ ls -l rrd/*/*toilet* | grep -o 'rrd/.*'
rrd/by-id/zigbee2mqtt_toilet_humidity.rrd -> ../by-name/toilet_humidity.rrd
rrd/by-id/zigbee2mqtt_toilet_temperature.rrd -> ../by-name/toilet_temperature.rrd
rrd/by-name/toilet_humidity.rrd
rrd/by-name/toilet_temperature.rrd
```

## About file names

File names are created (aka normalized) using `__rrd_id` function:

```sh
$ mqtt2rrd rrd_id 'foo BAR/baz-topic'
foo_bar_baz_topic
```

Topics from broker are matched with same function.

Database file name (without extension) is used as graph title.
