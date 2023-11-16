# `node-red` - IoT Control Logic Visual Programming

This is done using the [Node-Red Installer project][1] from [Github][1].

Specifically looking at the [Raspberry Pi Installer build][2] running the `node-red-pi-install.sh`.

Reference:

- Rpi `node-red` + MQTT + InfluxDB + Grafana install:

   <https://www.superhouse.tv/41-datalogging-with-mqtt-node-red-influxdb-and-grafana/>

- Basic Configuration Tutorial for `node-red`

  <https://www.youtube.com/watch?v=AWzi4VrqTmw>

- Advance training on GUI & Dashboard configuration and flows in `node-red`:

    <https://www.youtube.com/watch?v=q0Ps5SfwAos>

    Install Needs:

    - `node-red-contrib-msg-resend`
    - `node-red-contrib-ui-led`
    - `node-red-contrib-ui-level`
    - `node-red-dashboard`
    - `node-red-node-wol`
    - `node-red-contrib-string`

- Creating Custom node in `node-red`

    <https://techeplanet.com/how-to-create-custom-node-in-node-red/>

- Random Numbers in `node-red` custom node

    <https://techeplanet.com/node-red-hello-world-flow/>


## 1. Installation on `node-red` on Raspberry Pi

```sh
bash <(curl -sL https://raw.githubusercontent.com/node-red/linux-installers/master/deb/update-nodejs-and-nodered)
```

This would run the script. You would need to answer few acknowledgments.
Apart from selecting [Nodejs install](./nodejs.md).
However its better to rely on our already [installed LTS Nodejs](./nodejs.md) version.

!!! note
     We can also do a local only installation using the direct install command:
    ```sh
    sudo npm install -g node-red
    ```

## 2. Configuration of `node-red`

This command needs to run as the user you wish to run your *Node-Red* flows.

```sh
# Initialize the Admin Configuration
node-red admin init
```

Its always a good idea to create a dummy user with less permissions for this case.

### Enable Node-Red services to auto-start

```sh
sudo systemctl enable --now nodered.service
```

### Optionally Enable the port on the Firewall

```sh
sudo ufw limit 1880
```



 [1]:https://github.com/node-red/linux-installers/
 [2]:https://github.com/node-red/linux-installers/tree/master/pibuild


----
<!-- Footer Begins Here -->
## Links

- [Back to NodeJs Main Article](./nodejs.md)
- [Back to Raspberry Pi Hub](./README.md)
- [Back to Linux Hub](../README.md)
- [Back to Root Document](../../README.md)