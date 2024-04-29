# VLSI_SOC_DESIGN_AND_PLANNING  

1. [Day 1:](#day1)  
•	[Get Familiar with Open-Source EDA Tools](#11).  
•	[Intro to OpenLane](#12).  
•	[Directory Structure of Openlane](#13)  
•	[Steps to characterize synthesis results](#14)  


2. [Day 2:](#day2)  
•	[Steps to run floorplan using OpenLANE](#21)  
•	[Review floorplan layout in Magic](#22)  
•	[Congestion aware placement using Replace](#23)  


3. [Day3:](#day3)  
•	[Lab introduction to Sky130 basic layers layout and LEF using inverted](#31)  
•	[Lab steps to create std cell layout and extract spice netlist](#32)  
•	[Lab introduction to Magic tool options and DRC rules](#33)  
•	[Lab introduction to Magic and steps to load Sky130 tech-rules](#34)  
•	[Lab exercise to fix poly.9 error in Sky130 tech-file](#35)  

 4. [Day4:](#day4)  
•	[Pre-layout timing analysis and importance of good clock tree](#41)  
•	[Lab steps to configure synthesis settings to fix slack and include vsdinv](#42)  
•	[Lab steps to configure OpenSTA for post-synth timing analysis](#43)   
•	[Lab steps to optimize synthesis to reduce setup violations](#44)  
•	[Lab steps to do basic timing ECO](#45)  
•	[Lab steps to run CTS using Triton](#46)  
•	[Lab steps to verify CTS runs](#47)  
•	[Lab steps to execute OpenSTA with right timing libraries and CTS assignment](#48)  

[Day 5:](#day5)  
•	[Final step for RTL2GDS using tritinRoute and openSTA](#51) 


# Day1:<a name ="day1">   </a>


## Get Familiar with Open-Source EDA Tools <a name ="11"> 
### Basic Bash Commands:  
`cd`   : Change directory.  
`ls`   : List all files.  
`mkdir`: Make a directory.  
`pwd`  : Show the current directory.  
`clear`: Clear the terminal.  
`tree` :This command is used to print the hierarchy of the file system from the present directory.  
`command --help` : shows the complete use that command  </a> 


## Intro to OpenLane:<a name ="12">  
OpenLane is an automated RTLtoGDSII flow. It consists of a variety of open-source tools including:

|    Software	      | Description                  |
| -------------     | -------------                | 
| Yosys	             | Verilog synthesis tool       | 
| Magic             | Layout editor                | 
| Digital           | netlist comparison tool     |               
| Fault	            | Digital fault simulator       | 
| CVC SPEF-Extractor| Circuit verification and analysis tool  | 
| Magic          | Layout editor                | 
| CU-GR	        | Global routing tool    |               
| Klayout	Mask            |  layout editor and viewer       |  


In the Sky130_fd_sc_hd PDK variant, which pertains to the SkyWater foundry's 130nm process, the inclusion of various technology files such as Verilog, SPICE, techlef, meglef, mag, GDS, CDL, lib, and LEF, ensures comprehensive support for design and layout requirements, facilitating efficient development of high-density standard cell libraries.  </a>

## Directory Structure of Openlane:<a name ="13">  
### This the directory structure of the openlane  


openlane  
├── AUTHORS.md  
├── clean_runs.tcl  
├── configuration  
├── conf.py  
├── CONTRIBUTING.md  
├── default.cvcrc  
├── designs  
├── docker_build  
├── docs  
├── flow.tcl  
├── LICENSE  
├── Makefile  
├── README.md  
├── regression_results  
├── report_generation_wrapper.py  
├── run_designs.py  
└── scripts  

### The Design folder have few design.  This includes  

designs  

├── 151  
├── aes  
├── aes128  
├── aes192  
├── aes256  
├── aes_cipher  
├── aes_core  
├── APU  
├── blabla  
├── BM64  
├── chacha  
├── cic_decimator  
├── des  
├── des3  
├── digital_pll_sky130_fd_sc_hd  
├── genericfir  
├── inverter  
├── jpeg_encoder  
├── ldpc_decoder_802_3an  
├── ldpcenc  
├── manual_macro_placement_test  
├── md5  
├── ocs_blitter  
├── picorv32a  
├── point_add  
├── point_scalar_mult  
├── PPU  
├── README.md  
├── s44  
├── salsa20  
├── sha3  
├── sha512  
├── sound  
├── spm  
├── synth_ram  
├── usb  
├── usb_cdc_core  
├── usbf_device  
├── wbqspiflash  
├── xtea  
├── y_dct  
├── y_huff  
└── zipdiv  

### To demonstrate the functionality of our tools, we have selected the picorv32a project. Below is an overview of the contents of picorv32a.  

picorv32a  

├── config.tcl  
├── sky130A_sky130_fd_sc_hd_config.tcl  
├── sky130A_sky130_fd_sc_hdll_config.tcl  
├── sky130A_sky130_fd_sc_hs_config.tcl  
├── sky130A_sky130_fd_sc_ls_config.tcl  
├── sky130A_sky130_fd_sc_ms_config.tcl  
└── src  
    ├── picorv32a.sdc  
    └── picorv32a.v  

The `config.tcl` file encompasses comprehensive details about the design, including specifications regarding enrollment, clock period, and relevant port configurations. Settings specified within the config.tcl file take precedence over default configurations within OpenLane, effectively overriding them.  

## Design Preparation Steps:  

 When you start OpenLANE, you should use the flow.tcl script because it guides you through the process step by step, like following a set path. If you use the interactive switch, you can control each step yourself. Without it, OpenLANE will automatically run all the steps from start to finish. As OpenLANE starts up, you'll notice the prompt change to indicate that it's ready for you to start working.  

 Before accessing the OpenLANE tool, navigate to the `/<path to openlane_working_dir>/openlane`. Here, you'll find the necessary files. To initiate the tool, simply enter the following command in this directory:  

`$  docker`  
`$ ./flow.tcl --interactive`  

  We're opting for the --interactive mode because it allows us to manually execute each tool ourselves. However, if we simply run ./flow.tcl, the entire process from RTL to GDS will be completed automatically.  
  •	we need add the required packages , the below command does that.  
  `$package require openlane 0.9`  
  •	Before running the synthesis , we have to prep the design.  
  `$ prep -design picorv32a`  
  
  RESULT FOR THE ABOVE COMMAND:

![Screenshot (384).png](https://github.com/SONALPOOJARY/VLSI_SOC_DESIGN_AND_PLANNING/blob/main/Screenshot%20(384).png)


•	Preparation is complete; now we can proceed with synthesis. We are now executing the command.  
`$ run_synthesis`  
•	After synthesis , If every thing goes correctly, we can see the terminal result as  
![VirtualBox_vsdworkshop_17_04_2024_23_21_40.png](https://github.com/SONALPOOJARY/VLSI_SOC_DESIGN_AND_PLANNING/blob/main/VirtualBox_vsdworkshop_17_04_2024_23_21_40.png)  </a>

## Steps to characterize synthesis results:<a name ="14">  

According to the synthesis data, there are a total of 1613 D flip-flops and 14876 cells in the design.  

![VirtualBox_vsdworkshop_18_04_2024_00_13_34.png](https://github.com/SONALPOOJARY/VLSI_SOC_DESIGN_AND_PLANNING/blob/main/VirtualBox_vsdworkshop_18_04_2024_00_13_34.png)  


`flop ratio = (number of flip flops) / (number of total cells)`  


The flop ratio, calculated as the ratio of the number of flip-flops to the total number of cells, stands at 10.84%.  

Prior to execution, the result folder appeared empty. However, after completing synthesis, it's evident that ABC has performed all the necessary mapping tasks.  

![VirtualBox_vsdworkshop_17_04_2024_23_31_20.png](https://github.com/SONALPOOJARY/VLSI_SOC_DESIGN_AND_PLANNING/blob/main/VirtualBox_vsdworkshop_17_04_2024_23_31_20.png)  

In the report, we can pinpoint the exact moment when synthesis was completed. The synthesis report now displays the actual statistics, mirroring the information we previously observed.  </a>



# Day2:<a name ="day2">   </a>

## Steps to run floorplan using OpenLANE:<a name  ="21">  

•	To initiate floorplanning, execute the following command.  
`$ run_floorplan`  
![VirtualBox_vsdworkshop_18_04_2024_23_57_52.png](https://github.com/SONALPOOJARY/VLSI_SOC_DESIGN_AND_PLANNING/blob/main/VirtualBox_vsdworkshop_18_04_2024_23_57_52.png)  

•	If floorplanning executes without any errors, the terminal output should resemble the following.  

![VirtualBox_vsdworkshop_18_04_2024_23_58Floorplan results are created in the below locations._07.png](https://github.com/SONALPOOJARY/VLSI_SOC_DESIGN_AND_PLANNING/blob/main/VirtualBox_vsdworkshop_18_04_2024_23_58_07.png)  

•	Floorplan results are created in the below locations.  
`vsduser@vsdsquadron:~/Desktop/work/tools/openlane_working_dir/openlane/designs/picorv32a/runs/17-04_17-25/results/floorplan$ ls
merged_unpadded.lef  picorv32a.floorplan.def  picorv32a.floorplan.def.png`  

-The file picorv32a.floorplan.def contains detailed floorplanning information, while picorv32a.floorplan.def.png provides a visual representation of the floorplan. You can view the floorplan image below for a clearer understanding of the layout.  

![VirtualBox_vsdworkshop_23_04_2024_23_43_34%20floorplan%20color%20output.png](https://github.com/SONALPOOJARY/VLSI_SOC_DESIGN_AND_PLANNING/blob/main/VirtualBox_vsdworkshop_23_04_2024_23_43_34%20floorplan%20color%20output.png)  

</a>


# Review floorplan layout in Magic:<a name  ="22">  

•	To view and edit the floor plan, we can utilize Magic by executing the following command.  

`magic -T /home/<user name>/Desktop/work/tools/openlane_working_dir/pdks/sky130A/libs.tech/magic/sky130A.tech lef read ../../tmp/merged.lef def read picorv32a.floorplan.def`  

•	After executing the above command, a pop-up window of Magic should appear, resembling the following:  

![/VirtualBox_vsdworkshop_20_04_2024_12_07_13.png](https://github.com/SONALPOOJARY/VLSI_SOC_DESIGN_AND_PLANNING/blob/main/VirtualBox_vsdworkshop_20_04_2024_12_07_13.png)  

•	Next, hover your mouse over the design and press `s` to select it. Then, press `v` to center the design on the screen.

![VirtualBox_vsdworkshop_19_04_2024_20_25_17.png](https://github.com/SONALPOOJARY/VLSI_SOC_DESIGN_AND_PLANNING/blob/main/VirtualBox_vsdworkshop_19_04_2024_20_25_17.png)  

•	To select a portion of the design in Magic, left-click on one corner and right-click on another to define the area. Then, press `z`
•	To zoom in on that section. In Magic, press `s` to select sections. After randomly choosing a component, press `s` and shift the Magic command window. Finally, execute the command `% what`.This will show the deatils of the selected component.  


![VirtualBox_vsdworkshop_20_04_2024_12_08_16.png](https://github.com/SONALPOOJARY/VLSI_SOC_DESIGN_AND_PLANNING/blob/main/VirtualBox_vsdworkshop_20_04_2024_12_08_16.png)  

•	You can observe that the Decap cells are positioned along the borders of the side rows.  

![VirtualBox_vsdworkshop_19_04_2024_20_31_50.png](https://github.com/SONALPOOJARY/VLSI_SOC_DESIGN_AND_PLANNING/blob/main/VirtualBox_vsdworkshop_19_04_2024_20_31_50.png)  </a>



## Congestion aware placement using Replace:<a name  ="23">  

•	After completing floorplanning, the next step is placement. There are two types of placement: global placement and detailed placement. When we initiate the placement process, the first stage is global placement. The primary goal of global placement is to minimize wire lengths.  


•	To perform placement, execute the following command:  

`run_placement`  

•	After run _placement  

![VirtualBox_vsdworkshop_20_04_2024_13_13_40.png](https://github.com/SONALPOOJARY/VLSI_SOC_DESIGN_AND_PLANNING/blob/main/VirtualBox_vsdworkshop_20_04_2024_13_13_40.png)  


![VirtualBox_vsdworkshop_20_04_2024_13_29_45.png](https://github.com/SONALPOOJARY/VLSI_SOC_DESIGN_AND_PLANNING/blob/main/VirtualBox_vsdworkshop_20_04_2024_13_29_45.png)  

•	The image `picorv32a.placement.def.png`  is an automatic snapshot of the placement process.

![VirtualBox_vsdworkshop_23_04_2024_23_41_42%20%20placement%20colour%20output.png](https://github.com/SONALPOOJARY/VLSI_SOC_DESIGN_AND_PLANNING/blob/main/VirtualBox_vsdworkshop_23_04_2024_23_41_42%20%20placement%20colour%20output.png)  

•	we view the plcament in magic, execute the command, from the 
 
 `/Desktop/work/tools/openlane_working_dir/openlane/designs/picorv32a/runs/17-04_17-25/results/placement` folder.  

 •	After executing the below command you will get the magic window and here you can observe the the floorplan.  

`magic -T /home/vsduser/Desktop/work/tools/openlane_working_dir/pdks/sky130A/libs.tech/magic/sky130A.tech lef read ../../tmp/merged.lef def read picorv32a.placement.def`  
 
•	If you zoom the image you can see the global placement.  

![VirtualBox_vsdworkshop_20_04_2024_13_30_32.png](https://github.com/SONALPOOJARY/VLSI_SOC_DESIGN_AND_PLANNING/blob/main/VirtualBox_vsdworkshop_20_04_2024_13_30_32.png)  

-GLOBAL PLACEMENT  


![VirtualBox_vsdworkshop_20_04_2024_13_32_12.png](https://github.com/SONALPOOJARY/VLSI_SOC_DESIGN_AND_PLANNING/blob/main/VirtualBox_vsdworkshop_20_04_2024_13_32_12.png)  </a>


# Day3:<a name ="day3">   </a>

## Lab introduction to Sky130 basic layers layout and LEF using inverted:<a name  ="31">  

![VirtualBox_vsdworkshop_21_04_2024_00_50_37.png](https://github.com/SONALPOOJARY/VLSI_SOC_DESIGN_AND_PLANNING/blob/main/VirtualBox_vsdworkshop_21_04_2024_00_50_37.png)  


•	In the Sky130 process, each color represents a different layer. The first layer, indicated by blue-purple, represents the local interconnect. The second layer, depicted in light purple, corresponds to metal 1, while metal 2 is represented by pink. N-well is illustrated with a solid dashed line, green denotes the N-diffusion region, and red represents the polysilicon gate. Additionally, brown signifies the P-diffusion layer.  
•	In the tkcon window, we can identify the selected area as NMOS, and similarly, we can check for PMOS. This allows us to verify the functionality of the CMOS design.  
  

![VirtualBox_vsdworkshop_21_04_2024_15_29_19.png](https://github.com/SONALPOOJARY/VLSI_SOC_DESIGN_AND_PLANNING/blob/main/VirtualBox_vsdworkshop_21_04_2024_15_29_19.png)  

•	similarly we will check for the output terminal also.(by double pressing "S" to select the entire thing at output Y).So, we can see that "Y" is attached to locali in cell def sky130_inv.  
•	we can check the source of the PMOS is connected to the ground or not. and similarly we can check it for NMOS also.  

![VirtualBox_vsdworkshop_21_04_2024_15_32_45.png](https://github.com/SONALPOOJARY/VLSI_SOC_DESIGN_AND_PLANNING/blob/main/VirtualBox_vsdworkshop_21_04_2024_15_32_45.png)  </a>

## Lab steps to create std cell layout and extract spice netlist:<a name  ="32">

•	To extract the file from here, we need to enter the command in the tkcon window. The command is `extract all`.  
![VirtualBox_vsdworkshop_21_04_2024_16_08_17.png](https://github.com/SONALPOOJARY/VLSI_SOC_DESIGN_AND_PLANNING/blob/main/VirtualBox_vsdworkshop_21_04_2024_16_08_17.png)  

•	We'll utilize the .ext file to generate the spice file for use with our ngspice tool. To achieve this, we apply the command `ext2spice cthresh 0 rthresh 0` in the tkcon window. This command doesn't create anything new. Subsequently, we need to re-enter the `ext2spice` command in the tkcon window.  


![VirtualBox_vsdworkshop_21_04_2024_16_11_32.png](https://github.com/SONALPOOJARY/VLSI_SOC_DESIGN_AND_PLANNING/blob/main/VirtualBox_vsdworkshop_21_04_2024_16_11_32.png)  

•	so, now we are checking the location and at there spice file has been created.  
•	Let's examine the contents of the spice file by typing `vim sky130_inv.spice`.  

![VirtualBox_vsdworkshop_21_04_2024_16_11_54.png](https://github.com/SONALPOOJARY/VLSI_SOC_DESIGN_AND_PLANNING/blob/main/VirtualBox_vsdworkshop_21_04_2024_16_11_54.png)  


 
![VirtualBox_vsdworkshop_21_04_2024_16_12_48.png](https://github.com/SONALPOOJARY/VLSI_SOC_DESIGN_AND_PLANNING/blob/main/VirtualBox_vsdworkshop_21_04_2024_16_12_48.png)  

•	We need to make some changes in the code so that we can make the a transiant simualtion for the given circuit.  

![VirtualBox_vsdworkshop_21_04_2024_21_12_40.png](https://github.com/SONALPOOJARY/VLSI_SOC_DESIGN_AND_PLANNING/blob/main/VirtualBox_vsdworkshop_21_04_2024_21_12_40.png)  


•	Then using ngpsice, we can simulate the inverter.  

`$ ngspice sky130_inv.spice`  


•	After executing the command, you will see the output below if there are no errors.  


![VirtualBox_vsdworkshop_21_04_2024_23_24_32.png](https://github.com/SONALPOOJARY/VLSI_SOC_DESIGN_AND_PLANNING/blob/main/VirtualBox_vsdworkshop_21_04_2024_23_24_32.png)  

•	To plot the graphs, enter the command below. You will then observe the waveform.  

`ngspice 1-> plot y vs time a`  

![VirtualBox_vsdworkshop_21_04_2024_23_27_24.png](https://github.com/SONALPOOJARY/VLSI_SOC_DESIGN_AND_PLANNING/blob/main/VirtualBox_vsdworkshop_21_04_2024_23_27_24.png)  

###We can calculate various timing parameters from the above graph:  

- Rise time: It is the time taken by the signal to rise from 20% to 80% of its value. Rise time = (2.2489 - 2.1819) = 66.92 picoseconds.  

  
- Fall time: It is the time taken by the output for transition from 80% to 20% of its value. Fall time = (4.09512 - 4.05264)e-09 = 42.51 picoseconds.  

  
- Propagation delay: It is the time difference between the 50% points of the input and output signals. Propagation delay = (2.2106 - 2.15012)e-09 = 60.48 picoseconds.  

  
- Cell fall delay: It is the time for the output falling to 50% while the input is rising to 50%. Cell fall delay = (4.07735 - 4.04988)e-09 = 27.47 picoseconds.  </a>


## Lab introduction to Magic tool options and DRC rules:<a name  ="33">

•	To download and extract the DRC corrections, execute the following commands.  

`$ wget http://opencircuitdesign.com/open_pdks/archive/drc_tests.tgz  
$ tar xfz drc_tests.tgz  
$ cd drc_tests`  

### In the folder created you will find the all the files as listed below:

drc_Tests  
├── capm.mag  
├── difftap.mag  
├── dnwell.mag  
├── hvtp.mag  
├── hvtr.mag  
├── licon.mag  
├── li.mag  
├── lvtn.mag  
├── mcon.mag  
├── met1.mag  
├── met2.mag  
├── met3.mag  
├── met4.mag  
├── met5.mag  
├── npc.mag  
├── nsd.mag  
├── nwell.mag  
├── pad.mag  
├── poly.mag  
├── psd.mag  
├── rpm.mag  
├── sky130A.tech  
├── tunm.mag  
├── varac.mag  
├── via2.mag  
├── via3.mag  
├── via4.mag  
└── via.mag  </a>

## Lab introduction to Magic and steps to load Sky130 tech-rules:<a name  ="34">  

•	To open Magic, use the following command.  

`$ magic -d XR &`  

![VirtualBox_vsdworkshop_22_04_2024_00_38_04.png](https://github.com/SONALPOOJARY/VLSI_SOC_DESIGN_AND_PLANNING/blob/main/VirtualBox_vsdworkshop_22_04_2024_00_38_04.png)  


![VirtualBox_vsdworkshop_22_04_2024_00_45_42.png](https://github.com/SONALPOOJARY/VLSI_SOC_DESIGN_AND_PLANNING/blob/main/VirtualBox_vsdworkshop_22_04_2024_00_45_42.png)  


![VirtualBox_vsdworkshop_22_04_2024_01_01_38.png](https://github.com/SONALPOOJARY/VLSI_SOC_DESIGN_AND_PLANNING/blob/main/VirtualBox_vsdworkshop_22_04_2024_01_01_38.png)  


•	The `drc why` command provides the reasons for the failure of the system.  


![VirtualBox_vsdworkshop_22_04_2024_00_51_05.png](https://github.com/SONALPOOJARY/VLSI_SOC_DESIGN_AND_PLANNING/blob/main/VirtualBox_vsdworkshop_22_04_2024_00_51_05.png)  

•	Next, select an empty area, hover the mouse pointer over the metal3 contact icon, and press the 'p' button. Then type 'pek' in the tkcon and execute the command 'cif see VIA2' in the tkcon tab. This will display the via information.  

 
![VirtualBox_vsdworkshop_22_04_2024_01_05_11.png](https://github.com/SONALPOOJARY/VLSI_SOC_DESIGN_AND_PLANNING/blob/main/VirtualBox_vsdworkshop_22_04_2024_01_05_11.png)  



![VirtualBox_vsdworkshop_22_04_2024_01_05_29.png](https://github.com/SONALPOOJARY/VLSI_SOC_DESIGN_AND_PLANNING/blob/main/VirtualBox_vsdworkshop_22_04_2024_01_05_29.png)  



•	To remove the information, execute the command "clear feed".  

![VirtualBox_vsdworkshop_22_04_2024_01_06_38.png](https://github.com/SONALPOOJARY/VLSI_SOC_DESIGN_AND_PLANNING/blob/main/VirtualBox_vsdworkshop_22_04_2024_01_06_38.png)  </a>



## Lab exercise to fix poly.9 error in Sky130 tech-file:<a name  ="35">  

•	In order to load poly,execute the below command  


`%load poly`    


![VirtualBox_vsdworkshop_22_04_2024_01_11_35.png](https://github.com/SONALPOOJARY/VLSI_SOC_DESIGN_AND_PLANNING/blob/main/VirtualBox_vsdworkshop_22_04_2024_01_11_35.png)  


•	The wrong rules in the files    

![VirtualBox_vsdworkshop_22_04_2024_01_28_14.png](https://github.com/SONALPOOJARY/VLSI_SOC_DESIGN_AND_PLANNING/blob/main/VirtualBox_vsdworkshop_22_04_2024_01_28_14.png)  


![VirtualBox_vsdworkshop_22_04_2024_01_29_34.png](https://github.com/SONALPOOJARY/VLSI_SOC_DESIGN_AND_PLANNING/blob/main/VirtualBox_vsdworkshop_22_04_2024_01_29_34.png)  

•	The new rules can be added  
•	Changing the drc rules we have to reload the tech file and and check the drc again by running the commands.    

`tech load sky130A.tech`  
`check drc`  
  

![VirtualBox_vsdworkshop_22_04_2024_01_40_06.png](https://github.com/SONALPOOJARY/VLSI_SOC_DESIGN_AND_PLANNING/blob/main/VirtualBox_vsdworkshop_22_04_2024_01_40_06.png)  



![VirtualBox_vsdworkshop_22_04_2024_01_41_39.png](https://github.com/SONALPOOJARY/VLSI_SOC_DESIGN_AND_PLANNING/blob/main/VirtualBox_vsdworkshop_22_04_2024_01_41_39.png)  

</a>  




# Day4:<a name ="day4">   </a>

## Pre-layout timing analysis and importance of good clock tree:<a name  ="41">  

•	To integrate the '.lef' file from the '.mag' file into the picorv32a flow, we must follow specific guidelines for creating standard cells:  

-Ensure that input and output ports align with the intersection of vertical and horizontal tracks.  
-The standard cell's width should be an odd multiple of the track pitch, and its height should be an odd multiple of the track's vertical pitch.  

For further details, refer to the track file located at:  

`pdk/sky130/libs.tech/openlane/sky130_fd_sc_hd/track.info`

![VirtualBox_vsdworkshop_22_04_2024_10_33_26.png](https://github.com/SONALPOOJARY/VLSI_SOC_DESIGN_AND_PLANNING/blob/main/VirtualBox_vsdworkshop_22_04_2024_10_33_26.png)

•	During the routing stage, tracks act as guides for routing metal layers such as metal 1, metal 2, etc.  
•	Automated Place and Route (PNR) necessitates specifying routing paths, which are determined by tracks. Each track is positioned at coordinates (0.23, 0.46)um horizontally and (0.17, 0.34)um vertically for li1, metal 1, and metal 2 layers.  
•	In the layout, ports are located on the li1 layer. To ensure that ports align with the intersection of tracks, we need to convert the grid into tracks.  
•	To achieve this, we'll first open the tracks file and then access the tkcon window to execute the "help grid" command.  

![VirtualBox_vsdworkshop_22_04_2024_10_44_08.png](https://github.com/SONALPOOJARY/VLSI_SOC_DESIGN_AND_PLANNING/blob/main/VirtualBox_vsdworkshop_22_04_2024_10_44_08.png)



![VirtualBox_vsdworkshop_22_04_2024_10_46_53.png](https://github.com/SONALPOOJARY/VLSI_SOC_DESIGN_AND_PLANNING/blob/main/VirtualBox_vsdworkshop_22_04_2024_10_46_53.png)

  

![VirtualBox_vsdworkshop_22_04_2024_10_56_18.png](https://github.com/SONALPOOJARY/VLSI_SOC_DESIGN_AND_PLANNING/blob/main/VirtualBox_vsdworkshop_22_04_2024_10_56_18.png)  

•	To extract the `sky130_inv.lef` and `sky130_vsdinv.mag` files, execute the command `lef write`  

![VirtualBox_vsdworkshop_22_04_2024_11_00_37.png](https://github.com/SONALPOOJARY/VLSI_SOC_DESIGN_AND_PLANNING/blob/main/VirtualBox_vsdworkshop_22_04_2024_11_00_37.png)  

  

![VirtualBox_vsdworkshop_22_04_2024_11_07_50.png](https://github.com/SONALPOOJARY/VLSI_SOC_DESIGN_AND_PLANNING/blob/main/VirtualBox_vsdworkshop_22_04_2024_11_07_50.png)  

•	Then we have to copy the file `sky130_vsdinv.mag` to `src` folder of `picorv32a`, then we can verify the files using the command `ls -ltr`  

![VirtualBox_vsdworkshop_22_04_2024_11_08_02.png](https://github.com/SONALPOOJARY/VLSI_SOC_DESIGN_AND_PLANNING/blob/main/VirtualBox_vsdworkshop_22_04_2024_11_08_02.png)  

•	Make some changes / edit the `config.tcl` file  

![CONFIG.png](https://github.com/SONALPOOJARY/VLSI_SOC_DESIGN_AND_PLANNING/blob/main/CONFIG.png)  


•	Now we will navigate to the OpenLANE directory and execute the Docker command. We'll run the following commands consecutively.  

`./flow.tcl -interactive`  

`package require openlane 0.9` 

`prep -design picorv32a`  

`set lefs [glob $::env(DESIGN_DIR)/src/*.lef]` 

`add_lefs -src $lefs`  

`run_synthesis`  


![VirtualBox_vsdworkshop_22_04_2024_11_51_08.png](https://github.com/SONALPOOJARY/VLSI_SOC_DESIGN_AND_PLANNING/blob/main/VirtualBox_vsdworkshop_22_04_2024_11_51_08.png)  


![VirtualBox_vsdworkshop_22_04_2024_12_56_00.png](https://github.com/SONALPOOJARY/VLSI_SOC_DESIGN_AND_PLANNING/blob/main/VirtualBox_vsdworkshop_22_04_2024_12_56_00.png)  


![VirtualBox_vsdworkshop_22_04_2024_13_01_52.png](https://github.com/SONALPOOJARY/VLSI_SOC_DESIGN_AND_PLANNING/blob/main/VirtualBox_vsdworkshop_22_04_2024_13_01_52.png)  


•	The synthesis was successfully executed.

![VirtualBox_vsdworkshop_22_04_2024_13_03_49.png](https://github.com/SONALPOOJARY/VLSI_SOC_DESIGN_AND_PLANNING/blob/main/VirtualBox_vsdworkshop_22_04_2024_13_03_49.png)  </a>




## Lab steps to configure synthesis settings to fix slack and include vsdinv:<a name  ="42">  

•	We'll check the README file and make changes to the OpenLANE settings accordingly.  

![VirtualBox_vsdworkshop_22_04_2024_13_19_06.png](https://github.com/SONALPOOJARY/VLSI_SOC_DESIGN_AND_PLANNING/blob/main/VirtualBox_vsdworkshop_22_04_2024_13_19_06.png)


•	We can configure environment parameters by running the following command.  

`prep -design picorv32a -tag 01-04_12-54 -overwrite`  

`set lefs [glob $::env(DESIGN_DIR)/src/*.lef]`  

`add_lefs -src $lefs`  

`echo $::env(SYNTH_STRATEGY)`  

`set ::env(SYNTH_STRATEGY) "DELAY 3"`  

`echo $::env(SYNTH_BUFFERING)`  

`echo $::env(SYNTH_SIZING)`  

`set ::env(SYNTH_SIZING) 1`  

`echo $::env(SYNTH_DRIVING_CELL)`  

`run_synthesis`  

`prep -design picorv32a -tag 01-04_12-54 -overwrite`  


•	In the previous run we have observed the negative slack value  

`wns(worst negative slack)= -23.89`  
`tns(total negative slack)= -711.59`  

•	After making the settings changes, we'll rerun the synthesis by executing the command `run_synthesis`.  


![VirtualBox_vsdworkshop_22_04_2024_13_28_49.png](https://github.com/SONALPOOJARY/VLSI_SOC_DESIGN_AND_PLANNING/blob/main/VirtualBox_vsdworkshop_22_04_2024_13_28_49.png)  

- Synthesis successfully executed.  

![VirtualBox_vsdworkshop_22_04_2024_13_30_32.png](https://github.com/SONALPOOJARY/VLSI_SOC_DESIGN_AND_PLANNING/blob/main/VirtualBox_vsdworkshop_22_04_2024_13_30_32.png)  

•	Now we execute the `run_floorplan` command.  

![VirtualBox_vsdworkshop_22_04_2024_13_32_45.png](https://github.com/SONALPOOJARY/VLSI_SOC_DESIGN_AND_PLANNING/blob/main/VirtualBox_vsdworkshop_22_04_2024_13_32_45.png)

•	Since we encountered errors, we need to execute the following command to rectify them for the floorplan.  


`init_floorplan`  
`place_io`  
`tap_decap_or`  



![VirtualBox_vsdworkshop_22_04_2024_13_33_21.png](https://github.com/SONALPOOJARY/VLSI_SOC_DESIGN_AND_PLANNING/blob/main/VirtualBox_vsdworkshop_22_04_2024_13_33_21.png)  


![VirtualBox_vsdworkshop_22_04_2024_13_33_40.png](https://github.com/SONALPOOJARY/VLSI_SOC_DESIGN_AND_PLANNING/blob/main/VirtualBox_vsdworkshop_22_04_2024_13_33_40.png)  


![VirtualBox_vsdworkshop_22_04_2024_13_33_59.png](https://github.com/SONALPOOJARY/VLSI_SOC_DESIGN_AND_PLANNING/blob/main/VirtualBox_vsdworkshop_22_04_2024_13_33_59.png)  

- Now we can proceed with the floorplan by using the command `run_floorplan`.  

![VirtualBox_vsdworkshop_22_04_2024_13_34_32.png](https://github.com/SONALPOOJARY/VLSI_SOC_DESIGN_AND_PLANNING/blob/main/VirtualBox_vsdworkshop_22_04_2024_13_34_32.png)  


-Floorplan run is successsful.  

![VirtualBox_vsdworkshop_22_04_2024_14_07_57.png](https://github.com/SONALPOOJARY/VLSI_SOC_DESIGN_AND_PLANNING/blob/main/VirtualBox_vsdworkshop_22_04_2024_14_07_57.png)  


•	We can view the floor plan by executing the command :  
`magic -T/home/vsduser/Desktop/work/tools/openlane_workingdir/pdks/sky130A/libs.tech/magic/sky130A.tech lef read ../../tmp/merged.lef def read picorv32a.placement.def`  
from the folder   
 `/Desktop/work/tools/openlane_working_dir/openlane/designs/picorv32a/runs/17-04_17-25/results/placement`.  

- Output is shown below


![VirtualBox_vsdworkshop_22_04_2024_14_08_51.png](https://github.com/SONALPOOJARY/VLSI_SOC_DESIGN_AND_PLANNING/blob/main/VirtualBox_vsdworkshop_22_04_2024_14_08_51.png)  


- We can zoom-in to see the layouts


![VirtualBox_vsdworkshop_22_04_2024_14_10_42.png](https://github.com/SONALPOOJARY/VLSI_SOC_DESIGN_AND_PLANNING/blob/main/VirtualBox_vsdworkshop_22_04_2024_14_10_42.png)  



![VirtualBox_vsdworkshop_22_04_2024_14_14_13.png](https://github.com/SONALPOOJARY/VLSI_SOC_DESIGN_AND_PLANNING/blob/main/VirtualBox_vsdworkshop_22_04_2024_14_14_13.png)  </a>




## Lab steps to configure OpenSTA for post-synth timing analysis:<a name  ="43">

•	To conduct STA analysis and identify timing issues, we'll start by running synthesis. Use the following commands in the OpenLANE directory.

`docker`  
`./flow.tcl -interactive`  
`package require openlane 0.9`  
`prep -design picorv32a`  
`set lefs [glob $::env(DESIGN_DIR)/src/*.lef]`  
`add_lefs -src $lefs`  
`set ::env(SYNTH_SIZING) 1`  
`run_synthesis`  

![VirtualBox_vsdworkshop_22_04_2024_22_24_21.png](https://github.com/SONALPOOJARY/VLSI_SOC_DESIGN_AND_PLANNING/blob/main/VirtualBox_vsdworkshop_22_04_2024_22_24_21.png)  

•	Now we have to make a new `pre_sta.conf file`. We can do this by vim editor . We store this `pre_sta.conf` file in `openlane` folder.  

![VirtualBox_vsdworkshop_29_04_2024_01_14_42.png](https://github.com/SONALPOOJARY/VLSI_SOC_DESIGN_AND_PLANNING/blob/main/VirtualBox_vsdworkshop_29_04_2024_01_14_42.png)  

•	Now, we'll create a new file named "my_base.sdc" in the "openlane/designs/picorv32a/src" directory. We'll copy the content from the "base.sdc" file and make the necessary edits as shown below.


![VirtualBox_vsdworkshop_29_04_2024_01_15_09.png](https://github.com/SONALPOOJARY/VLSI_SOC_DESIGN_AND_PLANNING/blob/main/VirtualBox_vsdworkshop_29_04_2024_01_15_09.png)  


•	Then we execute the command sta pre_sta.conf , then we get the result as below


![VirtualBox_vsdworkshop_22_04_2024_21_54_29.png](https://github.com/SONALPOOJARY/VLSI_SOC_DESIGN_AND_PLANNING/blob/main/VirtualBox_vsdworkshop_22_04_2024_21_54_29.png)  


![VirtualBox_vsdworkshop_22_04_2024_23_02_25.png](https://github.com/SONALPOOJARY/VLSI_SOC_DESIGN_AND_PLANNING/blob/main/VirtualBox_vsdworkshop_22_04_2024_23_02_25.png)  


![VirtualBox_vsdworkshop_22_04_2024_23_02_48.png](https://github.com/SONALPOOJARY/VLSI_SOC_DESIGN_AND_PLANNING/blob/main/VirtualBox_vsdworkshop_22_04_2024_23_02_48.png)  </a>  




## Lab steps to optimize synthesis to reduce setup violations:<a name  ="44">  

•	Next, we'll increase the fanout and re-synthesize the circuit by executing the following commands.  

`prep -design picorv32a -tag 02-04_05-27 -overwrite`  
`set lefs [glob $::env(DESIGN_DIR)/src/*.lef]`  
`add_lefs -src $lefs`  
`set ::env(SYNTH_SIZING) 1`  
`set ::env(SYNTH_MAX_FANOUT) 4`  
`echo $::env(SYNTH_DRIVING_CELL)`  
`run_synthesis`  

![VirtualBox_vsdworkshop_22_04_2024_22_24_21.png](https://github.com/SONALPOOJARY/VLSI_SOC_DESIGN_AND_PLANNING/blob/main/VirtualBox_vsdworkshop_22_04_2024_22_24_21.png)  


•	Now run the command `sta pre_sta.conf` .  

![VirtualBox_vsdworkshop_22_04_2024_21_54_29.png](https://github.com/SONALPOOJARY/VLSI_SOC_DESIGN_AND_PLANNING/blob/main/VirtualBox_vsdworkshop_22_04_2024_21_54_29.png)  

![VirtualBox_vsdworkshop_22_04_2024_23_02_25.png](https://github.com/SONALPOOJARY/VLSI_SOC_DESIGN_AND_PLANNING/blob/main/VirtualBox_vsdworkshop_22_04_2024_23_02_25.png)  


![VirtualBox_vsdworkshop_22_04_2024_23_02_48.png](https://github.com/SONALPOOJARY/VLSI_SOC_DESIGN_AND_PLANNING/blob/main/VirtualBox_vsdworkshop_22_04_2024_23_02_48.png)  </a>





## Lab steps to do basic timing ECO:<a name  ="45">  

•	To report all the connections to a net, use the command `report_net -connections _11672_`  
•	To check the command syntax help for replacing a cell, use `replace_cell -help`  
•	To replace the cell with ID "_14510_" with "sky130_fd_sc_hd__or3_4", execute `replace_cell _14510_ sky130_fd_sc_hd__or3_4`  
•	To generate a custom timing report with fields "net cap slew input_pins" and four digits, use `report_checks -fields {net cap slew input_pins} -digits 4`  

![VirtualBox_vsdworkshop_22_04_2024_22_50_57.png](https://github.com/SONALPOOJARY/VLSI_SOC_DESIGN_AND_PLANNING/blob/main/VirtualBox_vsdworkshop_22_04_2024_22_50_57.png)  

![VirtualBox_vsdworkshop_22_04_2024_22_59_16.png](https://github.com/SONALPOOJARY/VLSI_SOC_DESIGN_AND_PLANNING/blob/main/VirtualBox_vsdworkshop_22_04_2024_22_59_16.png)  

![VirtualBox_vsdworkshop_22_04_2024_23_02_25.png](https://github.com/SONALPOOJARY/VLSI_SOC_DESIGN_AND_PLANNING/blob/main/VirtualBox_vsdworkshop_22_04_2024_23_02_25.png)  

![VirtualBox_vsdworkshop_22_04_2024_23_02_48.png](https://github.com/SONALPOOJARY/VLSI_SOC_DESIGN_AND_PLANNING/blob/main/VirtualBox_vsdworkshop_22_04_2024_23_02_48.png)  </a>  




## Lab steps to run CTS using Triton:<a name  ="46">  


•	Now, after making changes, we'll write the Verilog file using the command "write_verilog", and the file will be located in  

`/home/vsduser/Desktop/work/tools/openlane_working_dir/openlane/designs/picorv32a/runs/22-04_09-27/results/synthesis/picorv32a.synthesis.v`  

![VirtualBox_vsdworkshop_23_04_2024_09_14_45.png](https://github.com/SONALPOOJARY/VLSI_SOC_DESIGN_AND_PLANNING/blob/main/VirtualBox_vsdworkshop_23_04_2024_09_14_45.png)  

![VirtualBox_vsdworkshop_23_04_2024_09_21_00.png](https://github.com/SONALPOOJARY/VLSI_SOC_DESIGN_AND_PLANNING/blob/main/VirtualBox_vsdworkshop_23_04_2024_09_21_00.png)  


•	Now, we won't redo the synthesis to avoid clearing our modifications. Instead, we'll continue from the floor plan stage. To run the floor plan, we'll proceed by executing the following commands.  

   `prep -design picorv32a -tag 22-04_09-27 `  
   `set lefs [glob $::env(DESIGN_DIR)/src/*.lef]`  
   `add_lefs -src $lefs`  
   `set ::env(SYNTH_STRATEGY) "DELAY 3"`  
   `set ::env(SYNTH_SIZING) 1`  
   `init_floorplan`  
   `place_io`  
   `tap_decap_or`  
   `run_placement`      

   `# Incase getting error will use this command    
   unset ::env(LIB_CTS)`  

![VirtualBox_vsdworkshop_23_04_2024_09_34_42.png](https://github.com/SONALPOOJARY/VLSI_SOC_DESIGN_AND_PLANNING/blob/main/VirtualBox_vsdworkshop_23_04_2024_09_34_42.png)  

![VirtualBox_vsdworkshop_23_04_2024_09_35_59.png](https://github.com/SONALPOOJARY/VLSI_SOC_DESIGN_AND_PLANNING/blob/main/VirtualBox_vsdworkshop_23_04_2024_09_35_59.png)  


![VirtualBox_vsdworkshop_23_04_2024_09_36_19.png](https://github.com/SONALPOOJARY/VLSI_SOC_DESIGN_AND_PLANNING/blob/main/VirtualBox_vsdworkshop_23_04_2024_09_36_19.png)  



![VirtualBox_vsdworkshop_23_04_2024_09_37_16.png](https://github.com/SONALPOOJARY/VLSI_SOC_DESIGN_AND_PLANNING/blob/main/VirtualBox_vsdworkshop_23_04_2024_09_37_16.png)  


![VirtualBox_vsdworkshop_23_04_2024_09_37_57.png](https://github.com/SONALPOOJARY/VLSI_SOC_DESIGN_AND_PLANNING/blob/main/VirtualBox_vsdworkshop_23_04_2024_09_37_57.png)  

- Finally we run the command `run_cts`  

![VirtualBox_vsdworkshop_23_04_2024_09_38_33.png](https://github.com/SONALPOOJARY/VLSI_SOC_DESIGN_AND_PLANNING/blob/main/VirtualBox_vsdworkshop_23_04_2024_09_38_33.png)  


- After successful execution of the command, it will generate the` picorv32a.cts.def` file. This file will be used for power planning and further steps.  


![VirtualBox_vsdworkshop_23_04_2024_09_52_01.png](https://github.com/SONALPOOJARY/VLSI_SOC_DESIGN_AND_PLANNING/blob/main/VirtualBox_vsdworkshop_23_04_2024_09_52_01.png)

- magic -T output
  
![VirtualBox_vsdworkshop_23_04_2024_09_53_39.png](https://github.com/SONALPOOJARY/VLSI_SOC_DESIGN_AND_PLANNING/blob/main/VirtualBox_vsdworkshop_23_04_2024_09_53_39.png)  


- Output in color 
![VirtualBox_vsdworkshop_23_04_2024_23_40_52%20cts%20colour%20output.png](https://github.com/SONALPOOJARY/VLSI_SOC_DESIGN_AND_PLANNING/blob/main/VirtualBox_vsdworkshop_23_04_2024_23_40_52%20cts%20colour%20output.png)  </a>



## Lab steps to verify CTS runs:<a name  ="47">  

•	We are using all these atomic commands like run_synthesis ,run_floorplan and etc.., are these are stores in 
 `/home/vsduser/Desktop/work/tools/openlane_working_dir/openlane/scripts/tcl_commands` . This folder contains.  

 ![tcl%20commands.png](https://github.com/SONALPOOJARY/VLSI_SOC_DESIGN_AND_PLANNING/blob/main/tcl%20commands.png)  

 - Lets see the `cts.tcl` file  

 ![](https://github.com/SONALPOOJARY/VLSI_SOC_DESIGN_AND_PLANNING/blob/main/VirtualBox_vsdworkshop_23_04_2024_10_11_39.png)  

 ![](https://github.com/SONALPOOJARY/VLSI_SOC_DESIGN_AND_PLANNING/blob/main/VirtualBox_vsdworkshop_23_04_2024_10_19_08.png)  

•	In Openroad , observe that it does not contain a synthesis file.  


![](https://github.com/SONALPOOJARY/VLSI_SOC_DESIGN_AND_PLANNING/blob/main/openroad%20command%20in%20openlane.png)  

-To create a database in OPENROAD using LEF and TMP files, follow these steps:
1.Ensure that you are in the directory containing the LEF and TMP files.  

2.Launch the OPENROAD tool by entering the command: openroad inside the openlane floor.  


![](https://github.com/SONALPOOJARY/VLSI_SOC_DESIGN_AND_PLANNING/blob/main/openroad%20files.png)  



- Once inside the OPENROAD tool, execute the following commands to read the LEF and DEF files:  
`read_lef /openLANE_flow/designs/picorv32a/runs/22-04_09-27/tmp/merged.lef`  
`read_def /openLANE_flow/designs/picorv32a/runs/22-04_09-27/results/cts/picorv32a.cts.def`  

- To create the OpenROAD database file named pico_cts.db, use the command:  
 `write_db pico_cts.db`


![](https://github.com/SONALPOOJARY/VLSI_SOC_DESIGN_AND_PLANNING/blob/main/read_lef%20and%20def.png)  

- we  can  read the `db` file using the command `read_db pico_cts.db`

![](https://github.com/SONALPOOJARY/VLSI_SOC_DESIGN_AND_PLANNING/blob/main/read%20db.png)

## To read the netlist post CTS :  

`read_verilog /openLANE_flow/designs/picorv32a/runs/22-04_09-27/results/synthesis/picorv32a.synthesis_cts.v`  

•	To read the library for the design, use the command "read_liberty $::env(LIB_SYNTH_COMPLETE)".  
•	Next, link the design and library with "link_design picorv32a".  
•	Then, read the custom SDC we created with "read_sdc /openLANE_flow/designs/picorv32a/src/my_base.sdc".  
•	Set all clocks as propagated clocks by executing "set_propagated_clock [all_clocks]".  
•	Generate a custom timing report using the command "report_checks -path_delay min_max -fields {slew trans net cap input_pins} -format full_clock_expanded -digits 4".  
•	Finally, to exit from the OpenROAD flow and return to the OpenLANE flow, type "exit".  
  

![](https://github.com/SONALPOOJARY/VLSI_SOC_DESIGN_AND_PLANNING/blob/main/read%20sdc.png)

![](https://github.com/SONALPOOJARY/VLSI_SOC_DESIGN_AND_PLANNING/blob/main/to%20read%20the%20lib%20for%20design.png)  

- The results of the report  

![](https://github.com/SONALPOOJARY/VLSI_SOC_DESIGN_AND_PLANNING/blob/main/slack%20met.png)  

![](https://github.com/SONALPOOJARY/VLSI_SOC_DESIGN_AND_PLANNING/blob/main/result%20of%20the%20report%20cheks.png)  








## Lab steps to execute OpenSTA with right timing libraries and CTS assignment:<a name  ="48">


                           

