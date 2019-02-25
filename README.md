# SEVAX (Soft Error Vulnerability Analysis Framework for Xilinx FPGAs)  

## Requirements for Installation

 - Windows Xp/Vista/7 or Linux
 - Xilinx Ise (version supported from Rapidsmith Framework)
 - JDK 1.6 or earlier
 - Rapidsmith Framework (installation instructions)
 
### Steps for Installation
 - Make sure the Xilinx tools and JDK are on your PATH.
 - Add all the jar files located into the **unipi/jars** (*commons-cli-1.2.jar*, *commons-cli-1.2-javadoc.jar*, *commons-cli-1.2-sources.jar*, *commons-collections4-4.0-alpha1.jar*, *commons-collections4-4.0-alpha1-javadoc.jar*, *commons-collections4-4.0-alpha1-sources.jar*, *jxl.jar*) to your CLASSPATH environment variable.
 - Extract and import the unipi folder into the  Rapidsmith project

## Overview

The following figure depicts the main functions supported by the SEVAX (Soft Error Vulnerability Analysis framework for Xilinx FPGAs) framework. The user is free to run the entire flow and calculate the soft error vulnerability of the final FPGA design or run individual tools at the intermediate stages of the flow for an early sensitivity estimation of the design.

![](https://github.com/unipieslab/sevax/blob/master/pics/flowjpg.jpg)

The functions supported by our framework are the following:

 - Post–mapping analysis of the block configuration bits: It extracts the FPGA resource utilization data from the XDL netlist produced by the packing/mapping step and analyses the sensitivity of the block configurations bits based on a precompiled resource usage profile of the target architecture.
 - Post–placement analysis of the interconnection configuration bits: It takes into consideration the actual sites of the resources obtained by the placement process and analyses the possibility of a net connecting two or more components to become open or short due to a soft error in a programmable interconnection point (PIP).
 - Post–routing analysis of the interconnection configuration bits: It provides a more accurate analysis since it relies on the final routed circuit. It considers all possible defects that can be caused by a soft error in a PIP, i.e. open faults, bridging faults and antenna faults. The results are written in a text file (.rsba stands for routing sensitive bit analysis) for further processing.
 - Analysis of the Xilinx report for essential configuration bits: It parses the Xilinx sensitivity report (essential bitmap file, .ebd) and the bitstream file using RapidSmith packages and classifies the essential bits into block, interface and interconnection bits and allocates them to configuration frames. This step generates the xsba file used in the visualization of the ebd sensitive bits on the FPGA layout.
 - Visualization of soft-error vulnerable areas: A Graphic tool built as an extension of the Rapidsmith Device.Explorer class reads the results from the two previous analysis steps and illustrates the vulnerable areas of the FPGA layout.
 - Placing a design with the simulated annealing placer SA Placer.

More information can be found in the "Development of a soft error vulnerability analysis framework for FPGA devices" master thesis.

## Running the SEVAX framework

SEVAX framework is organized into several packages (all packages are prefixed with “**unipi.sevax**”)

The user can run the tools of the framework from the user interface class (**userInterface.java**), which is located in the **unipi.sevax.userInterface** package or running the runnable Jar file **sevax.jar**.

The arguments of the sevax.jar tool are the following: 

 -e             Epsilon value. Default 0.005
 -ebd         Ebd Analysis
 -h             Help.
 -l              Redirect Console to log file.
 -m            Moves per temperature multiplier. Default 10
 -p             The path of design.
 -pm          Post-Mapping Analysis
 -pp           Post-Placement Analysis
 -pr           Post-Routing Analysis
 -sa           Place with SA
 -ucf          Path of ucf file
 -xml          Path of xml file
For example you could type: "java -jar sevax.jar -p DesignDirectory -xml sensitiveBits.xml -pm -pp" for Post-Mapping and Post-Placement analysis.

Below, two examples are provided. In the first example we run the entire flow for a benchmark (mapped, placed, routed and bitstream generated). In the second example we run the entire flow for a mapped benchmark. The SEVAX tool will place the design with simulated annealing placer, route it and generate the bitstream with Xilinx tools.

**Example 1**

Download the benchmarks.rar and extract them.
Download the sevax.jar.
Download the arch.xml.
Type **java -jar sevax.jar -p benchmarks/barrel64Ready -xml arch.xml -pm -pp -pr -ebd**
After the analysis you should have the same result with benchmarks/barrel64Ready_log.txt text file.
Run the Graphic tool "/rapidSmith/unipi/sevax/analysis/SensitiveDeviceBrowser.java" from Eclipse to see the vulnerable areas of the FPGA layout.
Choose the the benchmarks/barrel64Ready folder.

**Example 2**

Type **java -jar sevax.jar -p benchmarks/barrel64 -xml arch.xml -pm -pp -sa -pr -ebd**
After the analysis you should have the same result with benchmarks/barrel64Ready_log.txt text file.
Run the Graphic tool "/rapidSmith/unipi/sevax/analysis/SensitiveDeviceBrowser.java" from Eclipse to see the vulnerable areas of the FPGA layout.
Choose the the benchmarks/barrel64 .