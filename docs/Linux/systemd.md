# `systemd` - Management layer and glue of Linux

References:
- <https://www.digitalocean.com/community/tutorials/how-to-use-systemctl-to-manage-systemd-services-and-units>
- <https://www.cyberciti.biz/faq/systemd-systemctl-view-status-of-a-service-on-linux/>

## Clear `systemd` journal - a.k.a Logs

Reference: <https://ma.ttias.be/clear-systemd-journal/>

#### Find space taken up by Logs

```sh
sudo du -hs /var/log/
```

#### Clean up logs based on duration

```sh
sudo journalctl --vacuum-time=1d
```

Clean up everything older than 1day.

#### Clean up logs based on Size

```sh
sudo journalctl --vacuum-size=20M
```

Clean up logs above `20MiB` size.
Means expunge all that exceed this limit.

## List all currently active services in `systemd`

```sh
systemctl --type=service --state=active
```

This would present a list of active services.
Use `up` and `down` arrows to navigate and `q` to quit the listing.

## List all services that have failed in `systemd`

```sh
systemctl --type=service --state=failed
```

## List all Auto-Start services in `systemd`

```sh
systemctl list-unit-files
```

## Deactivate a service in `systemd`

```sh
sudo systemctl disable --now <service name>
```

This would both *disable the `<service name>`* and *unload it immediately*.

If you just want to disable it at next boot then remove the `--now` flag.

### Remove a `systemd` service completely

```sh
systemctl stop [servicename]
systemctl disable [servicename]
rm /etc/systemd/system/[servicename]
rm /etc/systemd/system/[servicename] # and symlinks that might be related
rm /usr/lib/systemd/system/[servicename]
rm /usr/lib/systemd/system/[servicename] # and symlinks that might be related
systemctl daemon-reload
systemctl reset-failed
```

Replace the `[servicename]` with the desired service.

For Reference from [Superuser.com](https://superuser.com/questions/513159/how-to-remove-systemd-services)
<https://superuser.com/a/936976>

## Check status and health of a service in `systemd`

```sh
sudo systemctl status -l <service name>
```

The `-l` flag would detail more while displaying the `<service name>` logs.

## Fix the `systemd` degraded status

Especially installing Manjaro Linux!

[Manjaro `vconsole.conf` fix](./ArchLinux/manjaro-vconsole-conf-fix-for-systemd.md)

## Time Synchronization services

Sometimes these [interfere with disk spin down](./spin-down-disks.md#systemd-timesycd---time-synchronization-services).

## `systemd` Timers

List out the [timers active here](./spin-down-disks.md#systemd-timers).

----
<!-- Footer Begins Here -->
## Links

- [Back to Linux Hub](./README.md)
- [Back to Root Document](../README.md)
