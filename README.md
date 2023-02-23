# SKY130-OpenSTA-workshop

The following repository consists of knowledge gained and steps followed while doing the Sign-off Timing Analysis - Basics to Advanced Using [OpenSTA/SKY130](https://github.com/The-OpenROAD-Project/OpenSTA/blob/master/doc/OpenSTA.pdf) workshop. The [workshop](https://www.vlsisystemdesign.com/sign-off-timing-analysis-basics-to-advanced/) covers all the basic concepts in STA and Timing constraints using open soucrce EDA tools such as OpenSTA/SKY130. 

# Table of Contents 
  * Sky130 Day 1
    * Day 1 Lectures 
      * STA Definition
      * Timing Paths
      * Timing Path elements
      * Setup and Hold Checks
      * Slack Calculation
      * SDC Overview 
      * Clocks
      * Generated Clocks
      * Boundary Constraints 
    * Day 1 Lab
      * OpenSTA Introduction
      * Inputs to OpenSTA 
      * Constraints creation 
      * OpenSTA runscripts 
  * Sky130 Day 2
    * Day 2 Lectures
      * Other Timing Checks
      * Design Rule Checks
      * Latch Timing
      * STA text Report 
    * Day 2 Lab
      * Liberty Files
      * SPEF
      * Timing Reports
  * Sky130 Day 3
    * Day 3 Lectures
      * Multiple Clocks
      * Timing Arcs and Timing Sense 
      * Cell Delays and Clock Network
      * Setup and Hold Detailed 
      * STA Text Report  
    * Day 3 Lab  
      * Understanding Slack Computation 
  * Sky130 Day 4
    * Day 4 Lectures
      * Crosstalk and Noise 
      * Operating modes and other variations 
      * Clock Gating Checks 
      * Checks on Async Pins  
    * Day 4 Lab 
      * Understanding Clock Gating Checks
  * Sky130 Day 5
    * Day 5 Lectures
      * Clock Groups 
      * Clock Properties 
      * Timing Exceptions 
      * Multiple Modes   
    * Day 5 Lab    
      * Slack Computation 
      * CRPR and ECO insertion 
     
     
# Day 1 
## Lectures
### STA Definition

Static Timing Analysis (STA) is a technique used in digital circuit design to verify the timing performance of a circuit. Timing constraints are specifications for the timing behavior of the circuit, including maximum and minimum delay times, clock frequency, setup and hold times, clock skew, and jitter. These constraints are used by the STA tool to perform timing analysis and ensure the circuit meets its timing requirements. Timing constraints are typically specified using a Hardware Description Language (HDL) or a Constraints file, and they are critical in the digital circuit design process to ensure proper functionality.

#### STA Features

* Static in nature: STA is a static analysis technique, which means that it does not involve simulation or execution of the circuit. Instead, it analyzes the timing behavior of the circuit based on its structure and timing constraints.
* Exhaustive: STA performs an exhaustive analysis of the circuit's timing behavior, checking all possible paths and scenarios to ensure that the circuit meets its timing requirements.
* Functionality: STA does not verify the functional correctness of the circuit, only its timing performance. It assumes that the circuit is functionally correct and focuses on ensuring that the timing requirements are met.
* Conservative: STA takes a conservative approach to timing analysis, assuming the worst-case scenario for all timing constraints. This ensures that the circuit meets its timing requirements even under worst-case conditions, but can result in overly pessimistic timing analysis results.
* Synchronous Design: STA is primarily used for synchronous digital circuit designs, which rely on a clock signal to synchronize the inputs and outputs. Asynchronous designs, which do not use a clock signal, require different timing analysis techniques.

#### Inputs to STA 

* Netlist: is a structural representation of the digital circuit design in Verilog or VHDL. It describes the components of the circuit, their connections, and the hierarchy of the design.
* SDC or Constraints file: A Synopsys Design Constraints (SDC) file or other constraints file is a text file that specifies the timing constraints of the digital circuit design. These constraints include the maximum and minimum delay times of each component, the setup and hold times of each data signal, the clock frequency, and the clock skew
* Logic Libraries: are collections of pre-characterized cells that are used to represent the components of the digital circuit design in the netlist. The logic libraries provide the timing information necessary to analyze the timing performance of the circuit.

### Timing Paths 

-> What do you mean by Timing Paths?
* It Refer to the logical paths a signal takes through a digital circuit from its source to its destination, including sequential and combinational elements. STA analyzes timing paths to determine their delay, setup and hold times, and other timing parameters specified in the constraints. Timing paths are categorized into combinatorial and sequential, and the critical path is the longest path in the design with the maximum operating frequency.

### Timing Path Elements
   
Timing path elements in STA are the start point, where a signal originates, the end point, where it terminates, and the combinational logic elements, such as gates, that the signal passes through. Timing paths are traced to determine the overall delay and timing performance of the digital circuit.

* Start Point: Is the point where the signal originates or enters the digital circuit. This point is typically an input port of the design, where the signal is first introduced to the circuit.
* End Point: Is the point where the signal terminates or leaves the digital circuit. This point is typically an output port of the design, where the signal is outputted from the circuit.
* Combinational Logic: Combinational logic elements are the building blocks of a digital circuit and are used to perform logic operations on the signals passing through the circuit. These elements do not store any information, and the output of a combinational logic element is solely determined by the input values at that moment.

### Setup and Hold Checks

->What is Setup Check?
* Is the minimum time that the data must be stable before the clock edge, and if this time is not met, it can lead to setup violations, resulting in incorrect data being stored in the sequential element. The setup check is essential to ensure correct timing behavior of a digital circuit and prevent data loss or other timing-related issues.
* The setup time of a flip-flop depends on the technology node, operating conditions, and other factors. The value of the setup time is usually provided in the logic libraries.

-> What is Hold Check?
* Is the minimum amount of time that the data must remain stable after the clock edge, and if this time is not met, it can lead to hold violations, resulting in incorrect data being stored in the sequential element. The hold check is necessary to prevent issues such as data corruption, metastability, and other timing-related problems in digital circuits.

### Slack Calculation 

Setup and hold slack is defined as the difference between data required time and data arrivals time. 

>Setup slack = Data required time - Data arrival time

-> What is Data Arrival Time?
* The time taken by the signal to travel from the start point to the end point of the digital circuit. 

-> What is Data Required Time? 
* The time for the clock to traverse through the clock path of the digital circuit. 

-> What is Slack? 
* It is difference between the desired arrival times and the actual arrival time for a signal. 
* Positive Slack indicates that the design is meeting the timing and still it can be improved. 
* Zero slack means that the design is critically working at the desired frequency. 
* Negative slack means, design has not achieved the specified timings at the specified frequency.
* Slack has to be positive always and negative slack indicates a violation in timing. 

### SDC Overview 

#### Constraint for Timing 
Over here, we set clocks definition, clock group, clock latency, clock uncertainty, clock transition, input delay, output delay, timing derates etc.

Eg: 
> Create_Clock - The create_clock command creates a clock object in the current design. This command defines the specified source_objects as a clock source.
> Create_generated_clock - The create_generated_clock command creates a generated clock object. A pin or port could be specified for the generated clock object. Generated clock follows the master clock, so whenever the master clock changes generated clock will change automatically.


#### Constraints for Area and Power

The constraints for area and power are specified in the SDC file to ensure that the design meets the required performance, power consumption, and die area.

Eg:
->set_max_area: Specifies the maximum allowed area for the design
->set_max_dynamic_power: Specifies the maximum allowed dynamic power consumption


#### Constraints for Design Rules

They ensure that the design meets the required physical design rules and can be successfully manufactured

Eg:
->set_max_capacitance: Specifies the maximum allowed capacitance on a net or port
->set_min_transition: Specifies the maximum allowed input transition time


#### Constraints for Interfaces

Interfaces in digital design refer to the standard communication protocols that are used to connect different parts of a system.

Eg:
->set_input_delay: Specifies the input delay for an interface signal
->set_driving_cell: tells the synthesis tool which cell to use as the source of the signal on the specified net or pin.
->set_load: Specifies the load capacitance of a particular net or pin


#### Constraints for Modes 

Refers to the different operational modes of a design, such as power-on reset, test mode, or low-power mode.

Eg: 
-> set_case_analysis: Specifies whether or not case analysis should be performed during timing analysis. 
-> set_logic_dc: Used to set the logical value of a signal during static timing analysis.
-> set_logic_one: Used to set the logical value of a signal to a constant one during static timing analysis.

### Clocks

* Create_clock 
  * Is a command used in timing constraint files to define a clock signal that is used to synchronize different parts of a design.
  * It specifies the name of the clock signal, its frequency, and its phase.
* Create_clock -add switch
  * Is an extension of the "create_clock" command, which allows specifying a switch that is used to enable or disable the clock signal.  
  * This switch can be useful in power optimization techniques like clock gating, where the clock signal is selectively gated to reduce power consumption.

### Generated Clocks 

* Is a command used to create a virtual clock signal from one or more physical clock signals.
* takes various input parameters, such as the name of the virtual clock signal, the relationship between the virtual and physical clock signals, and the timing characteristics of the virtual clock signal.
* here is an example 
``` create_generated_clock -divide_by 2 -source C1 -master_clock CLK1 ```

## Lab 
### Opensta Introduction

OpenSTA is a static timing verifier that operates at the gate level and can be used to verify the timing of a design using various file formats, including Verilog netlists, Liberty libraries, SDC timing constraints, SDF delay annotations, and SPEF parasitics. It is a stand-alone executable that can be easily integrated with other tools as a timing engine. OpenSTA is designed to access host netlist data structures without duplication using a network adapter. It supports query-based incremental updates of delays, arrival, and required times, and features a simulator to propagate constants from constraints and tie high/low netlist.

### Inputs to Opensta

#### Steps to follow 
* Accessing the directory:
  * git clone https://github.com/vikkisachdeva/openSTA_sta_workshop
  * cd to lab1 directory using following unix command “cd lab1”
  
* Accessing the verilog file
  * Type “ls” and you will notice one of the file named “simple.v”
  * simple.v is our design in Verilog format
  * Open file using “leafpad simple.v”
  * This file at the bottom will contain standard cells or library cell instantiations as shown in the image
  ![image](https://user-images.githubusercontent.com/62239145/220377617-b081c6a2-83cd-40ba-85d1-120682eb1da5.png)

  
* Accessing the Library file 
  * Type "cd .." to leave the lab1 directory.
  * Type “ls” and you will notice one of the file named “sky130_fd_sc_hd_tt_025C_1v80.lib”

![image](https://user-images.githubusercontent.com/62239145/220379700-a147eb92-3cfd-42de-a396-6e91f8a6189e.png)

  * Open file using “leafpad sky130_fd_sc_hd_tt_025C_1v80.lib”.
  * This contains the library cells that were instantiated in the verilog file.

* Accessing the constraints file
  * Now go back to the lab1 directory by typing "cd lab1" on the terminal
  * Type “ls” and you will notice one of the file named “simple.sdc”
  * To open it enter the leafpad command
  ![image](https://user-images.githubusercontent.com/62239145/220380211-4c76b373-1ae7-4e79-ad10-c8a8c1546aa0.png)
* Runscript
  * The runscript is where you define all the commands you want to run in the openSTA tool
  * "leafpad run.tcl" to have a look at it
  
  ![image](https://user-images.githubusercontent.com/62239145/220382383-8382070f-bdbf-44eb-b7fb-0fb18051564c.png)
  
  * the comments show that the library files, constraint file and netlist are being included which are our 3 inputs for the STA operation. 
  * to execute type "sta run.tcl -exit | tee run.log"
  * execution results are shown in the next section

### Results 

* for start point: inp1 (input port clocked by tau2015_clk) 
* endpoint: out (failling edge-triggered flip-flop clcked by tau2015_clk)

    we get these final results
![image](https://user-images.githubusercontent.com/62239145/220327015-698ee6c9-b4b4-4fb1-b359-0d4f57c3e27c.png)
   as you can see negative slack which means issues still persist 

* for start point: inp1 (input port clocked by tau2015_clk)
* endpoint: f1 (failling edge-triggered flip-flop clcked by tau2015_clk)

we get these final results
![image](https://user-images.githubusercontent.com/62239145/220332172-79da3d2e-f4df-44e1-8a3d-84befcb78093.png)

Positive slack which means the design is meeting the timing requirements.

# Day 2 
## Lectures 
### Other Timing Check 

There are more timing checks to note while performing STA. Apart from the basic setup and hold checks which happen in data pins and clock pins. 

* Clock Gating Check:
  * Is a process that ensures that the clock gating logic is properly designed and does not introduce timing violations.  
  * The check includes checks for propagation delay, skew, and signal integrity to ensure the proper operation of the circuit.

* Async Pin Checks:
  * Is a process that ensures that the asynchronous inputs are properly synchronized and do not cause timing violations in the design.
  * The check includes setup and hold time checks, recovery time checks, pulse width checks, skew checks, and fanout checks to ensure the proper operation of the circuit.

* Data to Data Checks:
  * Are a set of checks that ensure that the data path in a digital circuit meets the timing requirements. 
  * The checks can be performed using slack-based or delay-based methods and include maximum and minimum data delay checks, data transition checks, data skew checks, and data pulse width checks. 

![image](https://user-images.githubusercontent.com/62239145/220392877-f91380b9-ccc2-4d72-b106-7a1cb3acf1ab.png)


### Design Rule Checks

There are more sets of checks which are done by the STA, called the Design Rule Checks.

* Slew/Transition Analysis 
  * Is a process of analyzing the timing behavior of a digital circuit with respect to the output signal transitions.
  * The analysis involves several checks, including maximum slew, minimum slew, slew rate, and slew propagation delay checks. 

* load analysis 
  * Analyzing the timing behavior of a digital circuit with respect to the load capacitance at the output of the circuit. 
  * Load Analysis also involves several checks, including maximum load, minimum load, and fanout checks, to ensure that the circuit functions correctly and meets its timing specifications.

* Clock Skew Analysis
  * Is a process of analyzing the timing behavior of a digital circuit with respect to the variation in arrival times of the clock signal at different points in the circuit.
  * Clock Skew Analysis involves several checks to ensure that the circuit functions correctly and meets its timing specifications.  
 
* Pulse Width Checks
  * Ensure that the pulse widths are within the required limits to prevent timing violations, such as setup and hold violations.


### Latch Timing

While data is launched and captured at the edges in **flip-flops**, **latches** allow for more flexible timing, as data can be accepted at any time before the latch closes. This means that latches can tolerate greater variation in delay times compared to flip-flops. When there are differences in delay times between two logic paths, **time borrowing** is possible in latched-based designs. This means that if one logic path has more delay and the other has less delay, the time is borrowed from the logic path with less delay in the next clock cycle. This helps to maintain the required timing and ensures that the circuit operates correctly.

![image](https://user-images.githubusercontent.com/62239145/220428472-1d1caeef-1063-46e1-a28e-6561d833d778.png)

### STA Text Report
 
![image](https://user-images.githubusercontent.com/62239145/220428686-53475a7a-04ee-467d-b8ce-73748d73db5d.png)

The STA text report generates a report that shows the critical path of the circuit, which is the longest path from an input to an output that determines the maximum delay in the circuit. The report also shows the timing margins, which is the difference between the calculated delay and the maximum allowed delay for the design to function properly.


## Lab 
### Liberty Files 

The .lib file is an ASCII file that contains timing and power parameters for individual circuit components. It includes information such as timing models, interconnect delays, and I/O delay paths. This file is used to calculate maximum timing values, perform timing checks, and optimize design.
The Liberty File Reference from UC Berkley (https://people.eecs.berkeley.edu/~alanmi/publications/other/liberty07_03.pdf) is a useful resource for learning more about this topic.

![image](https://user-images.githubusercontent.com/62239145/220446228-0bebf760-3a44-465c-a227-d512fe73fda6.png)

#### Steps to Follow

* Understanding LIB parsing
 * Type 'cd lab2' to enter the lab2 directory
 * Type 'ls' you will see 2 library files with the following names: simple_min.lib and simple_max.lib
 ![image](https://user-images.githubusercontent.com/62239145/220447445-c3114cc1-b654-487b-9bc1-f4e8d8634645.png)
 * open them using the leafpad command 
 * our task is to 'find all the cells in simple_max.lib'
 * We could use a command grep to display all the instances where the cell is mentioned in the .lib file.  
 * use the code ```grep "cell " simple_max.lib ``` to print all the instances where the keyword **cell** is used
 ![image](https://user-images.githubusercontent.com/62239145/220451495-bf7cbfed-b185-422b-ac19-8d919d40349a.png)
 * Next we find all the pins of the cell NAND2_X1 in simple_max.lib by opening the file and going to where NAN2_X1 is being defined.
 ![image](https://user-images.githubusercontent.com/62239145/220581268-d3614426-e65a-461d-b7ec-82d1f0277d6d.png)
 ![image](https://user-images.githubusercontent.com/62239145/220581503-2de29efb-7630-4cc1-8beb-adb704b02577.png)
 * as you can see the above images, there are 3 pins: o, a and b.
 * The difference between NAND3_X1 and NAND2_X1 is the number of pins and max capacitance value:
   * NAND3_X1 has o, a, b, and c pins and 4.27 max capacitance. 
   * NAND3_X1 has o, a, and b pins and 6.40 max capacitance
 * The two library files, simple_min.lib and simple_max.lib, represent the minimum and maximum delay values of a cell, respectively. Variations in the fabrication process can cause the delay of a cell to either increase or decrease. In order to perform timing analysis, it is crucial to consider both the minimum and maximum delay values provided in the library files. These libraries can be utilized by an STA tool for analysis purposes.
     
### SPEF 

A Standard Parasitic Exchange Format (SPEF) file is used to describe parasitic information related to a design. This file is automatically generated by the tool and users do not have to create it manually. The primary purpose of this file is to transfer parasitic information from one tool to another.

#### Steps to Follow

* Go to the lab2 directory by typing "CD lab2" 
* when you type "ls" you will find a file named 'simple.spef'
![image](https://user-images.githubusercontent.com/62239145/220636152-24a81723-d0c7-44a3-8e4c-b77a800c1208.png)
* Open this file using the 'leafpad' command.
* You will find the file containing 4 sections: Header, Name Map, Top Level Ports and parasitic Description.
![image](https://user-images.githubusercontent.com/62239145/220638281-c8e54ee6-5e93-48db-935e-aa74421a7124.png)
* We will use this file in our next run. 
* Now open the run.tcl file and add the command ``` report_timing -num_paths 5 ``` like shown in the below picture.  
![image](https://user-images.githubusercontent.com/62239145/220639168-02d7f106-bf1f-41f4-be31-7272ced35e6a.png)
* Rerun ``` sta run.tcl -exit | tee run.log``` on the terminal to execute another run. 
* open run.log file using leadfpad command to view the reports. 
![image](https://user-images.githubusercontent.com/62239145/220639683-4535e890-da92-48b8-bd2c-4b9a62e6e707.png)

# Day 3
## Lectures
### Multiple Clocks 

The purpose of using multiple clocks in static timing analysis is to accurately model the timing behavior of a complex digital circuit that has multiple clock domains and to ensure that the circuit meets its timing requirements under all possible operating conditions.

When performing the goal here is to identify the most restrictive setup and hold timing requirements for each clock domain so that the designer can ensure that the circuit meets its timing requirements under all possible operating conditions.

![image](https://user-images.githubusercontent.com/62239145/220838375-11e85ab4-30a4-44a2-949d-8cf77f974c28.png)


Setup check is done with multiple clocks using a common base period, the setup check is done separately for each clock domain.
![image](https://user-images.githubusercontent.com/62239145/220838621-86d8c344-d47a-4fb7-9103-07975a47dd54.png)


Hold Check is done similarly with multiple clocks using a common base period, also following 2 rules.
  * Data launched by setup launch edge must not be captured by previous capture edge 
![image](https://user-images.githubusercontent.com/62239145/220839060-8f6ccbca-3c9a-4340-9201-14cb039dfd31.png)

  * Data launched next launch edge must not be captured by setup capture edge. 
![image](https://user-images.githubusercontent.com/62239145/220839788-189acdfe-51dc-4790-bab6-0523debd8f62.png)


### Timing Arcs and Timing Sense

Timing arcs represent the delay of each logic element or path within the circuit.

Cell arcs represent the delay through a specific cell or gate in the circuit. Cell arcs include information such as input and output delays, setup and hold times, and output slew rates. 

Net arcs, on the other hand, represent the delay through a specific net or wire in the circuit. Net arcs include information such as wire delay and capacitance, as well as any additional delays introduced by buffers or repeaters along the net.  
![image](https://user-images.githubusercontent.com/62239145/220840044-884b0d09-28b6-4ab2-8d77-d7707cfb830a.png)

 
This type of timing arc represents the delay through a combinational logic path. Combinational arcs have a fixed delay that is determined by the gate delay and the wire delay between the input and output of the path.
![image](https://user-images.githubusercontent.com/62239145/220840190-33ef7a87-65d9-46c0-bcfc-24f742c8cd2d.png)


A sequential arc is a type of timing arc in digital circuit design that represents the delay or timing characteristics of a sequential element.
Sequential arcs are important for determining the maximum frequency at which the circuit can operate reliably and identifying potential timing violations.
![image](https://user-images.githubusercontent.com/62239145/220841485-33b07ade-2ee7-4af5-97c7-f379274442b5.png)


### Cell Delays and Clock Network

//What are Cell Delays and how are they calculated?







## Lab 

#### Understanding Slack Computation

#### Steps to Follow 

* Type "cd lab3" and run 'sta run.tcl -exit | tee out.txt'
* As you can see you get a report stating that your timing is being violated 
![image](https://user-images.githubusercontent.com/62239145/220645767-b0558046-98c1-4808-b1c7-4dba063cd2fc.png)
* Now the question is, Which of these paths corresponds to the -217.323 timing violation
![image](https://user-images.githubusercontent.com/62239145/220646354-8e7cb0ef-97fc-4a9f-893e-e4103fed7a7e.png)
![image](https://user-images.githubusercontent.com/62239145/220646389-62d1d10c-8159-4c15-94c1-f31cf23e9e56.png) 
* To find out we open the run.tcl file using the **leafpad** command
* Change the number of paths of the 'F1/CK' to ```-endpoint_count 100``` 
* Below you have images of the test reports for 8 paths after execution
![image](https://user-images.githubusercontent.com/62239145/220863384-772e73d0-7732-4a9e-b70b-3fc54ed66945.png)
![image](https://user-images.githubusercontent.com/62239145/220863579-1d4644e3-fcb3-44f7-8112-412e99a3b973.png)
![image](https://user-images.githubusercontent.com/62239145/220863657-f3dff4ea-3d51-4f11-9725-b050368135c5.png)
![image](https://user-images.githubusercontent.com/62239145/220863836-664ee16c-04dc-4fda-91d8-14e88f0df8b6.png)
![image](https://user-images.githubusercontent.com/62239145/220863904-589f7dc3-ccd8-4451-8f29-3280c0b8c9c8.png)
![image](https://user-images.githubusercontent.com/62239145/220863987-a787e65b-f462-40d9-a980-ee1dc80bf668.png)
![image](https://user-images.githubusercontent.com/62239145/220864131-28fd0372-1ba6-4022-86d7-3287d7d82bfc.png)
![image](https://user-images.githubusercontent.com/62239145/220863205-56247eea-13f2-4b08-b77b-3c83e17e3e04.png)


# Day 4
## Lecture 

## Lab 
### Clock Gating Checks 

#### Steps to Follow 

* Gating techniques illustrated is ‘Active Low Clock Gating’
* Type 'cd lab6' to enter the lab 6 directory
![image](https://user-images.githubusercontent.com/62239145/220867782-26816ea7-b0e4-41be-8c31-6cf6b1793876.png)
* Before we execute the operation lets have all look into the files.
* s27.v
![image](https://user-images.githubusercontent.com/62239145/220867671-cb5e5712-e47f-489d-a034-cdb86b73941f.png)
* s27.sdc (The highlighed line shows the Clock Gating Command which we is our main focue this run)
![image](https://user-images.githubusercontent.com/62239145/220868184-b2e5a532-5eda-4c96-9449-2743699d6d21.png)
* run.tcl
![image](https://user-images.githubusercontent.com/62239145/220867282-e0d402b7-1b49-4e74-a4b9-36958ccca412.png)
* Now, run the command ``` sta run.tcl -exit | tee run.log ```.
![image](https://user-images.githubusercontent.com/62239145/220869942-9b95504e-3ec6-4d32-bd06-09ed76ecd4d3.png)


### Async pin Check

#### Step to Follow

* We will be doing recovery and removal checks 
* First, type 'cd lab7'
* s27.v
![image](https://user-images.githubusercontent.com/62239145/220871328-8f1f5fe1-ae33-4e6b-84d3-7ae8348bed4b.png)
* run.tcl
![image](https://user-images.githubusercontent.com/62239145/220871483-4790b998-25fb-40af-80c8-4ff6237d9d34.png)
* Run using the command ``` sta run.tcl -exit | tee run.log ```.
* ![image](https://user-images.githubusercontent.com/62239145/220871738-8e6aa885-8543-4ec2-82dc-0c794a6dbfb0.png)


# Day 5
## Lectures 

## Lab 
### Understanding Slack Calculation

#### Steps to Follow 


* We first to a normal test run without CPPR 
* Enter lab4 directory using 'cd lab4'
* First we have a look at the files 
![image](https://user-images.githubusercontent.com/62239145/220873586-9211f541-757b-462d-86d6-836e11d93c7e.png)
* s27.v
![image](https://user-images.githubusercontent.com/62239145/220873771-9c354600-8c5a-49af-bbdb-05ee305a6167.png)
* run.tcl
![image](https://user-images.githubusercontent.com/62239145/220882922-807ba625-0ba3-4b73-a73f-0103f2496cd9.png)
* run using the command ``` sta run.tcl -exit | tee run.log ```.
![image](https://user-images.githubusercontent.com/62239145/220883409-e0d4565c-7af4-44e6-8fa7-d64f299d997b.png)

* Now we do the test with CPPR.
* 'c2' is the node which requires CPPR 
* Now change the command below (from 0 to 1)
![image](https://user-images.githubusercontent.com/62239145/220884199-8380761c-acf4-4a4d-ad33-f882f05f3208.png)
* Now run the test again 
![image](https://user-images.githubusercontent.com/62239145/220884431-7810700b-b036-4674-9ccf-2aa6e8c33d54.png)
* We get a new SLACK value.


### ECO - Engineering Change Order

The ECO cycle involves conducting multiple analyses individually for each check that needs to be resolved but has not yet been addressed until the PnR stage. Specialized signoff tools are available to help analyze the issue and recommend changes that need to be made to resolve the issue. These changes are recorded in an ECO file. In this lab, the focus will be on ECO for timing purposes, which involves addressing setup and hold violations.

#### Steps to Follow 

* Go to the lab5 directory and open run.tcl 
![image](https://user-images.githubusercontent.com/62239145/220885746-a17cf461-a5a0-4f8a-b8c7-17cb2f38458e.png)
![image](https://user-images.githubusercontent.com/62239145/220885865-00fc56c6-3c84-44cb-a3c3-1d3ba42d73e7.png)
* Now we open s27_eco.v and s27.v to note the differences.
* s27_eco.v

![image](https://user-images.githubusercontent.com/62239145/220887216-94e48f43-48de-4935-8797-1ca3b58c9570.png)

* s27.v

![image](https://user-images.githubusercontent.com/62239145/220887719-06c425bf-6634-42a3-ba20-33e25c024abd.png)

* Running the tests and comparing

![image](https://user-images.githubusercontent.com/62239145/220888353-dbf0f50e-3855-48d4-9681-435614451a31.png)
![image](https://user-images.githubusercontent.com/62239145/220888666-9d1377f4-b226-4ef5-bcd1-fffcc917308b.png)























