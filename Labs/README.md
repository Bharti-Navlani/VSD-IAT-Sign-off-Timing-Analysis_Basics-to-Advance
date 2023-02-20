**Index** 

* [Day 1](#day-1)
    + [Lab 1](#Lab-1)
        - [About OpenSTA](#About-OpenSTA)
        - [Constraints Creation](#Constraints-Creation)
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





