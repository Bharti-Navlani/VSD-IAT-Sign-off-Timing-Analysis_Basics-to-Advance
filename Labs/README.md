**Index** 

* [Day 1](#day-1)
    + [Lab 1](#Lab-1)
        - [About OpenSTA](#About-OpenSTA)
        - [Inputs to OpenSTA](#Inputs-to-OpenSTA)
        - [Run OpenSTA](#Run-OpenSTA)
        
* [Day 2](#day-2)
    + [Lab 2](#Lab-2)
        - [Liberty Files](#Liberty-Files)
        - [Exercise 1](#Exercise-1)
        - [SPEF Files](#SPEF-Files)
        - [Exercise 2](#Exercise-2)
        
 
 
 **Content**
 
> Day1 
 
>> Lab_1 : **About OpenSTA**

        
OpenSTA is a gate level static timing verifier. As a stand-alone executable it can be used to verify the timing of a design using standard file formats like :- 

-Verilog netlist    
-Liberty library    
-SDC timing constraints    
-SDF delay annotation    
-SPEF parasitics
    
OpenSTA is architected to be easily bolted on to other tools as a timing engine. 

By using a network adapter, OpenSTAcan access the host netlist data structures without duplicating them. 

Query based incremental update of delays, arrival and required times & Simulator to propagate constants from constraints and netlist tie high/low

Reference for commands used in OpenSTA :

https://github.com/Bharti-Navlani/VSD-IAT-Sign-off-Timing-Analysis_Basics-to-Advance/files/10786440/OpenSTA.pdf

>> Lab_1 : **Inputs to OpenSTA**

Verilog Model : Contains the Standard Cells

![D1_Lab_1p1](https://user-images.githubusercontent.com/84861735/220170992-4b3945b4-8bab-4da4-b3f4-498e1efebaa2.png)

Liberty File : Standards Cells Information is present in liberty or .lib file 

![D1_Lab_1p2](https://user-images.githubusercontent.com/84861735/220170995-c88730e3-b929-4688-9ade-47fb2c126db3.png)

Below image shows the inputs , output & funtion of sky130_fd_sc_hd___nand2_1

![D1_Lab_1p3](https://user-images.githubusercontent.com/84861735/220172357-5f00372c-de07-48cb-badc-dc56da1e4b9d.png)

Synopsys Constrain File : SDC which contains design constraints  & timing constraints 

![D1_Lab_1p4](https://user-images.githubusercontent.com/84861735/220171001-c15f3c62-5006-4ce4-b319-7b9cf263e785.png)

>> Lab_1 : **run OpenSTA**

Following files shows the steps to read all the above inputs & perform the timing check

![D1_Lab_1p5](https://user-images.githubusercontent.com/84861735/220172393-9ea9a303-ef76-4f21-b539-d5b3964ea0e0.png)


