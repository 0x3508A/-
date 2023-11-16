# IP-Messenger = LAN p2p Communicator

Best PC to PC local network Sharing tool.

Works for Windows and Linux (under WINE).

Home Page
<https://ipmsg.org/>

Help for IP Messenger v5.6.1
<https://ipmsg.org/help/ipmsghlp_eng.htm>

## Important Firewall Setting for IP Messenger to work

Normally use **2425 port for TCP/UDP**. (See 8. Appendices)
Use **2425 port only for UDP** with *no File(Folder) Transfer*.
(These ports should be activated when using firewall software.)

Setting is saved in the following registry key.
`\\HKEY_CURRENT_USER\Software\HSTools\IPMsgEng`
(If port number has been set, *IPMsg + port number*)
When changing your registry number, please re-start **ipmsg**.

Protocol specification comes with source.(Japanese)

These need to manually add in **Linux** for **`ufw`**

```shell
sudo ufw allow 2425
```

Then it works properly even on **Linux**.

IP Messenger use **2425/UDP** port for member detection and message communication, and use **2425/TCP** port for file and image transfer.

If those port are blocked by OS or Antivirus software, IP Messenger can't detect other member or can't send/receive file or images.
Please open these port.

----
<!-- Footer Begins Here -->
## Links

- [Back to Windows Hub](./README.md)
- [Back to Root Document](../README.md)
