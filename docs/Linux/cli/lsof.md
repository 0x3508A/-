# `lsof` - List open file command

#### Why ?

- Error File is in use
- Security unknown process running

## Examples

```sh
lsof
# All the files open

lsof | head
# First 10 lines
# COMMAND      PID  TID  TASKCMD       USER FD     TYPE

lsof | wc -l
# Count the Files open from current user

sudo lsof | wc -c
# Whole system level.

lsof -u <username>
# All from particular user

sudo lsof /mnt/wp-content
# Check who is reading this directory

sudo lsof -u ^root
# Find all not by root user

sudo lsof -i 4 /mnt/wp-content
# Find the IP address(ipV4) of user opening the file
```

----
<!-- Footer Begins Here -->
## Links

- [Back to Find network ports open article](../find-network-port-open.md)
- [Back to Command Line Interface Hub](./README.md)
- [Back to Linux Hub](../README.md)
- [Back to Root Document](../../README.md)
