# VSD-Openlane_Workshop-April2021
Capture lab steps during VSD 5 day VLSI open source/hardware EDA course from RTL to Tap Out

# Summary
I participated in the OpenLANE and Sky130PDK course run by VSD company. This was a five day course with the aim of guiding students along the path of going from an RTL (typically produced from an HDL compilation) all the way to tap out ready for a foundry shuttle run. All the necessary steps were covered using Open Source EDA tools called OpenLane. I couldn’t complete day5 (You really need to complete days 1 to 3 by Friday and dedicate whole of Saturday/Sunday to Day4/5 especially if you are new/rusty in this area); the course was really insightful with great tutors. 
This type of RTL to tap out EDA process is normally done through commercial tools (from the likes of Cadence, Siemens, Synopsis) so the open source approach will revolutionise the HW design industry with the potential to grow an Open HW community using the same model as the Open Source SW community. This is happening at the perfect time as 5G is being commercialised with AI, Edge, IoT and advanced radio beamforming technologies all being exploited to deliver high speed/bandwidth low latency services to all kind of electronic devices with different form factors.
I have included the structure of the course below with my labs screen shots; I have listed the theory to show the structure of the course and how the labs sessions/sections align but will not go into the detail (please enrol on the associated Udemy courses offered by VSD/Kunal or/and take the 5 day course to learn more about in the VLSI area).
Currently I'm a product manager for a telecomms software company developing BSS/charging software (considered more as enterprise side); I used to work in embedded sw (2G/2.5G Handset development and PBX) many years ago...

linkedin.com/in/robertedwardstelecommssw

# Day 1 – OpenLANE and Sky130PDK
## Theory 
### How to talk to Computers
#### Introduction to QFN-48 Package, chips, pads, core, die and Ips (Level 4)
#### Introduction to RISC-V
#### From Software Applications to Hardware
### SoC Design and OpenLANE
#### Introduction to all components of Open Source Digital ASIC Design
#### Simplified RTL2GDS flow
#### Introduction to OpenLANE and Strive chipsets
#### Introduction to OpenLANE detailed ASIC design flow

## Labs
### Get Familiar with Open Source EDA Tools
#### OpenLane Directory structure in detail

Explore various directories to get a feeling for what files contribute to the various stages of the RTL to GDS2 workflow:

