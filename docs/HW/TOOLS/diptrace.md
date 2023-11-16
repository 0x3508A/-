# DipTrace - Schematic and PCB Design Software

## Fiducial and Panelizing

Good guide on How to use Fiducial in Diptrace and create PCB panels for mass manufacturing.

<http://www.diptrace.com/forum/viewtopic.php?f=16&t=9761>

[PDF File of the Article](./diptrace/Panelizing_How_to_specify_V-score_or_tab-route_-_DipTrace-Forum.pdf)

## Usage Logic of Parts in Schematics

- Any normal PCB component
    - `Purchase = True` , `Populate = True`
- DNP Item or Fiducial or PCB Antenna .etc.
    - `Purchase = False` , `Populate = False`
- Only BOM part but not to be Populated - like Box, Bare PCB, clips, screws .etc.
    - `Purchase = True` , `Populate = False`
- For Parts that come as a Kit with other BOM parts and need not be procured separately
    - `Purchase = False` , `Populate = True`

## Component Fields

- Field: `Purchase`
    - `True` = Most Parts
    - `False` = Layout or PCB features or DNP(Do not Populate)
-   Field: `Populate`
    -   `True` = Need to Pick-n-place
    -   `False` = No need to populate or DNP(Do not Populate)
- Field: `DPNo` or `Digikey` or `DistiPartNo`
    - **Digikey** or **Mouser** or any distributor Part Number
- Field: `Description`
    - Description of the Component from the Distributor site like **Digikey**
- Field: `Distributor`
    - Name of the Distributor from whom this part needs to be bought

## Diptrace Library Revision 2021

Diptrace 2.4 Library Update activities for 2021

- [ ] Symbol = Passives
- [ ] Symbol = Diodes
- [ ] Symbol = Diodes-TVS-ESD
- [ ] Symbol = IC-MCU
- [ ] Symbol = IC-Sensor
- [ ] Symbol = Transistors
- [ ] Symbol = Special
- [ ] Footprint = IC
- [X] Footprint = General
- [X] Footprint = Connectors
- [ ] Footprint = Special

## Correcting the PCB Drill table Generation - *For DipTrace V2.4 and below*

1. Open PCB file fresh new window
2. Set the Units as `mm`
3. `File > Export > Gerber` to open Settings
4. *Un-Check* and then Check the **"Use Design Origin"** Check box
5. Now check the **"Drill Symbols"**
6. *Press* the **"Set Symbols"**
7. *Click* on **"Auto"** in the new Window and then **"Close"**
8. Finally click on **"Export"** to generate the new "`Through.gbr`"


----
<!-- Footer Begins Here -->
## Links

- [Back to IDEs, PCB, ECAD and Programming Tools Hub](./README.md)
- [Back to Hardware Hub](../README.md)
- [Back to Root Document](../../README.md)
