# `Grafana` - Data visualization frontend

Source for Raspberry Pi -

<https://grafana.com/tutorials/install-grafana-on-raspberry-pi/>

## 1. Install the Public Key

```sh
wget -q -O - https://packages.grafana.com/gpg.key | sudo apt-key add -
```

## 2. Add the Sources `Grafana`

```sh
echo "deb https://packages.grafana.com/oss/deb stable main" | \
sudo tee -a /etc/apt/sources.list.d/grafana.list
```

## 3. Install the Package

```sh
sudo apt update
sudo apt install grafana
```

## 4. Start `Grafana` Server

```sh
sudo systemctl start grafana-server
```

## 5. Enable to start every time at boot

```sh
sudo systemctl enable grafana-server
```

## 6. OPTIONAL - Enable Port on Firewall

```sh
sudo ufw limit 3000
```

## 7. One time configuration

Open the URL

`http://<RaspberryPi-IP>:3000/`

To login for first time:

> ```
> Login Name: admin
> Password  : admin
> ```

!!! note
    **Change the Password** as soon as you login. Its **IMPORTANT** to do this.

----
<!-- Footer Begins Here -->
## Links

- [Back to Raspberry Pi Hub](./README.md)
- [Back to Linux Hub](../README.md)
- [Back to Root Document](../../README.md)
