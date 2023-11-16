# `influxdb` - Time Series Database

- Raspberry Pi `infuxdb` install

    <https://www.superhouse.tv/41-datalogging-with-mqtt-node-red-influxdb-and-grafana/>

- <https://docs.influxdata.com/influxdb/v2.1/install/?t=Raspberry+Pi>
- <https://citizix.com/how-to-install-and-configure-influxdb2-in-debian-11/>
- <https://github.com/nygma2004/km/wiki/InfluxDB-and-Grafana>
- InfluxDB Query help

    <https://docs.influxdata.com/influxdb/v1.8/query_language/>

- Good Influx DB Tutorial Video

    <https://www.youtube.com/watch?v=gb6AiqCJqP0>

- Down sampling

    <https://www.youtube.com/watch?v=j3x0TohyGJY>

- Retention Policy

    <https://docs.influxdata.com/influxdb/v1.8/query_language/manage-database/#retention-policy-management>

## Installation Process for `influxdb`

### 1. Add the Repository Key

```sh
wget -qO- https://repos.influxdata.com/influxdb.key | sudo apt-key add -
```
This command outputs the key over a pipe and then we add it using `apt`

### 2. Find the Distribution

```sh
lsb_release -a
```

### 3. Add the Release to the Source list

```sh
echo "deb https://repos.influxdata.com/debian buster stable" |\
 sudo tee /etc/apt/sources.list.d/influxdb.list
```

!!! note "Adjust the Release"
    Current the release shown here is `buster` this needs to change as per your installation.

### 4. Update the repositories

```sh
sudo apt update
```

### 5. Install the Package

```sh
sudo apt install influxdb
```

### 6. To start on command

```sh
sudo systemctl start influxdb
```
For automatic startup:

```sh
sudo systemctl unmask influxdb
sudo systemctl enable influxdb
```

### 7. `influxdb` create the first Admin User

```sh
influx
```

In the Influx Shell:

```sql
> CREATE USER admin WITH PASSWORD 'adminpassword' WITH ALL PRIVILEGES
> exit
```

!!! note
    The password here should be a good one else your account can get hacked

### 8. Edit configuration

```sh
sudo nano /etc/influxdb/influxdb.conf
```

> Press `CTRL+W` to search for the section called `[http]`.
> Then un-comment and edit :

```sh
...
auth-enabled = true
...
pprof-enabled = true
...
pprof-auth-enabled = true
...
ping-auth-enabled = true
```
Finally save the changes.

### 9. Restart the `influxdb` service

```sh
sudo systemctl restart influxdb
```

### 10. Login with admin user to check if the changes worked

```sh
influx -username admin -password 'adminpassword'
```

Some Commands:

```sql
#### See Database present
> show databases
#### This command should work now.
> CREATE DATABASE sensors
```

## Show Measurements or Tables being Recorded

```sql
> show measurement
```

## Show All Fields

```sql
> show field keys
```

## Show Fields of Particular measurement

```sql
> show field keys from "measurement_name"
```

## Show All Tags (Index)

```sql
> show tag keys
```

## Show Fields of Particular measurement

```sql
> show tag keys from "measurement_name"
```

## InfluxDB Deleting Columns

There is no way to delete a "column" (i.e. a field or a tag) from an Influx measurement. Here's the [feature request for that](https://github.com/influxdata/influxdb/issues/6150).

You'll have to `SELECT INTO` a different measurement, excluding the columns you don't want:

```sql
> SELECT useful_field, nice_tag INTO new_measurement FROM measurement
```

## Rename measurements or Tables

```sql
> SELECT * INTO "new-measurement" from "old-measurement"
```

We can also use a `where` clause to filter based on **time** or `field`

## Delete Measurement

```sql
> drop measurement "measurement-name"
```


----
<!-- Footer Begins Here -->
## Links

- [Back to Raspberry Pi Hub](./README.md)
- [Back to Linux Hub](../README.md)
- [Back to Root Document](../../README.md)
