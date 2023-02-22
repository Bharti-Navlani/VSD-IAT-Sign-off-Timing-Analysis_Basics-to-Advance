* [Day 1](#day-1)
    + [STA Definition & Features](#STA-Definition-&-Features)
    + [Timing Paths](#Timing-Paths)
    + [Setup & Hold Checks](#Setup-&-Hold-Checks)
    + [Slack Calculation](#Slack-Calculation)
    + [SDC Overview](#SDC-Overview)
    + [Clocks Constraints](#Clock-Constraints)
    + [Port Delays and Boundary Constrains](#Port-Delays-and-Boundary-Constrains)
   
* [Day 2](#day-2)
    + [Other Timing Checks](#Other-Timing-Checks)
    + [Design Rule Checks](#Design-Rule-Checks)
    + [Latch Timing](#Latch-Timing)
    + [STA Text Report](#STA-Text-Report)
   
* [Day 3](#day-3)
    + Multiple Clocks(#multiple-clocks)
    + Types of Arch(#Types-of-Arch)
    + Cell Delays(#Cell-Delays)
    + Clock Skew(#Clock-Skew)
    + Clock Latency(#Clock-Latency)
    + Clock Jitter(#Clock-Jitter)
    + Setup Check(#Setup-Check)
    + Hold Check(#Hold-Check)
    + CRPR Clock Reconvergence Pessimism Removal(#CRPR-Clock-Reconvergence-Pessimism-Removal)
    

* [Day 4](#day-4)
    + Crosstalk and Noise(#Crosstalk-and-Noise)
    + Process Variation(#Process-Variation)
    + Clock Gating Checks(#Clock-Gating-Checks)
    + Checks on Async Pins(#Checks-on-Async-Pins)



* [References](#references)
* [Acknowledgement](#acknowledgement)



# Day 1 

### STA definition
Static timing analysis (STA) is a method of validating the timing performance of a design by checking all possible paths for timing violations. 

-   STA breaks a design down into timing paths 
-   calculates the signal propagation delay along each path 
-   checks for violations of timing constraints inside the design and at the input/output interface

STA takes the following inputs to perform timing analysis :- 

- Netlist (gate level representation of the design)
- SDC or Constrains file (what you want to time)
- Logic Libraries (information about cells delays mapping to silicon)

### STA Features

1) Static - Does not run dynamic simulation, hence faster & can be run on large capacity design
2) Exhaustive - Uses mathematical Techniques instead of input vectors , hence timing checks are performed on all possible scenarios
3) Functionality - Does not verify functionality of the design
4) Conservative - pessimistic & conservative 

### Timing Paths 

In order to perform the timing analysis , STA breaks the path into small paths at ports & at sequential elements and these paths are known as timing paths.

Following the timing path elements :- 

- **Startpoint** - it is the  point where data is launched by a clock edge or where the data must be available at a specific time.  it can be startpoint must be either an input port or a register clock pin.
- **Endpoint** - The end of a timing path where data is captured by a clock edge or where the data must be available at a specific time. it can be either a register data input pin or an output port.
- **Combinational logic network** - Elements that have no memory or internal state. 

Following design shows the different path with their start & end point 

![Fig_1p2_Timing_Path](https://user-images.githubusercontent.com/84861735/220678327-c1ccc5c3-81f3-43d4-a69e-b34933a6b23a.png)

Following design shows the different path for combinational logic :- 

![Fig_2p3_Combo_Logic](https://user-images.githubusercontent.com/84861735/220183305-f51887b2-c3b5-4fe4-b2b3-438f996109b1.png)

### Setup & Hold Checks

- A **setup check** specifies how much time it is necessary for data to be available at the input of a sequential element before  clock edge of the element that captures the data in the device. This check enforces a **maximum delay** on the data path relative to the clock edge. 

![Fig_3p1_Setup_Check](https://user-images.githubusercontent.com/84861735/220183308-85bf7bf8-f7d4-46aa-a77b-3c61c81b9249.png)

- A **hold check** specifies how much time is necessary for data to be stable at the input of a sequential device after the clock edge that captures the data in the device. This check enforces a **minimum delay** on the data path relative to the clock edge.

![Fig_3p2_Hold_Check](https://user-images.githubusercontent.com/84861735/220183311-acedfb7c-38b6-4264-b465-ae4b414e2117.png)

Setup & hold time will vary as per techonology node & for STA analysis it is available in logical libraries.

### Slack Calculation 

    Slack = Data Requiured Time - Data Arrival Time 
    
#### Data Arrival Time 
![Fig_4p2_Data_Arrival_Time2](https://user-images.githubusercontent.com/84861735/220183314-3743193c-50a8-459b-b316-12ebca02af29.png)

#### Data Required Time
![Fig_4p3_Data_Required_Time](https://user-images.githubusercontent.com/84861735/220183318-9e235ed4-db5c-43c0-b3ee-f5ba98460057.png)

Slack is function of clock frequency & setup time requirement of the end time flop

When difference between data required time & data arrival time is positive then timing is MET !
![Fig_5p1_Slack](https://user-images.githubusercontent.com/84861735/220183319-99cab57e-32d2-447d-9953-bc274c1130fa.png)

When difference between data required time & data arrival time is positive then timing is VIOLATED ! 
![Fig_5p2_Slack](https://user-images.githubusercontent.com/84861735/220183323-a37c0b7c-0615-4c4e-ae5c-b3ebb65e03b2.png)

When difference between data required time & data arrival time is zero then timing is just MET ! 


Consider a senario in which we have combo logic between two flops with delay of Ld
In addition to above , we also need to be consider setup time of the launch flop so 

    Arrival = Tckq + Ld
    
    Arrival < Tperiod - Tsetup(of capture flop) 

![Fig_5p3_Slack](https://user-images.githubusercontent.com/84861735/220183328-2bebc7c8-ec63-47f6-9e6a-554b75f4d71a.png)


### SDC Overview 

- Timing constraints are defined in some specific format which is known as , Synopsys Design Constraint (SDC), is used to specify timing and other design constraints
- SDC commands are the set of commands which are provided as an input to STA Tool to do timing analysis
- SDC contains set of commands which are listed as below :- 

#### Constraints for Timing
![SDC1](https://user-images.githubusercontent.com/84861735/220692469-759039de-5f2e-47e7-b999-81819933c151.png)
#### Constraints for Area and Power
![SDC2](https://user-images.githubusercontent.com/84861735/220692472-8e14339e-68fd-4353-976c-e4422c052e69.png)
#### Constraints for Design Rule
![SDC3](https://user-images.githubusercontent.com/84861735/220692481-dbb7b555-48c2-4b2a-bc97-4eb1fab851b9.png)
#### Constraints for Interfaces
![SDC4](https://user-images.githubusercontent.com/84861735/220692446-046fe66c-433a-4161-bc2c-cad47239bb11.png)
#### Constraints for Modes
![SDC5](https://user-images.githubusercontent.com/84861735/220692457-6f78afae-c56b-4344-b9f1-e93675a6fc59.png)
#### Exceptions to Design Constraints
![SDC6](https://user-images.githubusercontent.com/84861735/220692464-ad640726-ac54-4924-bd9a-08e11405d367.png)


### Clocks Constraints 

Clocks constraints can be given in sdc with some addition information like 
![Fig_6p1_create_clock](https://user-images.githubusercontent.com/84861735/220183330-e24eb7e6-c00c-4308-9384-0b061b425c5f.png)
![Fig_6p2_create_clock](https://user-images.githubusercontent.com/84861735/220183333-711233cf-8c33-449a-847b-79880ba1d6a2.png)
![Fig_6p3_create_clock](https://user-images.githubusercontent.com/84861735/220183335-801351a6-edfa-49e5-916b-09461b60df1a.png)
![Fig_6p4_create_clock](https://user-images.githubusercontent.com/84861735/220183339-5561ef3e-efb7-40a2-b740-8853e7ad732e.png)
![Fig_6p5_create_clock](https://user-images.githubusercontent.com/84861735/220183274-45165aa3-2f9e-48c6-a73e-134ccba7a6fd.png)

**Generated Clocks** are the clocks which are created inside the design which is a divided or multiplied version of parent clock , they can be specified in sdc as shown below :- 
![Fig_7p1_gen_clk](https://user-images.githubusercontent.com/84861735/220183278-be428e09-4699-4f70-bc58-b3a8713f49ac.png)
![Fig_7p2_gen_clk](https://user-images.githubusercontent.com/84861735/220183282-ef2cedb9-0c92-49f3-98f4-8bea93923f5a.png)
![Fig_7p3_gen_clk](https://user-images.githubusercontent.com/84861735/220183285-c4bd03f1-5a6a-42cd-8410-aa0f1e15946f.png)
![Fig_7p4_gen_clk](https://user-images.githubusercontent.com/84861735/220183288-fa54d7ea-91ae-459f-9415-2be72cf8c990.png)
![Fig_7p5_gen_clk](https://user-images.githubusercontent.com/84861735/220183291-afe8493e-fcd9-48e9-9cfd-ebda60169fdb.png)

### Port Delays and Boundary Constrains

Following commands are used to specify boundary constrains

    set_input_delay
    set_output delay

![Fig_8_BC1](https://user-images.githubusercontent.com/84861735/220697820-ee512236-a187-4595-b8fb-be296a8d3a4b.png)
![Fig_8_BC2](https://user-images.githubusercontent.com/84861735/220183293-b6c8c811-ccea-426d-b07e-c99cda9bf33b.png)
![Fig_8_BC3](https://user-images.githubusercontent.com/84861735/220183296-7f561f9d-61b1-4618-9783-0e9ea6f8d5a1.png)


# Day 2

### Other Timing Checks
Apart from Hold and Setup checks(which happens in data pins with respect to clocks) STA also dose other types of check like 
- Clock Gating Checks : These types of cells are added to shutoff clock when not required hence helps in saving power . Enable w.r.t clock is checked by STA tool
- Async Pin Checks : Checks w.r.t reset , set or clear pin w.r.t clocks 
- Data to Data Checks : STA tools checks skew between them 
![D2_Fig_1p1](https://user-images.githubusercontent.com/84861735/220701417-73499317-1296-427f-9b2f-56ba5304091d.png)

### Design Rule Checks
Design Rule Checks specifies about
1. Slew/Transition Analysis

![D2_Fig_1p2](https://user-images.githubusercontent.com/84861735/220701478-cf9d959f-6747-4ebc-9e08-852f3c4443f5.png)

2. Load Analysis
- These are  minimum and maximum capacitance on ports and nets.
- Also specify the maximum fanout load on ports and output pins.
    
3. Clock Skew Analysis
- It is difference in delay of the clock at different points.
- It is basically the delay between the launch clock and capture clock.
- Skew is said to be positive when capture clock has more delay

![D2_Fig_2p1_pos_skew](https://user-images.githubusercontent.com/84861735/220701484-c9ff1b41-9084-4dbc-a1c3-9edfe23c3965.png)

- Skew is said to be negative when capture clock has less delay

![D2_Fig_2p2_neg_skew](https://user-images.githubusercontent.com/84861735/220701487-bd260623-a5e3-42ea-b2fe-8e9b018d6bde.png)

4. Pulse Width Checks
- As clock travels through the path the clock pulse can change(can happen when rise delay and fall delay are not same) called as Shrinked Clock.
- If this shrinked clock is below the certain value then pulse width violation happens

![D2_Fig_3_PWC](https://user-images.githubusercontent.com/84861735/220701490-9c522a48-1c27-4cb1-bc84-e2eb6cb68bde.png)

### Latch Timing

Flop based design launch & capture data on clock edges whereas latch can start sampling data from the rising edge(or falling edge) itself and continue sampling till the respective falling edge (or rising edge) because latches are level sensitive . Hence latch based design are good for timing because has drawback when it comes to DFT 

![D2_Latch_ff](https://user-images.githubusercontent.com/84861735/220705673-e1adcec2-9712-467f-9d43-7117b1958563.png)

#### Time Borrowing
Time borrowing is the property of a latch by virtue of which a path ending at a latch can borrow time from the next path in pipeline such that the overall time of the two paths remains the same. The time borrowed by the latch from next stage in pipeline is, then, subtracted from the next path's time

![D2_Fig_4p3](https://user-images.githubusercontent.com/84861735/220701502-b38ad761-b104-4de5-8986-40da9c5e0030.png)

### STA Text Report
STA tool when does the analysis , its first converts that logic into nodes and cells and archs as shown below :- 

![D2_Fig_5p1](https://user-images.githubusercontent.com/84861735/220701507-6a199f28-9f8f-46dd-b0f0-8549f22b6799.png)

#### Sample STA Text Report

![D2_Fig_5p2](https://user-images.githubusercontent.com/84861735/220701508-b99282c7-27be-4364-8997-872cf6e2cf84.png)


# Day 3 

### Multiple Clocks
When different types of clock are present(of different frequency) then setup check is calculated or the most restrictive setup is identified by 
-First expanding a clock by a common base period 
-Most closest launch and capture which is used find the most restrictive setup. 
-The hold check is dependent upon the modified setup check

Setup Check : 

![D3_1](https://user-images.githubusercontent.com/84861735/220715620-4e790cc4-49b3-4547-b8e6-00c8e9317501.png)

Hold Check : 

![D3_2](https://user-images.githubusercontent.com/84861735/220715695-dd13cc16-9701-4015-9204-4d2fccb5d4ee.png)

There are two rules of hold check
1) Data launched by setup launch edge must not be captured by previous capture edge 
2) Data launched next launched edge must not be captured by setup captured edge 

![D3_3](https://user-images.githubusercontent.com/84861735/220715704-1e395c92-07fa-46ed-b55f-cca85fc2a24b.png)


### Types of Timing Arch

Following are the different types of archs :- 

1) Cell Arch - It is the input to output connection in the cell present in logical libraries . These arch will have information about the input-output functionality, rise and fall time, delays, etc.

2) Net Arch - It connects one cell to other cell and there are ways to calculate the information about the cell to cell rise and fall time, delays, etc.

![D3_5](https://user-images.githubusercontent.com/84861735/220715760-ffb416f4-046c-4718-b9e0-6bd7abf53669.png)

3) Combinational Arch - it does not contain any state elements 

![D3_6](https://user-images.githubusercontent.com/84861735/220715916-3bdb12e2-3f60-4c36-826e-112f837a74a8.png)

4) Sequetial Arch - These are related to clocks like our setup and hold timing. There are setup and hold arch which tells us about the setup and hold time requirement of the library. It has delay arch defined in the logical library.

![D3_7](https://user-images.githubusercontent.com/84861735/220715943-9eb7e659-15f9-49a7-98f6-f8d0fe56bdbe.png)


5) Positive Unate Arch - Positive Unate Arch follows the input in same direction. It basically tells us that when input rises the output also rises or when input falls the output also going to fall
6) Negative Unate Arch - Negative Unate Arch follows the input in opposite direction. It basically tells us that when input rises the output falls or when input rises the output also going to fall.
7) Non Unate Arch - In Non Unate Arch output cant be predicted the output when input changes.

![D3_4](https://user-images.githubusercontent.com/84861735/220715739-6d6e39eb-1d1b-479d-bb67-8976d41bdf3f.png)


### Cell Delays & Arch 
Cell Arch have these Cell Delays

![D3_8](https://user-images.githubusercontent.com/84861735/220715960-84a44c35-841f-403c-8df8-543e0f3e1c9e.png)

### Clock Skew
- Clock Skew is the time difference between arrival of the same edge of a clock signal at the Clock pin of the capture flop and launch flop.
- It is basically the delay difference between the two clocks.

![D9](https://user-images.githubusercontent.com/84861735/220716010-75c603a7-a620-41a1-a6d8-00decdc8f0dd.png)

### Clock Latency
The time taken by Clock signal to reach from clock source to the clock pin of a particular flip flop is called as Clock latency.

![D3_Fig_3p4](https://user-images.githubusercontent.com/84861735/220716053-0e3fb8bf-0784-4f7c-9d3c-f87b5fbdc7f7.png)

### Clock Jitter
Clock jitter is a characteristic of the clock source and the clock signal environment. It can be defined as “deviation of a clock edge from its ideal location.” Clock jitter is typically caused by clock generator circuitry, noise, power supply variations, interference from nearby circuitry etc.

![D3_Fig_3p5](https://user-images.githubusercontent.com/84861735/220716079-610baa29-e3a9-4385-ae3f-e305a5911d27.png)


### Setup Check with Clock Skew & Clock Jitter 

    Tcq + Tcombo + Tsetup <= Tperiod + Tskew - Su 

![D3_Fig_4p1](https://user-images.githubusercontent.com/84861735/220716100-b8c8bc58-c4e6-48bf-98ac-295c159f95bc.png)

### Hold Check with Clock Skew & Clock Jitter 

    Tc2q + Tcombo >= Thold + Tskew + Hu

![D3_Fig_4p2](https://user-images.githubusercontent.com/84861735/220716126-01253ad1-e055-4ce9-bb72-1ee8455d8ab9.png)

### Different Delay Values on Path - Setup Check

-A path can have different delay 
-STA to stay pessimistic SETUP , uses max delay path
-Launch data as late as possible, CLKL -> late 
-Captures data as early as possible, CLKC -> early 

![D3_Fig_4p3](https://user-images.githubusercontent.com/84861735/220716186-ab090523-79d8-4471-884c-45afd97750d0.png)

### Different Delay Values on Path - Hold Check 

-A path can have different delay 
-STA to stay pessimistic HOLD , uses min delay path
-Launch data as early as possible, CLKL -> early 
-Captures data as late as possible, CLKC -> late

![D3_Fig_4p4](https://user-images.githubusercontent.com/84861735/220716210-e6f7cd0d-df03-4bd0-8ae2-406f5049fe38.png)

### CRPR-Clock Reconvergence Pessimism Removal

![D3_Fig_4p5](https://user-images.githubusercontent.com/84861735/220716237-71eb94f3-0c17-4136-97f5-bd133c0e1d07.png)

- During decision of max and min launch path sometimes there is confussion in choosing the best path because some part of the circuit is common in two paths and that can be soled by CRPF
- Clock reconvergence pessimism (CRP) is a delay difference between the launching and capturing clock pathways. The most prevalent causes of CRP are convergent pathways in the clock network and varying minimum and maximum delays of clock network cells.


# Day 4 

### Crosstalk and Noise
Crosstalk is any phenomenon in electronics that occurs when a signal carried on one circuit or channel of a transmission system causes an undesirable effect in another circuit or channel

-In this when both aggressor is changing from 0 to 1 and victim is also changing in the same direction so it can couse the victim signal to change faster, hence rise time changes and delay decreses
-If the aggressor and victim are changing in opposite direction then because of coupling caps aggressor will try to push the victim signal in same direction, hence chage becomes slower in victim which in increases delay
-STA take this changes into consideration and accordingly calculate setup and hold time




When aggressor is changing whereas victim signal in not changing so we can have glich in the victim signal which could lead to functional failure. So STA need to make sure these glitches are not present or should be below a threshold value.

### Process Variation

Sometimes we have process variation(can have differnt delay or transition time)within a chip or from wafer to wafer or within the wafer. So STA need to take this into account.



### Clock Gating Checks
A clock gating check occurs when a gating signal can control the path of a clock signal at a logic cell. For example when a clock and enable signal is fedded as input to an AND gate.
The signal must be used as clock downstream, like feed a flop or latch clock pin or feed output port or feed generated clock.
Intention of this check is that transition on gating pin does not Create unnecessary active edge of the clock in the fanout.




So in case of AND and NAND gates clock gating checks that during setup check the enable should become stable sometime before the clock rising edge(so that coack has enough setup time) and during hold check the enable should become stable sometime after the clock rising edge



So in case of OR or NOR gates clock gating checks that before some time and after some time of the low value of the clock enable pin should be stable.
Sometimes tools cannot set this clock gating checks and gives error, so we need to do it manually by suing this commands

    set_clock_gating_check
    
### Checks on Async Pins

Async Pins in the designes are reset or clear pins
These checks are needed only of asynchronous pins not for synchronous pins(beacure synchronous reset pins come from flops which is alraedy synchronised )

Assertion means clear is on and outputs are zero, which is independent  of clock
De-assertion means clear is off and output is dependent on input and clock.



![D4_Fig_1p1](https://user-images.githubusercontent.com/84861735/220728296-09dc5ac1-15a8-40c5-b9b7-83fcb5d5c0b6.png)
-bd133c0e1d07.png)
![D4_Fig_1p2](https://user-images.githubusercontent.com/84861735/220728324-86723ba9-4dc3-42c0-8939-21f4680c599a.png)
![D4_Fig_2p1](https://user-images.githubusercontent.com/84861735/220728347-403e1c3d-d09e-4907-981b-ff5458eabc4d.png)
![D4_Fig_2p2](https://user-images.githubusercontent.com/84861735/220728385-175301ee-1c9c-499d-9420-aa0459c3d83d.png)
![D4_Fig_3p1](https://user-images.githubusercontent.com/84861735/220728429-d9dfe000-4147-476f-9e38-c423cbde4f2c.png)
![D4_Fig_3p2](https://user-images.githubusercontent.com/84861735/220728455-6d6bedef-6b68-4a9a-b633-fad023cab9eb.png)
![D4_Fig_3p3](https://user-images.githubusercontent.com/84861735/220728474-8c239c3b-c1cb-49d4-a9f7-75b53c7223a8.png)
![D4_Fig_3p4](https://user-images.githubusercontent.com/84861735/220728488-84df00e2-9e73-44d0-b2ff-fa17db8595a7.png)
![D4_Fig_4p1](https://user-images.githubusercontent.com/84861735/220728497-bc0e1ab3-7814-4e22-969a-0722afae7d81.png)
![D4_Fig_4p2](https://user-images.githubusercontent.com/84861735/220728519-2e4246a7-95ba-4046-a537-6a8a908d2358.png)
![D4_Fig_4p3](https://user-images.githubusercontent.com/84861735/220728537-5117e8ca-0c93-44b1-aa63-3cf175209cb1.png)




# Reference 

https://vsdiat.com/

# Acknowledgement

Kunal Ghosh, Co-founder of VLSI System Design (VSD) Corp. Pvt. Ltd.

Vikas Sachdeva, Advisor, Tech and VLSI Coach, Trainer and Innovator at vlsideepdive.