![image](https://user-images.githubusercontent.com/75379398/114645592-abd6a180-9cd1-11eb-98a6-88ac6524a364.png)

Run openLANE from following directory:

![image](https://user-images.githubusercontent.com/75379398/114646451-2eac2c00-9cd3-11eb-935d-1a2347ce6acd.png)

Run openLANE in interactive mode since we need to tell it which design to work with and want to execute the synthesis, floor planning and placement steps manually to check the progress of the various stages and iterate back where necessary to optimise:

![image](https://user-images.githubusercontent.com/75379398/114646548-5c917080-9cd3-11eb-9d7b-2c56f31946c8.png)

Issued “package require” command in the openLANE console to ensure correct package used.
View ~/Desktop/work/tools/openlane_working_dir/openLANE_flows/designs/picorv32a/config.tcl file to determine settings (I was supposed to set the appropriate synthesis parameters here). This file settings has the highest priority for synthesis:

![image](https://user-images.githubusercontent.com/75379398/114646651-8fd3ff80-9cd3-11eb-8457-099d78fd9819.png)

View:
~/Desktop/work/tools/openlane_working_dir/openLANE_flows/designs/picorv32a/sky130A_sky130_fd_sc_hd_config.tcl
This file has lower priority than config.tcl

![image](https://user-images.githubusercontent.com/75379398/114646687-a0847580-9cd3-11eb-8d20-157989c0eb91.png)

#### Design Preparation Stage
#### Review files after Preparation and Run Synthesis

Issue prep -design picrorv32 to prepare openLANE to use this design for subsequent EDA process steps.
Before issuing prep command the 
“~/Desktop/work/tools/openlane_working_dir/openLANE_flows/designs/picorv32a” directory only contained “src” subdirectory, after issuing the command it also contains a “runs” directory:

![image](https://user-images.githubusercontent.com/75379398/114646794-d4f83180-9cd3-11eb-9b5e-5f9f39068739.png)

After preparation examine generate merged.lef (Library Exchange Format) is a specification for representing the physical layout design rules for an ASIC.
~/Desktop/work/tools/openlane_working_dir/openLANE_flows/designs/picorv32a/runs/07-04_19-04/tmp/merged.lef:

![image](https://user-images.githubusercontent.com/75379398/114646821-e6413e00-9cd3-11eb-9a69-676814cea771.png)

Examine generated config.tcl from the preparation stage in:-
~/Desktop/work/tools/openlane_working_dir/openLANE_flows/designs/picorv32a/runs/07-04_19-04

![image](https://user-images.githubusercontent.com/75379398/114646842-f527f080-9cd3-11eb-81e2-db552ae3fffc.png)

View steps taken in cmds.log file:

![image](https://user-images.githubusercontent.com/75379398/114646899-0cff7480-9cd4-11eb-9309-524ad3a8abc9.png)

Now move onto synthesis stage and execute by issuing run_synthesis command in openLANE, screen below shows the output after this step was executed:

![image](https://user-images.githubusercontent.com/75379398/114646943-1dafea80-9cd4-11eb-8c88-0ca6295ae150.png)

Determine flop ratio (number of D flip flops/total number of cells), examine following file to calculate:-
(dfxtp_4 =1634/Number of cells =17323) -> 0.0943
~/Desktop/work/tools/openlane_working_dir/openLANE_flows/designs/picorv32a/runs/07-04_19-04/results/synthesis/picorv32a.synthesis.v

![image](https://user-images.githubusercontent.com/75379398/114646992-33251480-9cd4-11eb-9f25-03eb263f55cd.png)

Following report files examined in this directory:
~/Desktop/work/tools/openlane_working_dir/openLANE_flows/designs/picorv32a/runs/07-04_19-04/reports/synthesis/

![image](https://user-images.githubusercontent.com/75379398/114647037-489a3e80-9cd4-11eb-85f7-10c69e98e97f.png)

![image](https://user-images.githubusercontent.com/75379398/114647054-50f27980-9cd4-11eb-94fb-0c82219418cf.png)

#### OpenLANE Project Git Link Description
#### Steps to characterize Synthesis Results

# Day 2 – Good Floorplan vs bad floorplan and introduction to library cells

## Theory
### Chip Floor Planning considerations
#### Utilization factor and aspect ratio
#### Concept of pre-placed cells
#### De-coupling capacitors
#### Power planning
#### Pin placement and logical cell placement blockage
#### Additional tips for data setup
### Library Binding and Placement
#### Netlist binding and initial place design
#### Optimize placement using estimated wire-length and capacitance
#### Final placement optimization
#### Need for libraries and characterization
#### Congestion aware placement using RePlace
### Cell design and characterization flows
#### Input for cell design flow
#### Circuit design step
#### Layout design step
#### Typical characterization flow
### General Timing Charaterization Parameters
#### Timing threshold definitions
#### Propagation delay and transition time
## Labs
### Chip Floor Planning considerations
#### Steps to run floorplan using OpenLANE

For day2 I prepared another run project called “rob_trial_run1”
Experimented with viewing and setting environment variables within openLANE on the fly – potential issue since my setting did not match tutor’s….

![image](https://user-images.githubusercontent.com/75379398/114647355-e1c95500-9cd4-11eb-82a1-e74c8ecf936b.png)

Experiment with changing clock period within config.tcl and changing on the fly and seeing what takes effect after synthesis:

![image](https://user-images.githubusercontent.com/75379398/114647391-eee64400-9cd4-11eb-8e22-4a087e584f83.png)

![image](https://user-images.githubusercontent.com/75379398/114647417-f6a5e880-9cd4-11eb-97ee-0ce823630230.png)

![image](https://user-images.githubusercontent.com/75379398/114647438-fe658d00-9cd4-11eb-840b-e518d1475e2d.png)

#### Review floorplan and steps to view floorplan

Examine following file for floorplan options
~/Desktop/work/tools/openlane_working_dir/openlane/configuration/README.md

![image](https://user-images.githubusercontent.com/75379398/114647489-1806d480-9cd5-11eb-815e-0b1c9f3f8abf.png)

Floorplan Parameters are set in:
 ~/Desktop/work/tools/openlane_working_dir/openLANE_flows/configuration/floorplan.tcl

![image](https://user-images.githubusercontent.com/75379398/114647518-2523c380-9cd5-11eb-9be4-3d2a967c2080.png)

Following settings:
FP_IO_VMETAL 2
FP_IO_HMETAL 3
Really means….
FP_IO_VMETAL 3
FP_IO_HMETAL 4

![image](https://user-images.githubusercontent.com/75379398/114647562-35d43980-9cd5-11eb-8a33-b15d16a61ad7.png)

Move onto running floor planning through run_floorplan command in openLANE (results shown below):

![image](https://user-images.githubusercontent.com/75379398/114647593-42589200-9cd5-11eb-9c55-232b12b1634c.png)

Review placement log file:

~/Desktop/work/tools/openlane_working_dir/openLANE_flows/designs/picorv32a/runs/rob_trial_day2/logs/floorplan/ioPlacer.log

![image](https://user-images.githubusercontent.com/75379398/114647621-51d7db00-9cd5-11eb-8286-8f0cfa649029.png)

Settings correct with my configuration, since I did not set config.tcl so sky130 settings were used– I later modified config.tcl to the correct settings to determine that config file priority was respected, so config.tcl should take priority which it did in the modified run)

Look at generated config.tcl

![image](https://user-images.githubusercontent.com/75379398/114647686-6fa54000-9cd5-11eb-80d7-a09096eba776.png)

#### Review floorplan layout in Magic

Look at generated floorplan:
 
![image](https://user-images.githubusercontent.com/75379398/114647758-8cda0e80-9cd5-11eb-8805-7bead9b8fe05.png)

![image](https://user-images.githubusercontent.com/75379398/114647792-9b282a80-9cd5-11eb-87c9-e1eb850021b2.png)

Initiate Magic:

![image](https://user-images.githubusercontent.com/75379398/114647838-a7ac8300-9cd5-11eb-83ef-891ecf68b0a4.png)

Keys S, V to center layout:

![image](https://user-images.githubusercontent.com/75379398/114647886-b8f58f80-9cd5-11eb-8f4e-8873ac79a81b.png)

Zoom into data inputs (keys ‘Z’ and “shift ‘Z’” for in/out) and use “what” command in tkcon window to find out details of cell/component mouse is over (press ‘S’ over cell before “what”). Bound a region be pressing left mouse button for lower left position and then right mouse button for upper right position (area of interest bound by rectangle):

![image](https://user-images.githubusercontent.com/75379398/114647921-ca3e9c00-9cd5-11eb-8c3c-fe0992494a1a.png)

Zoom into generated cells:

![image](https://user-images.githubusercontent.com/75379398/114647949-d9bde500-9cd5-11eb-9732-f49c85ecb590.png)

Go over standard cells:

![image](https://user-images.githubusercontent.com/75379398/114648007-f1956900-9cd5-11eb-8368-ad1c1f6d5913.png)

![image](https://user-images.githubusercontent.com/75379398/114648016-f823e080-9cd5-11eb-87ea-69571a206b75.png)

run_placement – to run global placement in openlane (results below):

![image](https://user-images.githubusercontent.com/75379398/114648038-0671fc80-9cd6-11eb-9263-c8b07c30debd.png)

Check placement after post placement (now includes custom cells) with magic tool:

![image](https://user-images.githubusercontent.com/75379398/114648074-18539f80-9cd6-11eb-8197-502add1b31d2.png)

![image](https://user-images.githubusercontent.com/75379398/114648092-1f7aad80-9cd6-11eb-902a-0cccda50fd43.png)

Zoom in:

![image](https://user-images.githubusercontent.com/75379398/114648118-2ef9f680-9cd6-11eb-8a69-45c7d74465e4.png)

See details of some cells in tkcon:

![image](https://user-images.githubusercontent.com/75379398/114648144-3b7e4f00-9cd6-11eb-89d7-371900c65b49.png)

# Day 3 – Design Library cell using Magic Layout and ngspice characterization

## Theory
### Labs for CMOS inverter ngspice simulations
#### IO placer revision
#### SPICE deck creation for CMOS Inverter
#### SPICE simulation for CMOS inverter
#### Switching Threshold Vm
#### Static and dynamic simulation of CMOS inverter
### Inception of Layout – CMOS fabrication process
#### Create Active regions
#### Formation of N-well and P-well
#### Formation of gate terminal
#### Lightly doped drain (LDD) formation
#### Source – drain formation
#### Local interconnect formation
#### Higher level metal formation
## Labs
### Inception of Layout – CMOS fabrication process
#### Lab introduction to Sky130 basic layers layout and LEF using inverter

Examine pin layout in Magic:

![image](https://user-images.githubusercontent.com/75379398/114648379-97e16e80-9cd6-11eb-80c5-2b4c7ba80278.png)

Check pin configuration in floorplan.tcl, should indicate equidistance between pins:

![image](https://user-images.githubusercontent.com/75379398/114648411-a596f400-9cd6-11eb-8d2d-04ac8fd96d7f.png)

Change the floorplan pin distribution on the fly within openLANE:
set ::env(FP_IO_MODE) 2
rerun run_floorplan

![image](https://user-images.githubusercontent.com/75379398/114648441-b34c7980-9cd6-11eb-87db-f828af07c134.png)

![image](https://user-images.githubusercontent.com/75379398/114648459-bcd5e180-9cd6-11eb-8342-bf8cb55cab83.png)

Check pin placement through Magic (should be “on top of each other”):

![image](https://user-images.githubusercontent.com/75379398/114648473-c9f2d080-9cd6-11eb-92fe-6924112ac7cb.png)

Zoom in to see pins placed close together and confirm pin details through tkmon:

![image](https://user-images.githubusercontent.com/75379398/114648495-d6772900-9cd6-11eb-8303-d64ca5f4316f.png)

#### Lab steps to create cell layout and extract spice netlist
### Sky130 Tech File Labs
#### Lab steps to create final SPICE deck using Sky 130 tech

Focus on an inverter that we will include within the picorv32a design, need to down design for this inverter. Clone from git as follows:

![image](https://user-images.githubusercontent.com/75379398/114648578-0292aa00-9cd7-11eb-85cb-849d8b849891.png)

Want to examine different layers used in building the inverter.
Locate tech file for magic and copy to the working directory of the inverter:

![image](https://user-images.githubusercontent.com/75379398/114648606-0cb4a880-9cd7-11eb-9f87-2243861514d3.png)

View floorplan of inverter in Magic:

![image](https://user-images.githubusercontent.com/75379398/114648638-1807d400-9cd7-11eb-98bd-12f8cb3e059d.png)

•	PMOS at the top, source connected to Vdd

•	Red line is the Polysilicon

•	NMOS at the bottom, source connected to ground

•	Drain of PMOS/NMOS both connected to output

•	Gate of PMOS/NMOS both connected to input

During the labs, I made a mistake and did not clone to designs directory (the working directory is automatically deleted when remote VM restarted) so I had to redo clone into designs directory (subsequent screen shots) and redo copy of magic file.

Check if highlighted area is NMOS – confirmed:

![image](https://user-images.githubusercontent.com/75379398/114648899-7c2a9800-9cd7-11eb-9bac-f900591cd95b.png)

Check if highlighted area is PMOS – confirmed:

![image](https://user-images.githubusercontent.com/75379398/114648935-877dc380-9cd7-11eb-8258-80a3106bd654.png)

Y output connected to Drain or PMOS and CMOS (click mouse three times):

![image](https://user-images.githubusercontent.com/75379398/114648969-95334900-9cd7-11eb-8501-2975483cd953.png)

Confirm PMOS Source connected to VPWR (highlighted after two S key presses):

![image](https://user-images.githubusercontent.com/75379398/114649002-a3816500-9cd7-11eb-9317-45652293aada.png)

Confirm Source of NMOS connected to VGND (two S key presses shows materials highlighted):

![image](https://user-images.githubusercontent.com/75379398/114649042-b005bd80-9cd7-11eb-9a1d-70d342dfaa88.png)

LEF – protects vendor IP (just contains Abstract not Layout for placement), LEF also called frameview (does not contain logic information)
Check DRC function by removing some areas of abstract in Magic (error reported in tkcon):

![image](https://user-images.githubusercontent.com/75379398/114649083-c14eca00-9cd7-11eb-9cc2-1959cec38bf7.png)

Flush changes through option in “File”

#### Lab steps to characterize inverter using sky130 model files

Now want to determine the logical function of this inverter, do simulations in NGSpice tool; extract files from tkcon console (extract all – from git clone directory):

![image](https://user-images.githubusercontent.com/75379398/114649183-eba08780-9cd7-11eb-8021-25b8a5581090.png)

Now check if a file was extracted in directory concerned:

![image](https://user-images.githubusercontent.com/75379398/114649213-f9560d00-9cd7-11eb-88ef-2531bd3a5388.png)

Run this file in ngspice

Issue following in tkcon

ext2spice cthresh 0 rthresh 0

Extract all the parasitic capacitance resources, no new files created in working directory:

![image](https://user-images.githubusercontent.com/75379398/114649256-0f63cd80-9cd8-11eb-9b43-24c872cf76d8.png)

Issue general SPICE extraction command:-

ext2spice (new sky130_inv.spice file created)

![image](https://user-images.githubusercontent.com/75379398/114649280-20144380-9cd8-11eb-9db1-a69f2c4abda2.png)

Examine sky130_inv.spice file:

![image](https://user-images.githubusercontent.com/75379398/114649298-2c000580-9cd8-11eb-913a-3ca3ec2eb6d1.png)

Need to modify spice files for scale, power and pulse signal to drive characteristics testing of invertor. For scale, go into magic, turn on grid (Window Menu) and zoom in to get box size (enter box in tkcon to get box dimensions):

![image](https://user-images.githubusercontent.com/75379398/114649331-3c17e500-9cd8-11eb-94f1-a90391cafda3.png)

Modify spice deck for:
•	New scale
•	Include pshort and nshort models
•	Define VDD and VSS voltages
•	Define driving waveform pulse characteristics
•	Define characteristics of testing – transient, being driven by a varying waveform over time

Modified sky130_inv.spice shown below:

![image](https://user-images.githubusercontent.com/75379398/114649383-58b41d00-9cd8-11eb-8dec-e0c9d4d41fbd.png)

Explore what information is in the model files:

![image](https://user-images.githubusercontent.com/75379398/114649407-636eb200-9cd8-11eb-9a44-4fb7bb8b8832.png)

![image](https://user-images.githubusercontent.com/75379398/114649425-6c5f8380-9cd8-11eb-86f1-d4754094a764.png)

Name of models in the libraries are not aligned with the names in the spice deck, so this needs to be changed to “pshort_model.0 and nshort_model.0”

![image](https://user-images.githubusercontent.com/75379398/114649453-78e3dc00-9cd8-11eb-909d-512603bb6bd7.png)

Run spice simulation through:-

ngspice sky130_inv.spice

plot y vs time a

![image](https://user-images.githubusercontent.com/75379398/114649494-88fbbb80-9cd8-11eb-8032-37df6934ae73.png)

Zoom into the waveforms (top and bottom):

![image](https://user-images.githubusercontent.com/75379398/114649533-9a44c800-9cd8-11eb-9f72-d91e90cb7166.png)

Spikes at the top and bottom of the waveforms are observed, modify cload capacitance to try and smooth out the spikes
Go into spice deck and increase cload (which is C3) from 0.24fF to 2fF:

![image](https://user-images.githubusercontent.com/75379398/114649566-a7fa4d80-9cd8-11eb-914f-7571443702df.png)

Spikes improved on rerun of ngspice:

![image](https://user-images.githubusercontent.com/75379398/114649595-b6486980-9cd8-11eb-8c08-a0d563e90615.png)

RED waveform is output

BLUE waveform is input

Need to find value of 4 parameters

Rise Time/transition delay – time taken for output waveform to rise from 20% of maximum value to 80% of maximum value

Fall Time  – time taken for output waveform to fall from 80% of maximum value to 20% of maximum value

Propagation delay (high to low) – time difference between output falling 50% and input rising 50%

Propagation delay (low to high) - time difference between output rising 50% and input falling 50%

Calculation of Rise Time/transition delay – time taken for output waveform to rise from 20% of maximum value to 80% of maximum value

Calculation of Rise Time/transition delay – time taken for output waveform to rise from 20% of maximum value to 80% of maximum value

MAX voltage is 3.2V

80% of 3.2 = 2.56V, t=2.44ns

![image](https://user-images.githubusercontent.com/75379398/114649664-d8da8280-9cd8-11eb-9886-9fcd4ce2d482.png)

20% of 3.2 = 0.64V, t=2.18ns

![image](https://user-images.githubusercontent.com/75379398/114649690-e6900800-9cd8-11eb-87ef-976ee4d4656e.png)

Rise Time/transition delay = (2.44-2.18)ns = 0.26ns = 260ps

Calculation of Fall Time  – time taken for output waveform to fall from 80% of maximum value to 20% of maximum value

![image](https://user-images.githubusercontent.com/75379398/114649716-f4de2400-9cd8-11eb-8a20-c7b0041fc353.png)

80% of 3.2 = 2.56V, t=4.05ns (downward slope)

![image](https://user-images.githubusercontent.com/75379398/114649745-0293a980-9cd9-11eb-9ed4-23c31c448163.png)

20% of 3.2 = 0.64V, t=4.10ns (downslope)

Fall Time  = (4.10-4.05)ns = 0.05ns = 50ps

Calculation of Propagation delay (high to low) – time difference between input rising 50% and output falling 50% 
50% of 3.2 = 1.6V 

Input wave rise to 1.6V = 4.04ns

![image](https://user-images.githubusercontent.com/75379398/114649798-13441f80-9cd9-11eb-83bc-c03d689ba089.png)

Output wave (RED) fall to 1.6V = 8.09ns 

![image](https://user-images.githubusercontent.com/75379398/114649826-1f2fe180-9cd9-11eb-9026-6fa7bfd15e59.png)

Propagation delay (high to low)  = (8.09-4.04)ns = 4.05ns

Calculation of Propagation delay (low to high) - time difference between input falling 50% and output rising 50% 

50% of 3.2 = 1.6V 

RED WAVE is output

Input falling (BLUE) fall to 1.6V = 2.15ns

![image](https://user-images.githubusercontent.com/75379398/114649906-41296400-9cd9-11eb-8860-90085878567d.png)

output wave (RED) rise to 1.6V = 2.21ns

![image](https://user-images.githubusercontent.com/75379398/114649938-4d152600-9cd9-11eb-9948-c3b6dae7e8d0.png)

Propagation delay (low to high) = (2.21-2.15)ns = 0.06ns = 60ps

### Presentations from Tim Edwards from Open Circuit Design.
#### Lab introduction to Magic tool options and DRC rules
#### Lab introduction to Sky130 pdk’s and steps to download labs
#### Lab introduction to Magic and steps to load Sky130 tech-rules
#### Lab exercise to implement poly resistor spacing to diff and tap
#### Lab challenge exercise to describe DRC error as geometrical construct
#### Lab challenge to find missing or incorrect rules and fix them

# Day 4 – Pre-layout Timing Analysis and Importance of Good Clock Tree

## Theory
### Timing modelling using delay tables
#### Introduction to timing libs and steps to include new cell in synthesis
#### Introduction to delay tables
#### Delay table using Part 1
#### Delay table using Part 2
### Timing analysis with ideal clocks using openSTA
#### Setup timing analysis and introduction to flip-flop setup time
#### Introduction to clock jitter and uncertainty
### Clock tree synthesis TritonCTS and signal integrity
#### Clock tree routing and buffering using H-Tree algorithm
#### Crosstalk and clock net shielding
### Timing analysis with real clocks using OpenSTA
#### Setup timing analysis using real clocks
#### Hold timing analysis using real clocks

## Labs
### Timing modelling using delay tables
#### Lab steps to convert grid info to track info

Provide information to OpenLANE in the form of a LEF file to allow routing with parameters such as power rails + input/output. Extract LEF file out of .mag file in order to protect IP.

Guidelines for PnR

•	INPUT and OUTPUT port must lie on intersection of vertical and horizontal tracks

•	Width of standard cell should be multiples of the track pitch
•	Height of standard cell should be multiples of vertical pitch
  

Go to directory below to view tracks.info

![image](https://user-images.githubusercontent.com/75379398/114650623-7aae9f00-9cda-11eb-8f0a-66fe73a71ac3.png)

![image](https://user-images.githubusercontent.com/75379398/114650636-813d1680-9cda-11eb-9912-f78d3e73d5c5.png)

Tracks used by routing stage

Routes go over the tracks – used by PnR 

Routes are the metal traces (traces of li1, metal1, metal2)

Specify where routes will go – this information given by tracks

For li1 layer the horizontal (X) tracks have an offset of 0.23 and has pitch of 0.46 

For li1 layer the vertical tracks (Y) have an offset of 0.17 and has pitch of 0.34 


Ensure ports are on same track, verify A and Y are on the intersection of horizontal and vertical tracks on li1 layer

Refer to tracks file for li1 track, so make grid according to these in tkcon:

![image](https://user-images.githubusercontent.com/75379398/114650692-99149a80-9cda-11eb-9d73-a61c6cfc36c2.png)

Run grid command with the li1 track info:

![image](https://user-images.githubusercontent.com/75379398/114650715-a467c600-9cda-11eb-8819-e84229df3321.png)

Zoom into to check if the input/output ports are along the intersection of the horizontal and vertical tracks; this will ensure route can reach the port from the Y and X directions

![image](https://user-images.githubusercontent.com/75379398/114650766-b5183c00-9cda-11eb-8f8a-091120e55d14.png)

#### Lab steps to convert magic layout to std cell LEF

Second requirement is width of standard cell should be multiples of the X direction pitch for that layer (0.46 for li1)
This is confirmed where two boxes with width of .46um and two boxes at the edge each being .23um (measured with grid coordinates – not in screen shot)

To define a label for the port then select the port and add a label:

![image](https://user-images.githubusercontent.com/75379398/114650840-d37e3780-9cda-11eb-94ed-41be8067ce7c.png)

Now assign a purpose for the port – define a port class and use

Do for VGND and VPWR below:-

![image](https://user-images.githubusercontent.com/75379398/114650874-e09b2680-9cda-11eb-8b67-17b2fa1926a8.png)

Do for A port:

![image](https://user-images.githubusercontent.com/75379398/114650907-ee50ac00-9cda-11eb-8d6f-850d8875acb3.png)

Do for Y port:

![image](https://user-images.githubusercontent.com/75379398/114650922-f6a8e700-9cda-11eb-9d50-eb09cbe933bd.png)

Now extract the LEF file

Name the cell and save

Issue command save sky130_vsdinv.mag in tkcon:

![image](https://user-images.githubusercontent.com/75379398/114650949-0294a900-9cdb-11eb-88ed-a65ef6a3ab68.png)

Confirm that new mag file was saved successfully:

![image](https://user-images.githubusercontent.com/75379398/114650976-0e806b00-9cdb-11eb-817a-1cd0aad2d650.png)

Run magic with the new file:

![image](https://user-images.githubusercontent.com/75379398/114651005-1809d300-9cdb-11eb-92bd-7cbeb78d3504.png)

Scan through lef file:

![image](https://user-images.githubusercontent.com/75379398/114651037-20faa480-9cdb-11eb-9a06-15b5525dec86.png)

Selection of a layer and converting to a port will result in a pin being created as is evident in the lef file

Copy lef file to picorv32a/src area:

![image](https://user-images.githubusercontent.com/75379398/114651069-2eb02a00-9cdb-11eb-94b1-48a642e22886.png)

Need to include custom cell into openlane flow, first stage is synthesis where netlist is mapped to the cells in the library performed by abc; need library which has cell definition for synthesis.

View typical cell library “sky130_fd_sc_hd__typical.lib” below:

![image](https://user-images.githubusercontent.com/75379398/114651093-3c65af80-9cdb-11eb-8525-f586a96dfc6e.png)

library (“sky130_fd_sc_hd_tt_025C_1v80”) – this line defines the usage characteristics of the cells of the library

tt – PMOS typical & NMOS typical (in between fast and slow)

025C – operational temperature (25 degrees Cel)

1v80 – operational voltage (1.8V)

copy over fast/slow/typical libraries to picorv32a/src area:

![image](https://user-images.githubusercontent.com/75379398/114651154-51424300-9cdb-11eb-832e-511923a6d952.png)

Modify config.tcl (BEFORE):

![image](https://user-images.githubusercontent.com/75379398/114651175-5d2e0500-9cdb-11eb-923d-64ece681fa0e.png)

LIB_SYNTH – library file for abc mapping

LIB_MIN/MAX/TYPICAL - Used for ST analysis

Modify config.tcl (AFTER mods to include various characteristics libraries and new LEF file):

![image](https://user-images.githubusercontent.com/75379398/114651207-6f0fa800-9cdb-11eb-8ea1-2db858a94f98.png)

Prep design for day4 to use new config.tcl AND add in manually two additional settings to grab appropriate lef files:

![image](https://user-images.githubusercontent.com/75379398/114651242-7d5dc400-9cdb-11eb-8f0c-c4248a02af65.png)

run_synthesis

![image](https://user-images.githubusercontent.com/75379398/114651271-8ea6d080-9cdb-11eb-8ddf-ea15c97f86a8.png)

Synthesis ran with violation:

![image](https://user-images.githubusercontent.com/75379398/114651288-9797a200-9cdb-11eb-8434-cd18cba3bcbf.png)

Timing issue, need to fix slack:

![image](https://user-images.githubusercontent.com/75379398/114651311-a1b9a080-9cdb-11eb-8022-1fee6ce9116b.png)

Scroll back in synthesis report to see the delays being introduced and accumulating
Big delays introduced along the path, see example below:

![image](https://user-images.githubusercontent.com/75379398/114651339-ada56280-9cdb-11eb-9d83-189cd5925c22.png)

Total tns (I re-ran synthesis) reported as

tns : -1629.83ns

Need to examine synthesis related parameters in readme and modify on the fly within Openlane in order to fix the clock slack problem.

#### Lab steps to configure synthesis settings to fix slack and include vsdinv

Examine README.md file to see available parameters to tune:

![image](https://user-images.githubusercontent.com/75379398/114651419-d4639900-9cdb-11eb-81a6-65c29e326495.png)

Establish chip area (differs from Nicholas one…)

![image](https://user-images.githubusercontent.com/75379398/114651438-de859780-9cdb-11eb-8456-90ad394e3fa0.png)

Now compromise between path delay and synthesised area, tune by changing parameters on the fly SYNTH_STRATEGY – reduce to 1 from 2 – reduce area

SYNTH_BUFFERING – set to 1 indicating already enabled so leave – want high fan out nets to be buffered

SYNTH_BUFFERING –   was set to 0 indicating disabled so enabled by setting to 1 - tunes buffer sizing based on strategy

SYNTH_DRIVING_CELL – currently set to “sky130_fd_sc_hd__inv_8” – looks like reasonable setting :  determines level of power required depending on expected fan out of cells (adequate drive strength) 

Made the changes and re-run synthesis to determine if these changes result in a generated netlist that reduces the slack.

Check and setting of these parameters are shown below:

![image](https://user-images.githubusercontent.com/75379398/114651473-f3622b00-9cdb-11eb-9907-491c79cd099b.png)

Check generated chip area, it has increased (as desired):

![image](https://user-images.githubusercontent.com/75379398/114651494-feb55680-9cdb-11eb-9b35-9d1b61823754.png)

Generated floor_plan (run_floorplan) and check merged.lef to determine if the vsd cell has been included in the run (~/Desktop/work/tools/openlane_working_dir/openLANE_flows/designs/picorv32a/runs/rob_trial_day4/tmp/merged.lef):-

![image](https://user-images.githubusercontent.com/75379398/114651546-17257100-9cdc-11eb-96f6-4ea0170c0789.png)

Floorplan execution was sucessful and the cell definition was included in merged.lef so this should result in the cell being included in the placement when that stage is executed.
Now run placement, we see overflow decreasing indicating convergence of placement:

![image](https://user-images.githubusercontent.com/75379398/114651567-24426000-9cdc-11eb-9bfe-22c89ecec15a.png)

Placement ran successfully:

![image](https://user-images.githubusercontent.com/75379398/114651593-2efcf500-9cdc-11eb-95a0-13ce99d94351.png)

Run magic on the placement run:

~/Desktop/work/tools/openlane_working_dir/openLANE_flows/designs/picorv32a/runs/rob_trial_day4/results/placement directory and run:-

magic -T /home/robertedwards/Desktop/work/tools/openlane_working_dir/pdks/sky130A/libs.tech/magic/sky130A.tech lef read ../../tmp/merged.lef def read picorv32a.placement.def &

![image](https://user-images.githubusercontent.com/75379398/114651643-476d0f80-9cdc-11eb-8c2d-84cd4e5a612f.png)

Zoom in to find VSD cell:

![image](https://user-images.githubusercontent.com/75379398/114651672-518f0e00-9cdc-11eb-8365-7f5ed14e8f53.png)

Check identity in tkmon:

![image](https://user-images.githubusercontent.com/75379398/114651696-5b187600-9cdc-11eb-9ea3-5300c270a5df.png)

Confirms custom cell has been integrated into overal placement

Expand into cell and surroundings to view contacts:

![image](https://user-images.githubusercontent.com/75379398/114651724-666ba180-9cdc-11eb-84c6-d14ce3255b9d.png)

Further expansion onto grid:

![image](https://user-images.githubusercontent.com/75379398/114651743-6f5c7300-9cdc-11eb-838b-e7807c06ebde.png)

### Timing analysis with ideal clocks using openSTA
#### Lab steps to configure OpenSTA for post-sythn timing analysis

PnR analysis needs to be performed by a separate tool (Synopsis has Primetime); openlane has its own tool.

Perform STA, add buffers, change netlist with modified clock tree.

In this directory, we have one verilog file BUT after STA there will be an additional modified file:

![image](https://user-images.githubusercontent.com/75379398/114651801-88fdba80-9cdc-11eb-8977-886057e18e2b.png)

Create a config file called pre_sta.conf to be used for STA analysis:-

![image](https://user-images.githubusercontent.com/75379398/114651819-92872280-9cdc-11eb-86c7-46084153d02b.png)

![image](https://user-images.githubusercontent.com/75379398/114651832-97e46d00-9cdc-11eb-8074-c8227befd335.png)

Now need to create my_base.sdc
Start by copying from base.sdc (look at left for path), then made some modifications to add clock definitions and synthesis env variables and modify capacitance load at input to pin “inv__8” (currently at 13.6fF – copied from tutor default setting).
Need to change capacitance from vsdstdcelldesign/libs directory containing the typical characteristics library file (sky130_fd_sc_hd__typical.lib):

![image](https://user-images.githubusercontent.com/75379398/114651854-a29f0200-9cdc-11eb-873f-9f537cbfefb1.png)

Lib file directory/file shown below (from github in designs):

![image](https://user-images.githubusercontent.com/75379398/114651870-adf22d80-9cdc-11eb-8d02-ca9bcc691341.png)

Capacitance is at pin A (inv_8) is 0.0023280000pF, which is 2.32fF, updated my_base.sdc below:

![image](https://user-images.githubusercontent.com/75379398/114651885-b77b9580-9cdc-11eb-8516-06d25536ebd4.png)

Mistake made in my_base (clock settings spread over two lines so STA failed) – corrected below:

![image](https://user-images.githubusercontent.com/75379398/114651906-c2cec100-9cdc-11eb-82e9-525f63185148.png)

Now run STA with sta pre_sta.conf command, gives:-

![image](https://user-images.githubusercontent.com/75379398/114651928-cc582900-9cdc-11eb-8cc4-86781973211b.png)

Same tns (Total Negative Slack), wns (Worst Negative Slack) values as in synthesis run within Openlane.
Look through STA log, see fanout section:

![image](https://user-images.githubusercontent.com/75379398/114652061-0de8d400-9cdd-11eb-8a41-c9247e8b1241.png)

No CTS performed, we are assuming ideal clocks so skew is 0 (which is not realistic).

Board analysis (over optimistic right now) comes after CTS when more realistic clock delay parameters can be fed into processing.

Perform setup analysis where the STA log confirms the times are high in this area:

![image](https://user-images.githubusercontent.com/75379398/114652086-1a6d2c80-9cdd-11eb-9681-b693523d1b37.png)

#### Lab steps to optimize synthesis to reduce setup violations

Delay of any cell is a function of the input slow (high transition) and output load capacitance. These parameters need to be tuned (downwards) where possible to reduce the delay.
Fanout being high (as seen in the trace) will also contribute to the delay.
Need to determine how fan out can be changed. Consult README.md in openlane/configuration

![image](https://user-images.githubusercontent.com/75379398/114652137-2bb63900-9cdd-11eb-8f1a-051b2b3a469c.png)

Change FANOUT in OpenLANE on the fly and re-run synthesis:

![image](https://user-images.githubusercontent.com/75379398/114652164-383a9180-9cdd-11eb-85d0-0339dd853794.png)

Perform analysis of delays, see that large fanout nets are associated with relatively simple drivers (e.g. size 1 buffer):

![image](https://user-images.githubusercontent.com/75379398/114652186-438dbd00-9cdd-11eb-9f13-3a4a5f12b951.png)

# GOT TO THIS POINT IN THE LABS (NEED TO COMPLETE ON LOCAL LINUX PLATFORM)!
### Lab steps to do basic timing ECO
#### Clock tree synthesis TritonCTS and signal integrity
### Clock tree routing and buffering using H-Tree algorithm
### Crosstalk and clock net shielding
### Lab steps to run CTS using TritonCTS
### Lab steps to verify CTS runs
#### Timing analysis with real clocks using OpenSTA
### Setup timing analysis using real clocks
### Hold timing analysis using real clocks
### Lab steps to analyze timing with real clocks using OpenSTA
### Lab steps to execute OpenSTA with right timing libraries and CTS assignment
### Lab steps to observe impact of bigger CTS buffers on setup and hold timing

# Day 5 – Final Steps for RTL2GDS using TritonRoute and OpenSTA 
Unfortunately, I ran out of time so could not start this section :(

## Theory
### Routing and design rule check
#### Introduction to Maze Routing – Lee’s algorithm
#### Lee’s algorithm conclusion
#### Design Rule Check
### Power Distribution Network and routing
#### Lab steps to build power distribution network
#### Lab steps from power straps to std cell power
#### Basics of global and detail routing and configure TritonRoute
### TritonRoute Features
#### TritonRoute feature 1 – Honors pre-processed route guides
#### TritonRoute feature2 & 3 – Inter-guide connectivity and intra-layer routing
#### TritonRoute method to handle connectivity
#### Routing topology algorithm and final files list post-route
## Labs
### Power Distribution Network and routing
#### Lab steps to build power distribution network
#### Lab steps from power straps to std cell power
#### Basics of global and detail routing and configure TritonRoute
### TritonRoute Features
#### TritonRoute feature 1 – Honors pre-processed route guides
#### TritonRoute feature2 & 3 – Inter-guide connectivity and intra-layer routing
#### TritonRoute method to handle connectivity
#### Routing topology algorithm and final files list post-route

# Next Steps
## Setup OpenLANE and VSD sw tools on my LINUX platform and redo course including completing days 4 and 5
## Taking Kunal's VLSI courses on Udemy that cover the areas covered in these courses since there were some areas that I need to improve my understanding on
## Watch recommended OpenLANE material on Youtube
## Aim to reach a level of competence that allows me to take the VSD internship in June

# Acknowledgements
## Tutors on the course who made this great learning experience possible:
### Kunal Ghosh
### Praharsha Mahurkar 
### Nickson Jose
### Tim Edwards













