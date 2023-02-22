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


* [Appendix](#Appendix)
* [References](#references)
* [Acknowledgement](#acknowledgement)
* [Inquiries](#inquiries)


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
- If this shrinked clock is below the certain value then pulse width violation happens.
![D2_Fig_3_PWC](https://user-images.githubusercontent.com/84861735/220701490-9c522a48-1c27-4cb1-bc84-e2eb6cb68bde.png)

### Latch Timing

Flop based design launch & capture data on clock edges whereas latch can start sampling data from the rising edge(or falling edge) itself and continue sampling till the respective falling edge (or rising edge) because latches are level sensitive . Hence latch based design are good for timing because has drawback when it comes to DFT 

![D2_Latch_ff](https://user-images.githubusercontent.com/84861735/220705673-e1adcec2-9712-467f-9d43-7117b1958563.png)

#### Time Borrowing
Time borrowing is the property of a latch by virtue of which a path ending at a latch can borrow time from the next path in pipeline such that the overall time of the two paths remains the same. The time borrowed by the latch from next stage in pipeline is, then, subtracted from the next path's time.
![D2_Fig_4p3](https://user-images.githubusercontent.com/84861735/220701502-b38ad761-b104-4de5-8986-40da9c5e0030.png)

### STA Text Report
STA tool when does the analysis , its first converts that logic into nodes and cells and archs as shown below :- 
![D2_Fig_5p1](https://user-images.githubusercontent.com/84861735/220701507-6a199f28-9f8f-46dd-b0f0-8549f22b6799.png)

#### Sample STA Text Report
![D2_Fig_5p2](https://user-images.githubusercontent.com/84861735/220701508-b99282c7-27be-4364-8997-872cf6e2cf84.png)



