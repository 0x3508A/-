# Silabs Zibee Know-how Document

## Silabs Zigbee Links

- Development Kit errors:

    <https://www.silabs.com/community/wireless/zigbee-and-thread/forum.topic.0.15.html/slwstk6104a_examples-5ybj>

- IoT Boot Camp :

    <https://github.com/SiliconLabs/IoT-Developer-Boot-Camp/wiki/Zigbee-Hands-on-Forming-and-Joining>

- Zigbee3.0 Documentation of API <https://docs.silabs.com/zigbee/latest>
- Zigbee3.0 Boot-camp <https://www.silabs.com/support/training/zigbee-software-bootcamp>
- Zigbee3.0 Intro Video <https://www.youtube.com/watch?v=HJlQI2Z7V-Q>
- Zigbee3.0 Together Better (Advertisement) <https://www.youtube.com/watch?v=WgbeGMcRkzs>
- Zigbee3.0 Technical Presentation:

    <https://zigbeealliance.org/wp-content/uploads/zip/zigbee-technical-presentation.zip>

    - **[Zigbee-Technical-2019 PPT](./silabs-zigbee/Zigbee-Technical-2019.pptx.7z)**

- Zigbee Remote Control 2.0 <https://www.youtube.com/watch?v=QT-jTxvHa50>

    RF Remote Controls = Zigbee RF4CE

- EmberZNet PRO Zigbee Protocol Stack Software <https://www.silabs.com/developers/zigbee-emberznet>
- Technical Resource for Zigbee and Thread <https://www.silabs.com/support/resources.p-wireless_zigbee-and-thread>
- Zigbee & Thread Knowledge Base Articles List <https://www.silabs.com/community/wireless/zigbee-and-thread/knowledge-base.entry.html/2020/04/01/zigbee_thread_knowledgebasearticleslist-ih5r>
- Mesh Networking Training - Silicon Labs <https://www.silabs.com/support/training/mesh>
- Zigbee and Thread Mesh Development Community <https://www.silabs.com/community/wireless/zigbee-and-thread>

## Network Join Steps for End Device and Central

