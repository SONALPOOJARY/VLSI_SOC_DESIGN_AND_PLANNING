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

![VirtualBox_vsdworkshop_21_04_2024_15_32_45.png](https://github.com/SONALPOOJARY/VLSI_SOC_DESIGN_AND_PLANNING/blob/main/VirtualBox_vsdworkshop_21_04_2024_15_32_45.png)

## Lab steps to create std cell layout and extract spice netlist:<a name  ="31">

•	To extract the file from here, we need to enter the command in the tkcon window. The command is `extract all`.  
![VirtualBox_vsdworkshop_21_04_2024_16_08_17.png](https://github.com/SONALPOOJARY/VLSI_SOC_DESIGN_AND_PLANNING/blob/main/VirtualBox_vsdworkshop_21_04_2024_16_08_17.png)  

•	Now let's go to this location from the terminal. it is exctracted.  

![VirtualBox_vsdworkshop_21_04_2024_16_11_32.png](https://github.com/SONALPOOJARY/VLSI_SOC_DESIGN_AND_PLANNING/blob/main/VirtualBox_vsdworkshop_21_04_2024_16_11_32.png)  

•	We'll utilize the .ext file to generate the spice file for use with our ngspice tool. To achieve this, we apply the command `ext2spice cthresh 0 rthresh 0` in the tkcon window. This command doesn't create anything new. Subsequently, we need to re-enter the `ext2spice` command in the tkcon window.  

![VirtualBox_vsdworkshop_21_04_2024_16_11_54.png](https://github.com/SONALPOOJARY/VLSI_SOC_DESIGN_AND_PLANNING/blob/main/VirtualBox_vsdworkshop_21_04_2024_16_11_54.png)  

•	so, now we are checking the location and at there spice file has been created.  
•	Let's examine the contents of the spice file by typing `vim sky130_inv.spice`.  
![VirtualBox_vsdworkshop_21_04_2024_16_12_48.png](https://github.com/SONALPOOJARY/VLSI_SOC_DESIGN_AND_PLANNING/blob/main/VirtualBox_vsdworkshop_21_04_2024_16_12_48.png)  

•	We need to make some changes in the code so that we can make the a transiant simualtion for the given circuit.

![VirtualBox_vsdworkshop_21_04_2024_21_12_40.png](https://github.com/SONALPOOJARY/VLSI_SOC_DESIGN_AND_PLANNING/blob/main/VirtualBox_vsdworkshop_21_04_2024_21_12_40.png)  



![VirtualBox_vsdworkshop_21_04_2024_23_24_32.png](https://github.com/SONALPOOJARY/VLSI_SOC_DESIGN_AND_PLANNING/blob/main/VirtualBox_vsdworkshop_21_04_2024_23_24_32.png)  

![VirtualBox_vsdworkshop_21_04_2024_23_27_24.png](https://github.com/SONALPOOJARY/VLSI_SOC_DESIGN_AND_PLANNING/blob/main/VirtualBox_vsdworkshop_21_04_2024_23_27_24.png)  


