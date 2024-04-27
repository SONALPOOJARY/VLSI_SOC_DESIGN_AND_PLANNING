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


# Intro to OpenLane:<a name ="12">  
### OpenLane is an automated RTLtoGDSII flow. It consists of a variety of open-source tools including:

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

# Directory Structure of Openlane:<a name ="13">  
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

# Steps to characterize synthesis results:<a name ="14">  

According to the synthesis data, there are a total of 1613 D flip-flops and 14876 cells in the design.  

![VirtualBox_vsdworkshop_18_04_2024_00_13_34.png](https://github.com/SONALPOOJARY/VLSI_SOC_DESIGN_AND_PLANNING/blob/main/VirtualBox_vsdworkshop_18_04_2024_00_13_34.png)  

The flop ratio, calculated as the ratio of the number of flip-flops to the total number of cells, stands at 10.84%.  

Prior to execution, the result folder appeared empty. However, after completing synthesis, it's evident that ABC has performed all the necessary mapping tasks.  

![VirtualBox_vsdworkshop_17_04_2024_23_31_20.png](https://github.com/SONALPOOJARY/VLSI_SOC_DESIGN_AND_PLANNING/blob/main/VirtualBox_vsdworkshop_17_04_2024_23_31_20.png)  

In the report, we can pinpoint the exact moment when synthesis was completed. The synthesis report now displays the actual statistics, mirroring the information we previously observed.  </a>






