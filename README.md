# Advanced Physical Design using OpenLANE/Sky130

This repository include briefings about the Advanced Physical Design workshop using OpenLANE/Sky 130 organised by VLSI System Design(VSD) team, held between 22 Jan 2021 – 26 Jan 2021.

<p>&nbsp;</p>

# Table of Contents
## Day 1
* ASIC Design Flow
* Sky 130 PDK
* OpenLANE flow
* Synthesis using OpenLANE/Sky130  
## Day 2
* Physical Design Flow
* Floorplan using OpenLANE/Sky130
* Placement using OpenLANE/Sky130
## Day 3
* Standard Cells
* Simulation of Standard cell using Ngspice
## Day 4
* LEF file generation
* Synthesis of Standard cell
* Timing analysis using openSTA
* Clock tree synthesis using TritonCTS
## Day 5
* Power Distribution Network 
* Routing using TritonRoute
* SPEF generation

<p>&nbsp;</p>

# Day 1 

<p>&emsp;&emsp;
Application Specific Integrated Circuit popularly known as ASIC have become a part of our day to day lives. This has led to the ever increasing demand for faster, power efficient ASICs. 

## ASIC Design Flow
<p>&emsp;&emsp;
The ASIC Design Flow can be broadly divided into these major steps :-

* RTL Design - Register Transfer Level(RTL) Design step involves Hardware description languages such as Verilog, VHDL to write a code which corresponds to the required System Hardware.
* Logic Synthesis - The  RTL code is converted to a netlist which contains components such as gates and their connections.
* Functional Verification  - The netlist generated in the previous stage is checked in this stage. It is made sure that the netlist performs all intended functionality.
* Physical Design - The netlist is laid out on the chip as physical components with different layers. It also includes timing analysis and routing.
* Physical Verification and Sign Off - The design is checked for DRC, LVS errors. The Power, Timing and Delay characterization are done.
* Tape Out - The GDSII file is generated and sent to the foundry for fabrication.
<p>&nbsp;</p>
<p>&emsp;&emsp;
It was always a dream to have open source ASIC because for development of ASIC basically three things are required RTL Design, EDA tools and PDK data. RTL Design and EDA tools have been open-source for quite a while, but open source pdk was not available. Google and Skywater released Sky130 as the first open-source PDK. So, the dream of having open-source ASIC can finally become true.

## Sky130 PDK
<p>&emsp;&emsp;
Process Design Kit(PDK) is a set of files which contains Standard cell library, Design rules, Technology files, etc. It is usually distributed by a foundry so that VLSI Engineers can design according to these rules. 
<p>&emsp;&emsp;
Sky130 PDK is released by a joint venture between Google and Skywater as the first open source PDK. It uses 130nm process for chip fabrication. It has made way for a fully open source ASIC design.
<p>&nbsp;</p>

 ## OpenLANE flow
Openlane is a opensouce automated flow which is used to generate RTL to GDSII flow with no human in the loop principle. It means that it takes Verilog HDL files and SKY130 pdk as input and gives the output as GDSII file which can be directly sent to fabrication.
Openlane uses tools such as Openroad, Yosys, abc, magic, netgen, fault, opensta, tritonroute,etc at different stages to achieve its final goal.
<p>&nbsp;</p>

