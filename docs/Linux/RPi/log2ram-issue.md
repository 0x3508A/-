# Log2Ram error 1071

Reference: <https://jmwoccassionalnotes.blog/2022/06/18/log2ram-var-hdd-log-too-small/>

You might sometimes get the following error message:

```sh
log2ram[1071]: ERROR: RAM disk for "/var/hdd.log/" too small. Can't sync.
```

And this would not allow the `Log2Ram` service to start.

## Why the error 1071

Typically on first install the `Log2Ram` tires to directly copy all data available in `/var/log`. This may some times be a problem.
Since by default `Log2Ram` has only `40MiB` of space.
And, if the `/var/log` size is `>40MiB` you get the above error.


#### Use `du` to look at Log Directory size

First we look at the Log directory size:

```sh
$ du /var/log -h
du: cannot read directory '/var/log/private': Permission denied
4.0K    /var/log/private
44K     /var/log/apt
49M     /var/log/journal/62087aa04ffa494a9a3bba7805e42538
49M     /var/log/journal
4.0K    /var/log/runit/ssh
8.0K    /var/log/runit
50M     /var/log
```
We Note the last entry - `50M     /var/log`

This tells us the total size of the Log is 50MB so we can't fit it in RAM

## Fix the error 1071

We can edit `/etc/log2ram.conf` to fix this.

```sh
sudo nano /etc/log2ram.conf
```

Edit the line:

```sh
SIZE=40M
```

Change it to a slightly higher capacity than we saw in the [`du` command](#use-du-to-look-at-log-directory-size):

```sh
SIZE=60M
```

Save the file.

#### Restart `log2ram` service

```sh
sudo systemctl restart log2ram
```

#### Check status of `log2ram`

This time it should work:

```sh
sudo systemctl status -l log2ram
```

#### Last Step

Finally **Reboot the Raspberry Pi**.

----
<!-- Footer Begins Here -->
## Links

- [Back to Log2Ram Main Article](./log2ram.md)
- [Back to Raspberry Pi Hub](./README.md)
- [Back to Linux Hub](../README.md)
- [Back to Root Document](../../README.md)