1. Load the `Install Code` in **Switch/End Device**

    Note the `CRC` returned during Programming.

    ```
    program_install_code.sh <Programmer-ID> [<InstallCode>]
    ```

    **[Program for Silabs Install Code](#flashing-script-for-the-silabs-zigbee-install-code)**

    Some steps are also [detailed below](#rough-notes-on-command-sequences-for-silabs-zigbee).

2. Get **Switch/End device** `EUI64 address` using command :

    ```
    info
    ```

3. Derive a Key using `Install Code` and `EUI64` on the **Central**

    ```
    option install-code <link-key-table-index> {<Joining Node's EUI64>} {<16-byte install code + 2-byte CRC>}
    ```

    Example:

    ```
    option install-code 0 {00 0B 57 FF FE 64 8D D8} {83 FE D3 40 7A 93 97 23 A5 C6 39 B2 69 16 D5 05 C3 B5}
    ```

    In this command `"C3 B5"` is the CRC added at the End in LSB order.

    And `"83 FE D3 40 7A 93 97 23 A5 C6 39 B2 69 16 D5 05"` is the `Install Code`.

    ```
    #'MFG_INSTALLATION_CODE (Smart Energy Install Code)' token group
    # Install Code Flags : 0x0006
    Install Code       : 83FED3407A939723A5C639B26916D505
    # CRC                : 0xB5C3
    ```

    !!! note "Link Key"
        `Derived Link Key` from the Output of the command is used again.
        In the command we have used "0" hence the same location key needs to be copied.

4. To Form the Network with **Central** :

   ```
   plugin network-creator start 1
   ```

5. Open the Network using the `Derived Link Key` from step 3 with `"EUI64 address"` on **Central**:

    ```
    plugin network-creator-security open-with-key {eui64} {linkkey}
    ```

6. On the **Switch/End Device**  now start the steering:

    ```
    plugin network-steering start 0
    ```


## Configuration of the Light and Switch Projects

ZCL Clusters selection:

1. Switch = **HA On/Off Switch**
2. Light = **HA On/Off Light**

Zigbee Stack Device type:

1. Switch = **Router** / **End Device**
2. Light = **Coordinator or Router**

Printing & CLI:

1. `Enable Debug Printing` should be checked
2. In Table Functional Area `Application specific debug printing > On Off cluster` check
    - Compiled In
    - Enabled at startup

Plugins:

1. **Coordinator Or Router**
   1. Enable
      - Network Creator
      - Network Creator Security
      - ZigBee PRO Stack Library
      - Security Link Keys library
      - Serial
      - Scan Dispatch - `scan-dispatch`
      - Concentrator Support: Source Route Library
   1. Disable
      - Install Code Library
      - Network Steering
      - Update TC Link Key
1. **Switch or End Device**
   1. Enable
      - ZigBee PRO Stack Library
      - Serial
      - Install Code Library
      - Network Steering
      - Update TC Link Key
   1. Disable
      - Network Creator
      - Network Creator Security
      - Security Link Keys library

## Rough Notes on Command Sequences for Silabs Zigbee

Some relevant Links:

- <https://www.silabs.com/community/wireless/zigbee-and-thread/forum.topic.0.15.html/slwstk6104a_examples-5ybj>

- <https://github.com/SiliconLabs/IoT-Developer-Boot-Camp/wiki/Zigbee-Hands-on-Forming-and-Joining>

Here is the Command sequence:

```
node [(>)84FD27FFFEE659CC] chan [0] pwr [0]
eui64 {84 FD 27 FF FE E6 59 CC}
option install-code 0 {84 FD 27 FF FE E6 59 CC} {83 FE D3 40 7A 93 97 23 A5 C6 39 B2 69 16 D5 05 C3 B5}

linkkey {66 B6 90 09 81 E1 EE 3C A4 20 6B 6B 86 1C 02 BB}

Short ID: 0x0000, EUI64: (>)847127FFFEDF012A, Pan ID: 0xE945

Pan ID: 0xE945

plugin network-creator-security open-with-key {eui64} {linkkey}

plugin network-creator-security open-with-key {84 FD 27 FF FE E6 59 CC} {66 B6 90 09 81 E1 EE 3C A4 20 6B 6B 86 1C 02 BB}

NWK Key: 5A 1B 02 D4 5D B2 8A 1B  04 32 EF E5 54 7D 28 A2

Link E4 9F 1F FF 20 34 5F 82  51 7E C3 50 5B 86 8E 80

--------------------------------------------------------------------------

Install Code       : 83FED3407A939723A5C639B26916D505
# CRC                : 0xB5C3


Full Install Code {83 FE D3 40 7A 93 97 23 A5 C6 39 B2 69 16 D5 05 C3 B5}

MFG_EMBER_EUI_64: 9DEE43FEFF818E58

EUI64: {9D EE 43 FE FF 81 8E 58}

option install-code 0 {9D EE 43 FE FF 81 8E 58} {83 FE D3 40 7A 93 97 23 A5 C6 39 B2 69 16 D5 05 C3 B5}

key print

NWK Key: FF FF FF FF FF FF FF FF  FF FF FF FF FF FF FF FF
Transient Key: 66 B6 90 09 81 E1 EE 3C  A4 20 6B 6B 86 1C 02 BB

plugin network-creator start 1

plugin network-creator-security open-with-key {9D EE 43 FE FF 81 8E 58} {66 B6 90 09 81 E1 EE 3C A4 20 6B 6B 86 1C 02 BB}

Switch:
plugin network-steering start 0

git@github.com:boseji/goRepoTemplate.git
Program Configuration File Storage in JSON

# Stop the Network on Router / Central
plugin network-creator stop


# Light Steps

network leave

plugin network-creator-security open-network

plugin network-creator start 1


```

## Flashing Script for the Silabs Zigbee Install Code

Here is the [source code](./silabs-zigbee/linux-installcode.sh).

## Silabs MGM220P #Zigbee Examples

### Internal Bootloader 512KB

This is needed for all application to work. Since it helps to boot the
application from the #secure side of ARM Cortex-M33.

Creating it directly from the Wizard works.

### Z3LightSoc - Zigbee Light Coordinator

This application works with the `LED1` blinking and `LED0` being used
as Light on the `WSTK Board`.

The `Button 0` Should have been for provisioning but that does not work.

Hence we need to use commands.

On `Z3Light` for Start Network Formation:

```
plugin network-creator-security open-network
```

To Start a New Network:

```
// Form Z3.0 network.
>>plugin network-creator start [useCentralizedSecurity:1]

//Form centralized network.
>> plugin network-creator start 1

//Form distributed network.
>> plugin network-creator start 0
```

From <https://www.silabs.com/community/wireless/zigbee-and-thread/knowledge-base.entry.html/2018/04/24/how_to_form_zigbee3-zrzB>

### Z3Switch - Zigbee On/Off Switch (Zigbee Router / Device)

This application again should use `Button 0` for provisioning.
But that does not work. Hence the commands.

On Z3Switch:

```
 # forget older network
leave network
 # Start joining
plug network-steering start 0
```

Next to Toggle ON-Off things:

```
zcl on-off toggle
send 0 1 1
```

## Zigbee Experiments follow-on to Silab's Meet 18th October 2021

There are were various aspects discussed on optimality of *MGM220P* for our use-case.

It was suggested that *MGM210* is better module for Gateway application.

Since, we currently do not have the *MGM210*, we would use the **Thunder Board Sense 2**.

### Design of Experiments

1. **Gateway** on **MGM220P** (*HA/LO Light*) to **End Device** on **MGM220P** (*HA Switch*) - following the instructions from *IoT Bootcamp*.
2. **Gateway** on **Thunder Board Sense 2** (*HA/LO Light*) to **End Device** on **MGM220P** (*HA Switch*) - following the instructions from *IoT Bootcamp*.

### Hypothesis

1. The success of first experiment determined by a successful network and Light to switch operation. This means that the previous version of **EmberZNet Pro - Zigbee stack** had some issues. Another possibility can be there are setup issues and update for all boards is mandatory along with update of the **Silabs Simplicity Studio V5** IDE.
2. The success of the second experiment determined by a successful network and Light to switch operation. This means that the **MG12** is better suited for Gateway operation. The **EmberZNet Pro stack** does not support **MGM220P** as Gateway.
3. There is some issues with Linux based operation of **Silabs Simplicity Studio V5** IDE. Or it needs different compilers. This is the case when both experiments have failed.

### Experiment Inputs

1. WSTK Kit + MGM220P Carrier Board (2 Sets) would be used for Experiment 1
2. Thunder Board Sense 2 would be used for Experiment 2 along with WSTK Kit + MGM220P Carrier Board.
3. Linux PC with update Silabs Simplicity Studio V5 would be used.
4. All Adapter and Security firmware would be up to date.
5. Wireless range at desk level and close-by.

#### Details

| Entity                | Detail           |
| --------------------- | ---------------- |
| WSTK Kit              | BRD4001A Rev A02 |
| MGM220P Carrier Board | BRD4311B Rev A01 |
| Thunder Board Sense 2 | BRD4166A Rev D03 |
| Gecko SDK Suite       | v3.2.3           |
| Platform              | 3.2.1.0          |
| MCU                   | 6.1.3.0          |
| EmberZNet             | 6.10.2.0         |

#### Examples Used

1. For Gateway - ZigbeeMinimal with IoT Bootcamp Light Configuration
2. For EndDevice - ZigbeeMinimal with IoT Bootcamp Switch Configuration
3. Bootloader - Internal Single Bank 512k (Common for MGM220P)

Configuration for Zigbee Light:

1. ZCL Cluster : 1 - HA On/Off Light
2. Zigbee Stack : Coordinator or Router
3. Printing and CLI: - Application specific Debug Printing
   - Basic Cluster
   - Identify Cluster
   - On-Off Cluster
4. Plugins Enabled
   - I/O - Serial
   - Common Clusters - Identify Cluster
   - Zigbee 3.0
     - Network Creator
     - Network Creator Security
     - Find and Bind Target
   - Stack Libraries
     - Security Link Keys
     - Zigbee PRO Stack Library
     - Install code library
     - Source Route Library
5. Plugin Disabled
   - Zigbee 3.0
     - Network Steering
     - Update TC Link Key
6. Callbacks Enabled
   - General - On/Off Cluster
     - Off
     - On
     - Toggle (Enabled Automatically)

Custom Callback Code added for Light:
```c
// Sending-OnOff-Commands: Step 1
bool emberAfOnOffClusterOnCallback(void){
  emberAfCorePrintln("On command is received");
  halClearLed(1);// Off
}

bool emberAfOnOffClusterOffCallback(void){
  emberAfCorePrintln("Off command is received");
  halSetLed(1);//Inverted
}

bool emberAfOnOffClusterToggleCallback(void){
  emberAfCorePrintln("Toggle command is received");
  halToggleLed(1);
}
```

Configuration for Zigbee Switch:

1. ZCL Cluster : 1 - HA On/Off Switch
2. Zigbee Stack : Router
3. Printing and CLI: - Application specific Debug Printing
   - Basic Cluster
   - Identify Cluster
   - On-Off Cluster
4. Plugins Enabled
   - Zigbee 3.0
     - Network Steering
     - Find and Bind Initiator
   - Stack Libraries
     - Install Code
     - Zigbee PRO Stack Library
   - Zigbee 3.0
     - Update TC Link Key
5. Plugins Disabled
   - Zigbee 3.0
     - Network Creator
     - Network Creator Security
6. Callbacks Enabled
   - Plugin Specific Callbacks
     - Button 0 Pressed Short
     - Button 1 Pressed Short

Custom Callback Code added for Zigbee Switch:

```c
// Sending-OnOff-Commands: Step 2
void emberAfPluginButtonInterfaceButton0PressedShortCallback(uint16_t timePressedMs)
{
  emberAfCorePrintln("Button0 is pressed for %d milliseconds",timePressedMs);

  EmberStatus status;

  emberAfFillCommandOnOffClusterOn()
  emberAfCorePrintln("Command is zcl on-off ON");

  emberAfSetCommandEndpoints(emberAfPrimaryEndpoint(),1);
  status=emberAfSendCommandUnicast(EMBER_OUTGOING_DIRECT, 0x0000);

  if(status == EMBER_SUCCESS){
    emberAfCorePrintln("Command is successfully sent");
  }else{
    emberAfCorePrintln("Failed to send");
    emberAfCorePrintln("Status code: 0x%x",status);
  }
}

void emberAfPluginButtonInterfaceButton1PressedShortCallback(uint16_t timePressedMs)
{
  emberAfCorePrintln("Button1 is pressed for %d milliseconds",timePressedMs);

  EmberStatus status;

  emberAfFillCommandOnOffClusterOff()
  emberAfCorePrintln("Command is zcl on-off OFF");

  emberAfSetCommandEndpoints(emberAfPrimaryEndpoint(),1);
  status=emberAfSendCommandUnicast(EMBER_OUTGOING_DIRECT, 0x0000);

  if(status == EMBER_SUCCESS){
    emberAfCorePrintln("Command is successfully sent");
  }else{
    emberAfCorePrintln("Failed to send");
    emberAfCorePrintln("Status code: 0x%x",status);
  }
}
```


#### Install Code

 Note : Make sure that the Zigbee Switch board is disconnected from IDE before Loading.
```sh
export DEFAULT_INSTALL_CODE="83FED3407A939723A5C639B26916D505"
export SWITCH_BOARD="440223097"
# Get the IDs of Boards
commander adapter probe
# Erase the Previous Code
commander flash -s $SWITCH_BOARD --tokengroup znet --token "Install Code: !ERASE!"
# Add the Default Install Code
commander flash -s $SWITCH_BOARD --tokengroup znet --token "Install Code:$DEFAULT_INSTALL_CODE"
# Check the Token dump
commander tokendump -s $SWITCH_BOARD --tokengroup znet --token TOKEN_MFG_INSTALLATION_CODE
```

Info Obtained:
```
Install Code       : 83FED3407A939723A5C639B26916D505
# CRC                : 0xB5C3
```

#### Get EUI from Zigbee Switch

- Connect the Zigbee Switch to Simplicity Studio and launch the console
- Enter the Command: `info`

Info Obtained:
```
...
node [(>)588E81FFFE43E631] chan [0] pwr [0]
...
```

**`EUI : 58 8E 81 FF FE 43 E6 31`**

#### Add the Zigbee Switch info to Zigbee Light

- Connect the Zigbee Light to Simplicity Studio and launch the console
- Enter the Command:
  ```
  option install-code <link key table index> {<Joining Node's EUI64>} {<16-byte install code + 2-byte CRC>}
  ```
  In Our case this would become:
  ```
  option install-code 0 {58 8E 81 FF FE 43 E6 31} {83 FE D3 40 7A 93 97 23 A5 C6 39 B2 69 16 D5 05 C3 B5}
  ```
  Note: The CRC was actually `0xB5C3` however it needs to placed in *Little Endian* format as `C3 B5` at the end of the *16-byte Install Code*.
- On the Light it should show `Success: Set joining link key`
- Next we need the Derived Keys. Give command `keys print`
- Note the *Link Key*:
  ```
  ...
  Index IEEE Address         NWKIndex  In FC     TTL(s) Flag    Key
  0     (>)588E81FFFE43E631  0         00000000  0x00A4 0x0000  66 B6 90 09 81 E1 EE 3C  A4 20 6B 6B 86 1C 02 BB
  ...
  ```
  Here *Link Key = `66 B6 90 09 81 E1 EE 3C  A4 20 6B 6B 86 1C 02 BB`*.
- We need to add this *Link Key* later for Opening the Network.

#### Initiate the Network

- Keeping both Zigbee Light and Switch Connected
- Enter Commands in Zigbee Light Console:
  ```
  plugin network-creator start 1
  ```
  This would begin the Join Process.
- Next give the query Command to Zigbee Light Console:
  `network id` This would show the Network ID
  ```
  ... Pan ID: 0x5661..
  ```
  Note the **Pan ID** here this needs to be checked back in the Switch
- Next give command to open the network for the device.
  Format:
  ```
  plugin network-creator-security open-with-key {eui64} {link-key}
  ```
  In our case it would be :
  ```
  plugin network-creator-security open-with-key {58 8E 81 FF FE 43 E6 31} {66 B6 90 09 81 E1 EE 3C  A4 20 6B 6B 86 1C 02 BB}
  ```
- Now on the Zigbee Switch Console give command to start the Network discovery:
  ```
  plugin network-steering start 0
  ```
  For a Successful Join The device would show: `Join network complete: 0x00`
- Now to check the Network ID on Zigbee Switch Console:
  `network id`
  ```
  ... Pan ID: 0x5661
  ```
  This is same as the Zigbee Light. Hence the Zigbee Switch has joined the network Hosted by Zigbee Light.

#### Network Monitoring

- Keeping the Zigbee Light connected on Console
- Give the Command to get the keys: `keys print`
  ```
  ...
  NWK Key: A3 F3 E7 47 22 2F EA 0B  3E 5A E5 00 40 09 6A 98
  ...
  -     (>)588E81FFFE43F036  00000000  L     y     A4 48 C7 E9 40 1E B1 CD  0B 31 F6 B2 A1 CC EE 45
  ```
  Here are 2 keys `Network Key` and `Derived Link Key`
- Open the `Preferences` for the Simplicity Studio. Navigate to `Network Analyzer -> Decoding -> Security Keys`
  Add the `Network Key` and Network Key`
- Open the `Start Capture` to start with Network Analyzer on Zigbee Light Switch.
- In the Zigbee Light Console enter the command to begin find-and-bind:
  ```
  plugin find-and-bind target 1
  ```
  You should get `Find and Bind Target: Start target: 0x00`
- On the Zigbee Switch Console give the command:
  ```
  plugin find-and-bind initiator 1
  ```
  You should get `Find and Bind Initiator: Complete: 0x00`

#### Find and Bind for Light and Switch

- Connect both the Zigbee Light and Zigbee Switch
- Ensure that the PAN ID is the same on both from the respective consoles.
- Send Command on Zigbee Light console:
  ```
  plugin find-and-bind target 1
  ```
  it should show `Find and Bind Target: Start target: 0x00`
- Send Command on the Zigbee Switch console:
  ```
  plugin find-and-bind initiator 1
  ```
  it should show `Find and Bind Initiator: Complete: 0x00`
- This means that the Button to Light has been connected
- Next Check the routing on Zigbee Light console give this command:
  ```
  option binding-table print
  ```
- Now we can send Toggle from the Zigbee Light Switch:
  ```
  zcl on-off toggle
  bsend 1
  ```

##### Manual ON Off

- On the Zigbee Switch Console
  ```
  zcl on-off off
  send 0 1 1
  ```
  This would send the Off Command to the Light
- Now for the ON command:
  ```
  zcl on-off on
  send 0 1 1
  ```

### Experiment 1 Results

- Network Join was successful.
- Button Based commands are not working.
- Find and Bind also failed.

#### Conclusions

- Though the Network Joining does work the Find-and-bind does not work correctly.
- The success of the Gateway operation can be attribute to the updated stack and updated Simplicity studio v5.
- Manual ON/OFF commands from Switch work.
- Manual Toggle command does not work.
- Button Press does not work even though they have been configured.
- This disproves the Hypothesis 3.
- This partiality supports the Hypothesis 1.


### Experiment 2 Results

- There was an update to the Simplicity studio again.
- Network Info
  ```
  Pan ID: 0xF832
  Pan ID: 0xF832
  NWK Key: 70 D5 3C 62 2E 8B 1E F9  A4 55 BB D5 59 86 C1 07
  Link Key: ED C9 3A 40 0D 2D 08 4C  3B AA 16 66 25 57 28 49
  ```
- Find and Bind also failed.

#### Conclusions

- Though the Network Joining does work the Find-and-bind does not work correctly.
- Gateway operation works better when both end are MG12 or TB2 instead of MGM220P.
- For Manual Commands sent from Switch, Light shows that the ON-OFF in Serial print. However the actual hardware does not show any change.
- Manual Toggle command does not work.
- Button Press does not work even though they have been configured.
- This disproves the Hypothesis 3.
- This partially supports the Hypothesis 2.
- MGM220P is not fully supported by EmberZNet Stack v6.10.2.0
- MG12 works only with other MG12 devices like TB2 and not MGM220P.

### Experiment Results

- MGM220P is incompatible to other family of EFR32 devices.
- MG12 works with only MG12 devices (prior experiments with TB2 were successful.).
- MGM220P is not supported fully in EmberZNet Stack v6.10.2.0

#### Verdict

- MGM220P needs to be evaluated in further detail.
- Compatibility needs to be checked with suggested MGM210 boards.
- MG12 is not suited for Gateway operation due to incompatibility with MGM220P.

----
<!-- Footer Begins Here -->
## Links

- [Back to IDEs, PCB, ECAD and Programming Tools Hub](./README.md)
- [Back to Hardware Hub](../README.md)
- [Back to Root Document](../../README.md)
