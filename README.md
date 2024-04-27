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

# Directory Structure of Openlane ="13">  
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
└── scripts  </a>