![OpenLANE flow](https://github.com/efabless/openlane/raw/master/doc/openlane.flow.1.png)
   
<p>&nbsp;</p>
The tools used by OpenLane flow are :-

* Yosys - RTL Synthesis
* abc - Technology mapping
* Fault - DFT insertion
* OpenROAD - Physical Design
* TritonRoute - Detailed Routing
* OpenSTA - Static Timing Analysis
* Magic - Layout and DRC check 
<p>&nbsp;</p>


## Synthesis using OpenLANE/Sky130
<p>&nbsp;</p>
To understand the OpenLANE flow, a design "picorv32a" is used in this workshop. Synthesis of "picorv32a" is performed using OpenLANE/Sky130 in this section.
<p>&nbsp;</p>
On your computer, open terminal on the folder containing Openlane and type the command
<p>&nbsp;</p>

>`./flow.tcl –interactive `
 <p>&nbsp;</p>

 ![openlane](https://github.com/an3ol/Advanced-Physical-Design-using-OpenLANE-Sky130/blob/main/photo/openlane.png?raw=true) 

In order to load the packages, type the following command
<p>&nbsp;</p>

>`package require openlane 0.9`

<p>&nbsp;</p>

To prepare the design for synthesis, type

<p>&nbsp;</p>

>`prep –design  <design_name> `

<p>&nbsp;</p>
In our case the design name is picorv32a, so we type

<p>&nbsp;</p>

>`prep –design picorv32a`
 
<p>&nbsp;</p>

 ![openlane](https://github.com/an3ol/Advanced-Physical-Design-using-OpenLANE-Sky130/blob/main/photo/prep.png?raw=true)

After few moments, ‘Preparation complete’ message will be seen.
 Now the design is ready for synthesis. Type the command

<p>&nbsp;</p>

>`run_synthesis`

 <p>&nbsp;</p>

It may take sometime depending upon the size of your design. After synthesis it will show the message as ‘Synthesis complete’.
 
<p>&nbsp;</p>

 ![openlane](https://github.com/an3ol/Advanced-Physical-Design-using-OpenLANE-Sky130/blob/main/photo/synthesiscomplete.png?raw=true)


You can find information such as no. of cells in your design and other components such as no. of gates, etc.
<p>&nbsp;</p>

 ![openlane](https://github.com/an3ol/Advanced-Physical-Design-using-OpenLANE-Sky130/blob/main/photo/flop.png?raw=true)
 
The information regarding the synthesis can be found on the runs folder located inside the folder where design was saved.
 
<p>&nbsp;</p>

# Day 2







<p>&nbsp;</p>

## Physical Design Flow
<p>&nbsp;<p>
The Physical Design Flow can be broadly divided into the following steps :-

* Floorplan - The chip is partitioned and the standard cells are placed randomly for the time being.
* Placement - The standard cells are efficiently placed keeping in mind the area congestion and delay.
* Clock tree synthesis - The clock tree is built with the buffers included in the design which are required for propagation of clock signals with minimum delay.
* Routing - The connections are made between the Standard cells using metal wires at different layers.

<p>&nbsp;</p>

## Floorplan using OpenLANE/Sky130

<p>&nbsp;

The design "picorv32a" for which the netlist was generated in the previous section, will be used in this section to understand Floorplan.
<p>&nbsp;</p>

To perform Floorplanning, type the command

>`run_floorplan`
 

After a while, we get the message ‘Done!’


To see the floorplanning,  we have to use the tool Magic. The command to open the tool is
>`magic –T <address to the sky130A.tech file> lef read < address to the merged.lef file > def read < address to the design.floorplan.def>`
<p>&nbsp;</p>

In the Magic window, Press S and then V. The floorplan is shown below:-
<p>&nbsp;</p>

 ![openlane](https://github.com/an3ol/Advanced-Physical-Design-using-OpenLANE-Sky130/blob/main/photo/floorplan_magic.png?raw=true)

 <p>&nbsp;</p>

## Placement using OpenLANE/Sky130
<p>&nbsp;</p>

The next step is placement of the standard cells. Type the command

>`run_placement`
 
A message Program end will appear
 
To see the placement,  we have to use the tool Magic. The command to open the tool is
>`magic –T <address to the sky130A.tech file> lef read < address to the merged.lef file > def read < address to the design.placement.def>`
 <p>&nbsp;</p>
In the magic window, we can see placement of the standard cells
 <p>&nbsp;</p>
 
 ![openlane](https://github.com/an3ol/Advanced-Physical-Design-using-OpenLANE-Sky130/blob/main/photo/magic%20placement.png?raw=true)
 <p>&nbsp;</p>

# Day 3
<p>&nbsp;</p>

## Standard Cell
<p>&nbsp;
A Standard cell is a basic unit which is used to build any design. It can be a simple gate such as NOT, AND, OR, etc.  or it can be a combinational circuit such as adder. A D-Flip Flop is also a standard cell. A library of Standard cells are usually provided by the foundry in its PDK. We can also build our own standard cells to be used in our design, tailored as per our needs.

For the rest part of the workshop, we will use an Inverter Standard cell made by Nickson Jose, our instructor at the workshop.
<p>&nbsp;</p>

## Simulation of Standard cell using Ngspice
<p>&nbsp;</p>

&nbsp; The standard cell 'vsd_inv' is simulated using Ngspice simulator to verify its properties. In order to simulate 'vsd_inv', spice file needs to be generated. To generate the spice file, magic tool is used.
<p>&nbsp;</p>

On the terminal window, type the following command
>`magic -T sky130A.tech sky130_inv.mag`

The Magic window will appear with the layout showing different layers of the inverter.
<p>&nbsp;</p>

![openlane](https://github.com/an3ol/Advanced-Physical-Design-using-OpenLANE-Sky130/blob/main/photo/inverter_layers.png?raw=true)
<p>&nbsp;</p>

On the magic command window, type 


> `extract all`
>
> `ext2spice cthresh 0 rthresh 0`
>
>`ext2spice`<p>

<p>&nbsp;</p>

![openlane](https://github.com/an3ol/Advanced-Physical-Design-using-OpenLANE-Sky130/blob/main/photo/spice%20file.png?raw=true)
<p>&nbsp;</p>

The 'sky130_inv.spice' file will be generated.
This file will be used for Ngspice simulation of the standard cell. To invoke Ngspice type the command

>`ngspice sky130_inv.spice`

After the simulation is complete, type
>`plot y vs time a` 

A waveform will appear which shows the input and output signal wrt time.
<p>&nbsp;</p>

![openlane](https://github.com/an3ol/Advanced-Physical-Design-using-OpenLANE-Sky130/blob/main/photo/inv_output.png?raw=true)
<p>&nbsp;</p>

# Day4
<p>&nbsp;</p>

## LEF file generation
<p>&nbsp;
Now that we have verified the inverter using Spice simulations , we will try to use our standard cell in the "picorv32a" design. In order to do so, we have to generate a LEF file for our Standard cell Inverter and run Synthesis again.
<p>&nbsp;</p>
Magic tool is used generate the LEF file. On the command window of Magic type

> `lef write`
<p>&nbsp;</p>

![openlane](https://github.com/an3ol/Advanced-Physical-Design-using-OpenLANE-Sky130/blob/main/photo/LEF.png?raw=true)
It will generate the LEF file which can now be used in the OpenLANE flow for synthesis.
<p>&nbsp;</p>

## Synthesis of Standard cell
Before Synthesis, we need to make changes in the 'config.tcl' file so that OpenLANE recognise our cell. The modified 'config.tcl' file is shown below
<p>&nbsp;</p>

![openlane](https://github.com/an3ol/Advanced-Physical-Design-using-OpenLANE-Sky130/blob/main/photo/config.png?raw=true)
<p>&nbsp;</p>

We can now proceed with the synthesis, type

>`./flow.tcl -interactive`
>
>`package require 0.9`
>
>`prep -design picorv32a -tag trial1 -overwrite`
>
>`set lefs [glob $::env(OPEN_LANE_ROOT)/designs/$::env(DESIGN_NAME)/src/*.lef]`
>
>`add_lefs -src $lefs`
>
>`run_synthesis`

<p>&nbsp;</p>

![openlane](https://github.com/an3ol/Advanced-Physical-Design-using-OpenLANE-Sky130/blob/main/photo/synth_slack.png?raw=true)
<p>&nbsp;</p>

After successful Synthesis, we can check if our Standard cell is present in the design.
<p>&nbsp;</p>

![openlane](https://github.com/an3ol/Advanced-Physical-Design-using-OpenLANE-Sky130/blob/main/photo/synth_vsd_inv.png?raw=true)

## Timing Analysis using OpenSTA

The timing analysis is is done using OpenSTA. type command
>`sta preSTA.conf`

It will generate the timing information regarding the design such as hold slack and set up slack. We have to make sure that the slacks are positive. If they are not positive, we have to do modifications on the design.

The most common method of doing it is identifying the cells with higher delay and replacing them with bigger cells, it is called Upsizing. We can do this by the command

>`replace_cell <instance> <lib_cell>`

we can also restrict the number of fanouts from a device to reduce the negative slack. type

> `set ::env(SYNTH_MAX_FANOUT) 4`

After we come across a satisfactory value of slack, we replace this new netlist with modifications to the location of synthesis file by typing

>`write verilog <address>`

Now, we have to run the Floorplan and Placement again before we proceed to the next step in Openlane flow.

## Clock tree synthesis using TritonCTS
Clock tree synthesis generates the clock tree for the given design. OpenLANE uses TritonCTS tool for Clock Tree generation.
Type

>`run_cts`

After the clock tree gets generated the netlist gets updated and we can find a new file under Synthesis folder 'picorv32a.ctc.v'.

We can check this netlist for any timing violations using OpenSTA.
# Day 5

## Power Distribution Network 
Usually the Power Distribution Network is done in the Floorplanning stage of Design but in OpenLANE flow, it is done after the Clock tree synthesis. Type

>`gen_pdn `

It will generate the Power Distribution Network for the design.

## Routing using TritonRoute
Routing refers to the laying of metal layers for connection between the standard cells. The routing is done in OpenLANE using TritonRoute.
Type
>`run_routing`

After completion of this step, a file will be created in routing folder with name 'picorv32a.def'.

## SPEF generation
To generate the SPEF file, type
> `python3 main.db <address of .lef file> <address of .def file>`

It will merge the .lef and .def file to generate a .spef file. It can be found in the routing folder  as 'picorv32a.spef'.
<p>&nbsp;</p>



# References

* [OpenLANE](https://github.com/efabless/openlane)
* [Sky130 PDK](https://skywater-pdk.rtfd.io/)
* [VSD Std Cell Design](https://github.com/nickson-jose/vsdstdcelldesign)

# Acknowledgement

* [Kunal Ghosh](https://github.com/kunalg123)
* [Nickson Jose](https://github.com/nickson-jose)
