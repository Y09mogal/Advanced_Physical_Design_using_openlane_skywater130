# Advanced_Physical_Design_usign_openlane_skywater130


## Day - 1 

<details>
<summary>Inception of Open-Source EDA, OpenLane and Sky130 PDK</summary>

<br />

Computers with hardware based on the RISC-V core can communicate with one another using the RISC-V Instruction Set Architecture (ISA). The compiler must translate the matching C/C++/Java programme into assembly language instructions in order for the user to be able to run a particular application software on a machine. The compiler's output is device dependant. These instructions are fed into the assembler, which produces binary code (0's and 1's) that the hardware logic in the chip layout can understand. The digital logic, which consists of gates, performs the function specified by the application software's user based on the bits received.

<br />

![day1-1](https://github.com/Y09mogal/Advanced_Physical_Design_using_openlane_skywater130/assets/79003694/ba869158-643d-42aa-b54e-863997cd9a44)

<br />

![day1-2](https://github.com/Y09mogal/Advanced_Physical_Design_using_openlane_skywater130/assets/79003694/0bc8d118-9f24-4d7b-88dd-dd5e2a6fd7ab)

<br />

</details>

<details><summary>SOC Design And OpenLane</summary>

### Components Of Digital Asic Design
Following are the components Of Digital Asic Design:

- EDA Tools: Open-source ASIC design typically relies on Electronic Design Automation (EDA) tools, which include tools for schematic capture, digital logic design, layout design, and simulation. Popular open-source EDA tools include Qflow, Magic, and OpenROAD.
- RTL : RTL IPs offer several advantages. They boost productivity, help bring products to market faster, and make designs more reliable. By using RTL IPs, designers can tap into well-tested and optimized components, reducing the chances of errors. Plus, they promote the reuse of designs, allowing engineers to mix and match different blocks to create more complex systems. In essence, RTL IPs are like a shortcut to building sophisticated digital circuits.
- PDK: An Open Source Process Design Kit (PDK) is essential to the production of semiconductors because it gives designers of integrated circuits the knowledge and resources they need. Open source PDKs, which aim to democratise access to semiconductor manufacturing techniques and promote creativity in chip design, are a relatively recent invention.
  <br>

  ### Simplified RTL to GSDII Flow
  The flow is as follows: 
 - RTL Design: Use a hardware description language (HDL) like Verilog or VHDL to create or import the RTL (Register Transfer Level) design for your digital circuit.
  - Synthesis: Use the open-source synthesis tool Yosys to transform the RTL code to a gate-level netlist. To generate a logical representation of the design, Yosys performs technology mapping and optimisation.
  - Floorplanning: To assign room for various blocks and components inside the chip's layout, OpenLANE performs floorplanning. This stage aids in the optimisation of area utilisation and the management of interconnections.
- Placement: In the following stage, standard cells are positioned in the correct spots on the chip. RePlAce, a tool for precise placement, is used by OpenLANE for this.
  - Clock Tree Synthesis (CTS): OpenLANE has a CTS tool to build a clock distribution network that limits clock skew and provides synchronous clock signals throughout the chip.
  - Routing: To establish connections between the installed standard cells and build the metal interconnects, OpenLANE uses the FastRoute router.
  - GDS2 Final Sign-Off: Verify that the GDSII file satisfies all design and manufacturing specifications by performing a final sign-off. This stage guarantees that the layout is ready for photomask creation and submission to the foundry.
  - GDSII Generation: Generate the GDSII file, which contains the final geometric data for all layers of the chip. This file is used in the fabrication process.
  
<br />

![day1-3](https://github.com/Y09mogal/Advanced_Physical_Design_using_openlane_skywater130/assets/79003694/5760688e-631c-4581-a3ed-2a8cebff4dd4)

<br />
  
### OpenLane
The development of custom chips has been revolutionised by the ground-breaking open-source ASIC (Application-Specific Integrated Circuit) design methodology known as OpenLane. OpenLane, created under the auspices of Google's SkyWater PDK project, marks a paradigm leap in the realm of integrated circuit design. This powerful tool automates and streamlines the entire ASIC design process, from RTL (Register Transfer Level) design to GDSII file production, allowing it to be accessible to a broader audience while drastically lowering design cycle times.

The open-source aspect of OpenLane, which encourages cooperation and openness among those involved in hardware design, is one of its distinguishing characteristics. It combines a wide range of open-source Electronic Design Automation (EDA) technologies, including as synthesis, placement, and routing tools, into a unified workflow. This automation not only accelerates chip development but also reduces the likelihood of human errors, ensuring higher-quality designs.

<br />

![Day1-4](https://github.com/Y09mogal/Advanced_Physical_Design_using_openlane_skywater130/assets/79003694/c13b6abd-60ab-487e-9906-e87965615781)

<br />

OpenLane flow consists of several stages. By default all flow steps are run in sequence. Each stage may consist of multiple sub-stages. OpenLane can also be run interactively as shown [here][25].

1. **Synthesis**
    1. `yosys` - Performs RTL synthesis
    2. `abc` - Performs technology mapping
    3. `OpenSTA` - Performs static timing analysis on the resulting netlist to generate timing reports
2. **Floorplan and PDN**
    1. `init_fp` - Defines the core area for the macro as well as the rows (used for placement) and the tracks (used for routing)
    2. `ioplacer` - Places the macro input and output ports
    3. `pdn` - Generates the power distribution network
    4. `tapcell` - Inserts welltap and decap cells in the floorplan
3. **Placement**
    1. `RePLace` - Performs global placement
    2. `Resizer` - Performs optional optimizations on the design
    3. `OpenDP` - Perfroms detailed placement to legalize the globally placed components
4. **CTS**
    1. `TritonCTS` - Synthesizes the clock distribution network (the clock tree)
5. **Routing**
    1. `FastRoute` - Performs global routing to generate a guide file for the detailed router
    2. `CU-GR` - Another option for performing global routing.
    3. `TritonRoute` - Performs detailed routing
    4. `SPEF-Extractor` - Performs SPEF extraction
6. **GDSII Generation**
    1. `Magic` - Streams out the final GDSII layout file from the routed def
    2. `Klayout` - Streams out the final GDSII layout file from the routed def as a back-up
7. **Checks**
    1. `Magic` - Performs DRC Checks & Antenna Checks
    2. `Klayout` - Performs DRC Checks
    3. `Netgen` - Performs LVS Checks
    4. `CVC` - Performs Circuit Validity Checks

</details>

<details><summary>Invoking OpenLane And Run Synthesis</summary>

### OpenLane Installation

**Prior to the installation of the OpenLane install the dependencies and packages using the command shown below:** </br>
``` 
sudo apt-get update
sudo apt-get upgrade
sudo apt install -y build-essential python3 python3-venv python3-pip make git
```

### Installation Of Docker

```
sudo apt install apt-transport-https ca-certificates curl software-properties-common
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /usr/share/keyrings/docker-archive-keyring.gpg

echo "deb [arch=amd64 signed-by=/usr/share/keyrings/docker-archive-keyring.gpg] https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null

sudo apt update
sudo apt install docker-ce docker-ce-cli containerd.io
sudo docker run hello-world

sudo groupadd docker
sudo usermod -aG docker $USER
sudo reboot 


# Check for installation
sudo docker run hello-world
```




**Installation Of OpenLane**

```
cd
git clone https://github.com/The-OpenROAD-Project/OpenLane --recurse-submodules 
cd OpenLane
make
make test
cd /home/tyrionlanni/OpenLane/designs/ci
cp -r * ../
```

For me apparently, with sudo it was working.

**Running Synthesis**

```
cd ~/OpenLane
make mount
./flow.tcl -interactive
package require openlane 0.9
prep -design picorv32a
run_synthesis
```
<br />

![run_synthesis](https://github.com/Y09mogal/Advanced_Physical_Design_using_openlane_skywater130/assets/79003694/2a6595f3-28ab-455f-bba0-af0bed69dafd)

<br />


**To view synthesis report**

```
cd OpenLane/designs/picorv32a/runs/RUN_2023.09.15_12.17.54/reports/synthesis/
gedit 1-synthesis_dff.stat 
```

<br />

![synthesis_netlist](https://github.com/Y09mogal/Advanced_Physical_Design_using_openlane_skywater130/assets/79003694/fa2df870-eff1-4f4b-b09f-80038a4ca062)

<br />

![synthesis_report](https://github.com/Y09mogal/Advanced_Physical_Design_using_openlane_skywater130/assets/79003694/99fc6196-3240-446d-9ece-976307e502ad)

<br />

```
cd OpenLane/designs/picorv32a/runs/RUN_2023.09.15_12.17.54/reports/synthesis/
gedit 1-synthesis.AREA_0.stat.rpt
```

```

61. Printing statistics.

=== picorv32 ===

   Number of wires:               9824
   Number of wire bits:          10206
   Number of public wires:        1512
   Number of public wire bits:    1894
   Number of memories:               0
   Number of memory bits:            0
   Number of processes:              0
   Number of cells:              10104
     sky130_fd_sc_hd__a2111o_2       2
     sky130_fd_sc_hd__a211o_2      101
     sky130_fd_sc_hd__a211oi_2       4
     sky130_fd_sc_hd__a21bo_2       19
     sky130_fd_sc_hd__a21boi_2       7
     sky130_fd_sc_hd__a21o_2       414
     sky130_fd_sc_hd__a21oi_2      127
     sky130_fd_sc_hd__a221o_2       65
     sky130_fd_sc_hd__a221oi_2       1
     sky130_fd_sc_hd__a22o_2       197
     sky130_fd_sc_hd__a22oi_2        2
     sky130_fd_sc_hd__a2bb2o_2      16
     sky130_fd_sc_hd__a311o_2       38
     sky130_fd_sc_hd__a31o_2        90
     sky130_fd_sc_hd__a31oi_2       10
     sky130_fd_sc_hd__a32o_2        89
     sky130_fd_sc_hd__a41o_2         2
     sky130_fd_sc_hd__and2_2       283
     sky130_fd_sc_hd__and2b_2       32
     sky130_fd_sc_hd__and3_2        77
     sky130_fd_sc_hd__and3b_2       76
     sky130_fd_sc_hd__and4_2        46
     sky130_fd_sc_hd__and4b_2        6
     sky130_fd_sc_hd__and4bb_2       3
     sky130_fd_sc_hd__buf_1       2735
     sky130_fd_sc_hd__buf_2         16
     sky130_fd_sc_hd__conb_1       106
     sky130_fd_sc_hd__dfxtp_2     1596
     sky130_fd_sc_hd__inv_2         83
     sky130_fd_sc_hd__mux2_2      1817
     sky130_fd_sc_hd__mux4_2       323
     sky130_fd_sc_hd__nand2_2      250
     sky130_fd_sc_hd__nand2b_2       2
     sky130_fd_sc_hd__nand3_2       18
     sky130_fd_sc_hd__nand3b_2       3
     sky130_fd_sc_hd__nand4_2        2
     sky130_fd_sc_hd__nor2_2       185
     sky130_fd_sc_hd__nor3_2        11
     sky130_fd_sc_hd__nor3b_2        3
     sky130_fd_sc_hd__nor4_2         4
     sky130_fd_sc_hd__nor4b_2        3
     sky130_fd_sc_hd__o2111a_2       1
     sky130_fd_sc_hd__o211a_2      224
     sky130_fd_sc_hd__o211ai_2       6
     sky130_fd_sc_hd__o21a_2       154
     sky130_fd_sc_hd__o21ai_2       94
     sky130_fd_sc_hd__o21ba_2       15
     sky130_fd_sc_hd__o21bai_2       3
     sky130_fd_sc_hd__o221a_2       19
     sky130_fd_sc_hd__o221ai_2       1
     sky130_fd_sc_hd__o22a_2        26
     sky130_fd_sc_hd__o22ai_2        1
     sky130_fd_sc_hd__o2bb2a_2       7
     sky130_fd_sc_hd__o311a_2       31
     sky130_fd_sc_hd__o311ai_2       2
     sky130_fd_sc_hd__o31a_2        21
     sky130_fd_sc_hd__o31ai_2        2
     sky130_fd_sc_hd__o32a_2        14
     sky130_fd_sc_hd__o41a_2         1
     sky130_fd_sc_hd__or2_2        337
     sky130_fd_sc_hd__or2b_2        20
     sky130_fd_sc_hd__or3_2        102
     sky130_fd_sc_hd__or3b_2        17
     sky130_fd_sc_hd__or4_2         29
     sky130_fd_sc_hd__or4b_2         6
     sky130_fd_sc_hd__xnor2_2       78
     sky130_fd_sc_hd__xor2_2        29

   Chip area for module '\picorv32': 102957.494400
```
```
Flop ratio = Number of D Flip flops = 1596  = 0.1579
             ______________________   _____
             Total Number of cells    10104
```






</details>







</details>

## Day-2 

### Good Floorplan vs Bad Floorplan And Introduction To Library Cells

<details><summary><strong>Chip Floor Planning Considerations</strong></summary>

### Utilization Factor
In the ASIC (Application-Specific Integrated Circuit) design cycle, the Utilisation Factor is a metric that quantifies how efficiently the physical area of the chip is used. It is the ratio of the occupied area on the semiconductor core (the area filled with logic, standard cells, and other components) to the total accessible area.

<br>

Set the utilisation factor to 0.5 or 0.6 to allow for optimisations, routing, inserting buffers, and so on.

### Aspect Ratio
The ratio of the die's height to its breadth is known as the aspect ratio. If the answer is "1," it is assumed that the die is square in shape.

### Pre-placed Cells
In ASIC (Application-Specific Integrated Circuit) design, pre-placed cells (or pre-placed blocks) refer to predetermined and fixed blocks of logic or circuitry that are manually inserted in particular locations on the semiconductor chip's layout before the automated placement and routing process.<br>
Pre-placed cells are positioned precisely on the chip layout and have special functionality built into their design. These cells often perform crucial functions that necessitate precise positioning and connection.

### Decoupling Capacitor 
A constant voltage level at the ICs' power supply pins is kept using decoupling capacitors. The decoupling capacitor provides the necessary charge to maintain the voltage stability when an IC changes or pulls transient current, avoiding glitches or erratic behaviour.

### Power Planning
Assume there are numerous macros in a chip and the output changes from '1' to '0', then it discharges into the ground line, resulting in ground bumpp. When charged from 0 to 1, we may notice voltage droop in the power source.<br>

<br />

![fp1](https://github.com/Y09mogal/Advanced_Physical_Design_using_openlane_skywater130/assets/79003694/d039378e-6391-4f60-855a-9830b8a1f637)

<br />

![fp2](https://github.com/Y09mogal/Advanced_Physical_Design_using_openlane_skywater130/assets/79003694/f8a7714c-74a6-4f8f-ac2f-279a94724b3d)

<br />

![fp22](https://github.com/Y09mogal/Advanced_Physical_Design_using_openlane_skywater130/assets/79003694/7e995c26-ed4b-468d-94e7-6e9b39dd4b6e)

<br />


Hence to resolve this we can have multiple supply line for vdd as well as ground as shown below:

<br />

![fp3](https://github.com/Y09mogal/Advanced_Physical_Design_using_openlane_skywater130/assets/79003694/511be3cb-3a80-4d8d-bc9f-290a1ff2473a)

<br />

### Pin Placement
Let us suppose we have following design:

<br />

![fp4](https://github.com/Y09mogal/Advanced_Physical_Design_using_openlane_skywater130/assets/79003694/e82d6372-c30a-4ba5-a31e-9b1d29fb48bf)

<br />

Now we have to place the pins in the chip as shown below:

<br />

![fp5](https://github.com/Y09mogal/Advanced_Physical_Design_using_openlane_skywater130/assets/79003694/69b6c7c0-08ee-417e-b174-bbb20a2811e0)

<br />

The Clock port are bigger than the normal I/O pins because of it's continuous use and larger area offers less resistance.

<details><summary><strong>Steps To Run Floorplan and Placement</strong></summary>
  
### Floorplanning
Command:

```
run_floorplan
```
<br />

![floorplan-d2](https://github.com/Y09mogal/Advanced_Physical_Design_using_openlane_skywater130/assets/79003694/a2e5cc74-f04c-47fc-a4d1-647178ba2196)

<br />

To view the floorplanning in magic:

```
cd OpenLane/designs/picorv32a/runs/RUN_2023.09.16_19.25.25/results/floorplan/
magic  /home/tyrionlanni/.volare/sky130A/libs.tech/magic/sky130A.tech lef read ../../tmp/merged.nom.lef def read picorv32.def &

```

<br />

![fp-magic](https://github.com/Y09mogal/Advanced_Physical_Design_using_openlane_skywater130/assets/79003694/030de756-c8b0-456a-83a1-c1082fe9a59c)

<br />

![floorplan](https://github.com/Y09mogal/Advanced_Physical_Design_using_openlane_skywater130/assets/79003694/47b93454-7541-4e4c-957d-71ebf0744984)

<br />

### Placement

Command:

```
run_placement
```

<br />

![placement-d2](https://github.com/Y09mogal/Advanced_Physical_Design_using_openlane_skywater130/assets/79003694/1e587e4b-2a90-483c-a5fb-da2b59345d5b)

<br />

To view placement :

```
cd OpenLane/designs/picorv32a/runs/RUN_2023.09.16_19.25.25/results/placement/
magic /home/tyrionlanni/.volare/sky130A/libs.tech/magic/sky130A.tech lef read ../../tmp/merged.nom.lef def read picorv32.def &
```


<br />

![placement-magic](https://github.com/Y09mogal/Advanced_Physical_Design_using_openlane_skywater130/assets/79003694/a8f549e7-162f-4c47-9926-1fc4941d0a0d)

<br />

![placement](https://github.com/Y09mogal/Advanced_Physical_Design_using_openlane_skywater130/assets/79003694/1147bb00-b97e-412c-af16-268636f8398a)

<br />


</details>
</details>

<details><summary><strong>Library Binding And Placement</strong></summary>

### Netlist Binding
A netlist is a critical representation of the electronic components and their interconnections within the chip in ASIC (Application-Specific Integrated Circuit) design. The mapping of logical components expressed in a high-level hardware description language (HDL) like Verilog or VHDL to physical components in the target technology library is a critical step in the ASIC design phase. <br>
Netlist binding is the process of mapping the gate-level netlist to the physical cells in the technology library. To implement each gate in the netlist, the synthesis tool selects individual cells from the library. This mapping is based on a number of parameters, including timing limitations, area constraints, and power concerns.


### Initial Design

<br />

![imgini](https://github.com/Y09mogal/Advanced_Physical_Design_using_openlane_skywater130/assets/79003694/1240e54c-2a89-493d-9aba-3f5f44a09249)
 
<br />

### Final Optimized Design

<br />

![imgfinal](https://github.com/Y09mogal/Advanced_Physical_Design_using_openlane_skywater130/assets/79003694/9fa4b5ce-66f5-4bac-b402-8d4aa035cecc)

<br />

  
</details>






<details><summary>Cell Design And Characterization Flow</summary>

### Cell Design

To build unique digital integrated circuits, a standardised procedure called the standard cell design flow is employed. It consists of multiple important steps, beginning with inputs and ending with varied outputs. The conventional cell design flow is broken down as follows:

**Inputs**:

1. **Process Design Kits (PDKs)**: These are supplied by semiconductor foundries and include critical information regarding the intended manufacturing process, such as standard cell libraries and design standards.

2. **DRC & LVS Rules**: Design Rule Checking (DRC) and Layout vs. Schematic (LVS) rules are guidelines that ensure the design adheres to manufacturing and electrical specifications.

3. **SPICE Models**: These are mathematical representations of electronic components used for simulation and verification.

4. **Libraries**: Standard cell libraries with pre-designed logic gates and flip-flops are crucial building blocks for the design.

5. **User-Defined Specifications**: Design requirements and constraints set by the designer, such as performance targets, power budget, and functionality.

**Design Steps**:

1. **Circuit Design**: Creating the logical representation of the circuit using hardware description languages (HDLs) like Verilog or VHDL, taking into account the provided libraries and user-defined specifications.

2. **Layout Design**: Translating the logical circuit into a physical layout using layout design techniques. This includes considerations like Euler's path and stick diagrams to optimize for area and performance.

3. **Extraction of Parasitics**: Extracting parasitic elements (such as capacitance and resistance) from the layout to refine the circuit's performance simulation.

4. **Characterization**: Evaluating the circuit's behavior under various conditions, including timing analysis (setup and hold times), noise analysis, and power consumption estimation.

<br />

![input](https://github.com/Y09mogal/Advanced_Physical_Design_using_openlane_skywater130/assets/79003694/80a376ea-fd1b-47f0-841c-969cbbc1017d)

<br />

**Outputs**:

1. **Circuit Description Language (CDL)**: A human-readable or machine-readable representation of the circuit, often used for simulation and documentation.

2. **Library Exchange Format (LEF)**: A file format that defines the physical properties of standard cells and facilitates the integration of these cells into the chip's layout.

3. **GDSII**: A file format used to describe the final layout of the chip in a format that can be sent to the semiconductor foundry for manufacturing.

4. **Extracted SPICE Netlist (.cir)**: A netlist that includes parasitic elements extracted from the layout, used for more accurate electrical simulations.

5. **Timing, Noise, and Power .lib Files**: Libraries containing information on the timing characteristics, noise margins, and power consumption of the designed cells, essential for further chip-level analysis and integration.

<br />

![cell-design-flow](https://github.com/Y09mogal/Advanced_Physical_Design_using_openlane_skywater130/assets/79003694/b4e2eeee-2413-4000-b54b-bdc308e6e3c1)

<br />

<br />

![design-step](https://github.com/Y09mogal/Advanced_Physical_Design_using_openlane_skywater130/assets/79003694/452d1b50-4818-4574-a96c-baaba8f97d28)

<br />

### Characterisation Flow

The standard cell characterization flow typically involves a structured series of steps to determine the electrical behavior and performance characteristics of individual standard cells within an ASIC design. This process is essential for accurately modeling the cells' behavior under various conditions. Here's a rephrased breakdown of the typical standard cell characterization flow:

**1. Model and Technology Data Setup**:
   - Import semiconductor process technology files (tech files) and the transistor-level models (usually SPICE models) for the standard cells.

**2. Read Extracted Spice Netlist**:
   - Input the extracted SPICE netlist that represents the specific standard cell to be characterized.

**3. Behavior Recognition**:
   - Identify and understand the expected behavior and functionality of the standard cell being characterized.

**4. Subcircuit Handling**:
   - Handle any subcircuits or hierarchical structures within the cell design, ensuring accurate simulation.

**5. Power Source Attachment**:
   - Connect appropriate power sources to the standard cell to simulate its behavior under different supply voltages and conditions.

**6. Characterization Setup**:
   - Configure the characterization setup, including specifying input stimulus patterns, test vectors, and conditions for the simulations.

**7. Output Load Configuration**:
   - Define and apply the necessary output capacitance loads to simulate the cell's response to different output loading conditions.

**8. Simulation Commands**:
   - Set up and execute simulation commands for the standard cell, which may include transient simulations, DC analyses, or other relevant simulation types.

**9. GUNA Software Integration**:
   - Utilize open-source software like GUNA to automate and streamline the characterization process.
   - Feed the data from steps 1 through 8 into the GUNA software.

**10. Model Generation**:
    - Use the GUNA software to generate comprehensive models for the standard cell, including timing models (setup time, hold time, propagation delay), noise models (noise margins, sensitivity to noise), and power models (static power, dynamic power).

**11. Model Validation**:
    - Verify and validate the generated models against simulation results to ensure accuracy and reliability.

**12. Documentation and Reporting**:
    - Document the generated models and their characteristics for future use in ASIC design.
    - Create reports summarizing the characterization results and models.

By following this standardized flow and using tools like GUNA, designers can efficiently characterize standard cells, which are essential building blocks in ASIC designs. These models are crucial for accurate timing analysis, power estimation, and noise margin assessments in the overall ASIC design process.

<br />

![characterisation-flow](https://github.com/Y09mogal/Advanced_Physical_Design_using_openlane_skywater130/assets/79003694/06eb5554-db75-4132-bf63-6e057b42cac1)

<br />

</details>

<details><summary><strong>Timing Characterization Parameters</strong></summary>
  
#### Timing threshold definitions 
Timing defintion |	Value
-------------- | --------------
slew_low_rise_thr	| 20% value
slew_high_rise_thr | 80% value
slew_low_fall_thr |	20% value
slew_high_fall_thr |	80% value
in_rise_thr	| 50% value
in_fall_thr |	50% value
out_rise_thr |	50% value
out_fall_thr | 50% value

#### Propagation Delay and Transition Time 

**Propagation Delay** 
The time difference between when the transitional input reaches 50% of its final value and when the output reaches 50% of its final value. Poor choice of threshold values lead to negative delay values. Even thought you have taken good threshold values, sometimes depending upon how good or bad the slew, the dealy might be still +ve or -ve.

```
Propagation delay = time(out_thr) - time(in_thr)
```

<br />

![p-delay](https://github.com/Y09mogal/Advanced_Physical_Design_using_openlane_skywater130/assets/79003694/9b58fc93-2e1f-4d64-b183-eaf4f7bcaffa)

<br />

**Transition Time**

The time it takes the signal to move between states is the transition time , where the time is measured between 10% and 90% or 20% to 80% of the signal levels.

```
Rise transition time = time(slew_high_rise_thr) - time (slew_low_rise_thr)

Low transition time = time(slew_high_fall_thr) - time (slew_low_fall_thr)
```

<br />

![p-delay1](https://github.com/Y09mogal/Advanced_Physical_Design_using_openlane_skywater130/assets/79003694/76be9fba-472e-4e1e-91fb-5ddb9902a4ad)

<br />


</details>


## Day-3 

### Design Library Cell using magic layout and ngspice charcterization

<details><summary>CMOS inverter ngspice simulations </summary>

##  Cmos Inverter

<br />

![cmos-inv](https://github.com/Y09mogal/Advanced_Physical_Design_using_openlane_skywater130/assets/79003694/00b1656d-bb72-499a-8d64-01b492372436)

<br />

The term "CMOS inverter" refers to a device that generates logic functions and is a necessary element in all integrated circuits. A CMOS inverter is a FET (field effect transistor) that consists of a metal gate on top of an insulating layer of oxygen on top of a semiconductor.

Both the NMOS and PMOS transistors' gate terminals receive the input signal. The output is derived from the connection point (the NMOS drain and the PMOS source) between these two transistors.
    The n-MOS transistor is ON whereas the P-MOS transistor is OFF when Vin is high and equal to VDD. The analogous circuit shown below has a direct route between Vout and the ground node, resulting in a steady-state value of 0V.

  When the input voltage is 0V, the n-MOS and p-MOS transistors are OFF and ON, respectively. The following equivalent demonstrates the existence of a path between VDD and Vout, resulting in a high output voltage.

  

# Spice Deck

<br />

![spice-deck](https://github.com/Y09mogal/Advanced_Physical_Design_using_openlane_skywater130/assets/79003694/e60c7e24-359d-479e-86e8-b8ac04c95af0)

<br />

Spice deck for the above:

```
*** MODEL Descriptions ***

*** NETLIST Description ***

M1 out in vdd vdd pmos W=0.37u L=0.25u
M2 out in 0 0 nmos W=0.375u L=0.25u

cload out 0 10f

vdd vdd 0 2.5

Vin in 0 2.5

*** SIMULATION Commands ***

.op

.dc Vin 0 2.5 0.05

***.include tsmc_025um_model.mod ***
.LIB "tsmc_025um_model.mod" cmos_models
.end

```

Spice Simulation

<br />

![spice-sim-d3](https://github.com/Y09mogal/Advanced_Physical_Design_using_openlane_skywater130/assets/79003694/1ecea176-ed34-4f98-b29b-1c6903d851ec)

<br />


Model File:
```
* SPICE 3f5 Level 8, Star-HSPICE Level 49, UTMOST Level 8

.lib cmos_models 
* DATE: Feb 23/01
* LOT: T0BM                  WAF: 07
* Temperature_parameters=Default
.MODEL nmos  NMOS (                                LEVEL   = 49
+VERSION = 3.1            TNOM    = 27             TOX     = 5.8E-9
+XJ      = 1E-7           NCH     = 2.3549E17      VTH0    = 0.3907535
+K1      = 0.4376003      K2      = 8.265151E-3    K3      = 4.214601E-3
+K3B     = -3.7220937     W0      = 2.517345E-6    NLX     = 2.310668E-7
+DVT0W   = 0              DVT1W   = 0              DVT2W   = 0
+DVT0    = 0.2411602      DVT1    = 0.3707226      DVT2    = -0.5
+U0      = 316.5922683    UA      = -9.89493E-10   UB      = 2.154013E-18
+UC      = 2.474632E-11   VSAT    = 1.254499E5     A0      = 1.2735648
+AGS     = 0.2428704      B0      = 2.579719E-8    B1      = -1E-7
+KETA    = 4.87168E-4     A1      = 0              A2      = 0.5196633
+RDSW    = 120            PRWG    = 0.5            PRWB    = -0.2
+WR      = 1              WINT    = 2.357855E-8    LINT    = 1.210018E-9
+DWG     = 2.292632E-9
+DWB     = -9.94921E-10   VOFF    = -0.1039771     NFACTOR = 1.3905578
+CIT     = 0              CDSC    = 2.4E-4         CDSCD   = 0
+CDSCB   = 0              ETA0    = 3.894977E-3    ETAB    = 7.800632E-4
+DSUB    = 0.0307944      PCLM    = 1.7312397      PDIBLC1 = 0.999135
+PDIBLC2 = 4.850036E-3    PDIBLCB = -0.0866866     DROUT   = 0.8612131
+PSCBE1  = 7.995844E10    PSCBE2  = 1.457011E-8    PVAG    = 0.0099984
+DELTA   = 0.01           RSH     = 5              MOBMOD  = 1
+PRT     = 0              UTE     = -1.5           KT1     = -0.11
+KT1L    = 0              KT2     = 0.022          UA1     = 4.31E-9
+UB1     = -7.61E-18      UC1     = -5.6E-11       AT      = 3.3E4
+WL      = 0              WLN     = 1              WW      = -1.22182E-16
+WWN     = 1.2127         WWL     = 0              LL      = 0
+LLN     = 1              LW      = 0              LWN     = 1
+LWL     = 0              CAPMOD  = 2              XPART   = 0.4
+CGDO    = 3.11E-10       CGSO    = 3.11E-10       CGBO    = 1E-12
+CJ      = 1.741905E-3    PB      = 0.9876681      MJ      = 0.4679558
+CJSW    = 3.653429E-10   PBSW    = 0.99           MJSW    = 0.2943558
+CF      = 0              PVTH0   = -0.01          PRDSW   = 0
+PK2     = 2.589681E-3    WKETA   = -1.866069E-3   LKETA   = -0.0166961      )
*
.MODEL pmos  PMOS (                                LEVEL   = 49
+VERSION = 3.1            TNOM    = 27             TOX     = 5.8E-9
+XJ      = 1E-7           NCH     = 4.1589E17      VTH0    = -0.583228
+K1      = 0.5999865      K2      = 6.150203E-3    K3      = 0
+K3B     = 3.6314079      W0      = 1E-6           NLX     = 1E-9
+DVT0W   = 0              DVT1W   = 0              DVT2W   = 0
+DVT0    = 2.8749516      DVT1    = 0.7488605      DVT2    = -0.0917408
+U0      = 136.076212     UA      = 2.023988E-9    UB      = 1E-21
+UC      = -9.26638E-11   VSAT    = 2E5            A0      = 0.951197
+AGS     = 0.20963        B0      = 1.345599E-6    B1      = 5E-6
+KETA    = 0.0114727      A1      = 3.851541E-4    A2      = 0.614676
+RDSW    = 1.496983E3     PRWG    = -0.0440632     PRWB    = -0.2945454
+WR      = 1              WINT    = 7.879211E-9    LINT    = 2.894523E-8
+DWG     = -1.112097E-8
+DWB     = 9.815716E-9    VOFF    = -0.1204623     NFACTOR = 1.2259401
+CIT     = 0              CDSC    = 2.4E-4         CDSCD   = 0
+CDSCB   = 0              ETA0    = 0.3325261      ETAB    = -0.0623452
+DSUB    = 0.9206875      PCLM    = 0.833903       PDIBLC1 = 9.948506E-4
+PDIBLC2 = 0.0191187      PDIBLCB = -1E-3          DROUT   = 0.9938581
+PSCBE1  = 2.887413E10    PSCBE2  = 8.325891E-9    PVAG    = 0.8478443
+DELTA   = 0.01           RSH     = 3.6            MOBMOD  = 1
+PRT     = 0              UTE     = -1.5           KT1     = -0.11
+KT1L    = 0              KT2     = 0.022          UA1     = 4.31E-9
+UB1     = -7.61E-18      UC1     = -5.6E-11       AT      = 3.3E4
+WL      = 0              WLN     = 1              WW      = 0
+WWN     = 1              WWL     = 0              LL      = 0
+LLN     = 1              LW      = 0              LWN     = 1
+LWL     = 0              CAPMOD  = 2              XPART   = 0.4
+CGDO    = 2.68E-10       CGSO    = 2.68E-10       CGBO    = 1E-12
+CJ      = 1.864957E-3    PB      = 0.976468       MJ      = 0.4614408
+CJSW    = 3.118281E-10   PBSW    = 0.6870843      MJSW    = 0.3021929
+CF      = 0              PVTH0   = 6.397941E-3    PRDSW   = 30.410214
+PK2     = 2.100359E-3    WKETA   = 5.428923E-3    LKETA   = -0.0111599      )
*
.endl
```
Commands to open ngspice and run the simulation:
```
ngspice
source Cmos.cir
```
To execute it:
```
run
setplot
display
```
we can set the dc plot

<br />

![Screenshot from 2023-10-02 14-55-49](https://github.com/Y09mogal/Advanced_Physical_Design_using_openlane_skywater130/assets/79003694/fd3b3794-6c6d-4d8a-9ead-a7651c91b8ca)

<br />

# Switching threshold
The instance where Vin = Vout (both PMOS and. NMOS in saturation since VDS = VGS), If VM = VDD/2, this indicates symmetric rise/fall behaviour for the CMOS gate (both PMOS and NMOS in saturation because VDS = VGS).This precise threshold causes both the PMOS and NMOS transistors to be active, which might result in the development of a leakage current.

<br />

![Switching-threshold](https://github.com/Y09mogal/Advanced_Physical_Design_using_openlane_skywater130/assets/79003694/7fe19216-1ea9-419b-8106-e97669a5e5cf)

<br />

Below shown switching threshold representation where Wp/Lp and xWn/Ln relation and calculation shown:

<br />

![img](https://github.com/Y09mogal/Advanced_Physical_Design_using_openlane_skywater130/assets/79003694/b089677f-bb42-4596-8422-590f0906572b)

<br />

Modified Cmos file:

```
*** MODEL Descriptions ***

*** NETLIST Description ***

M1 out in vdd vdd pmos W=0.375u L=0.25u
M2 out in 0 0 nmos W=0.375u L=0.25u

cload out 0 10f

vdd vdd 0 2.5

Vin in 0 0 pulse 0 2.5 0 10p 10p 1n 2n

*** SIMULATION Commands ***


.tran 10p 4n


***.include tsmc_025um_model.mod ***
.LIB "tsmc_025um_model.mod" cmos_models
.end
```




</details>
<details><summary>Inception Of Layout </summary>

## CMOS Fabrication

- Substrate Selection: Select the material for the chip's body or substrate.The substrate is the fundamental substance onto which the complete integrated circuit will be fabricated.

- Active Region Isolation: Create active regions for transistors by deposition, photolithography, and etching SiO2 and Si3N4 layers.

- N-Well and P-Well Formation: To form N-type and P-type regions, use ion implantation with Boron for P-well formation and Phosphorous for N-well formation.

- Gate Terminal Formation: Using photolithography techniques, create NMOS and PMOS gate terminals.

- LDD (Lightly Doped Drain) Formation: To avoid the hot electron effect, create LDD sections with light doping.

- Source and Drain Formation: Use screen oxide, Arsenic implantation, and annealing to prepare the source and drain regions.

- Local Interconnect Formation: Using HF etching, remove screen oxide and layer low-resistance Titanium (Ti) for contacts.

- Higher-Level Metal Formation: Planarization by CMP, followed by TiN and Tungsten deposition. The top SiN layer protects the chip.


# Inverter Standard cell Layout & SPICE extraction
To see the magic layout of the CMOS inverter we'll get the magic file from  [vsdstdcelldesign](https://github.com/nickson-jose/vsdstdcelldesign)  by cloning it within Openlane directory

```
git clone https://github.com/nickson-jose/vsdstdcelldesign
```
It will create a folder named vsdstdcelldesign in Openlane directory.
Now run the following command to view the sky130_inv.mag file. First, we must ensure that the sky130A.tech file is also in the same directory.
```
magic -T sky130A.tech sky130_inv.mag &
```

<br />

![layout](https://github.com/Y09mogal/Advanced_Physical_Design_using_openlane_skywater130/assets/79003694/96d18e42-49eb-4988-8d0b-2ee4490a0ec2)

<br />

#  Identification of NMOS and PMOS:

## PMOS

<br />

![pmos](https://github.com/Y09mogal/Advanced_Physical_Design_using_openlane_skywater130/assets/79003694/10c901ba-5759-4b11-87a7-b8129f11e767)

<br />

## NMOS

<br />

![nmos](https://github.com/Y09mogal/Advanced_Physical_Design_using_openlane_skywater130/assets/79003694/74aa2cb9-4976-41ec-9b3d-52803872a2b4)

<br />


# Connectivity of Source and Drain:

<br />

![drain-source-connectivity](https://github.com/Y09mogal/Advanced_Physical_Design_using_openlane_skywater130/assets/79003694/fa40c297-ab00-4d8b-9452-09180dff862d)

<br />

- P-Diffusion and N-Diffusion Regions: Examine the layout for P-diffusion and N-diffusion regions relative to the polysilicon layers. These are the active areas of the PMOS and NMOS transistors in the CMOS inverter.

- Drain and Source Connections: Make sure that the drains of both PMOS and NMOS transistors are connected to the output port (marked as Y) and the sources of both transistors are linked to the power supply VDD (commonly denoted as VPWR).

- LEF (Library Exchange Format): LEF is an electronic design automation (EDA) format that contains information on cell borders, VDD and GND (ground) lines, pin placements, and other physical aspects of integrated circuit libraries. It contains no information about the circuit's logic or functionality and is frequently employed to safeguard intellectual property (IP).

- SPICE Extraction: SPICE (Simulation Programme with Integrated Circuit Emphasis) extraction is the technique of obtaining electrical parameters from a physical layout (such as.mag format) in order to generate a SPICE netlist. This netlist is used to simulate and analyse circuits. 


## Steps To Create Standard Cell and Extract Spice Netlist
### Commands
```
extract all
ext2spice cthresh 0 rthresh 0
ext2spice
```

Following spice file is created:

<br />

![Screenshot from 2023-10-02 15-08-33](https://github.com/Y09mogal/Advanced_Physical_Design_using_openlane_skywater130/assets/79003694/ccaf90e7-09b3-4448-8284-ebf5566ea45c)

<br />

```
* SPICE3 file created from sky130_inv.ext - technology: sky130A

.option scale=10000u

.subckt sky130_inv A Y VPWR VGND
M1000 Y A VPWR VPWR pshort w=37 l=23
+  ad=1443 pd=152 as=1517 ps=156
M1001 Y A VGND VGND nshort w=35 l=23
+  ad=1435 pd=152 as=1365 ps=148
C0 A Y 0.05fF
C1 VPWR Y 0.11fF
C2 A VPWR 0.07fF
C3 Y 0 0.24fF
C4 VPWR 0 0.59fF
.ends
```




</details>

<details><summary>Sky130 Tech File Labs</summary>
After Extracting the spice netlist, we will modify the netlist by adding the following :

  ```
VDD VPWR 0 3.3V
VSS VGND 0 0
Va A VGND PUSLE(0V 3.3V 0 0.1ns 0.1 ns 2ns 4ns)
.tran 1n 20n
.control
run 
.endc
.end
```

The "sky130_in.spice" file is created and then modified to include the "pshort.lib" and "nshort.lib" libraries, which are created specifically for PMOS and NMOS components. Furthermore, the minimum grid size of the inverter is computed based on the Magic configuration and incorporated into the deck using the command ".option scale=0.01u". The model names within the MOSFET definitions are changed to "pshort.model.0" for PMOS and "nshort.model.0" for NMOS to ensure consistency.

The final sky130A_inv.spice file modified to:

```
* SPICE3 file created from sky130_inv.ext - technology: sky130A

.option scale=0.01u
.include ./libs/pshort.lib
.include ./libs/nshort.lib
//.subckt sky130_inv A Y VPWR VGND
M1000 Y A VPWR VPWR pshort_model.0 w=37 l=23 
+  ad=1.44n pd=0 as=1.51n ps=0.156m
M1001 Y A VGND VGND nshort_model.0 w=35 l=23 
+  ad=1.44n pd=0.152m as=1.37n ps=0.148m

VDD VPWR 0 3.3V
VSS VGND 0 0V
Va A VGND PULSE(0V 3.3V 0 0.1ns 0.1ns  2ns 4ns)

C0 A Y 0.05fF
C1 VPWR Y 0.11fF
C2 A VPWR 0.07fF
C3 Y 0 0.24fF
C4 VPWR 0 0.59fF
C5 VPWR VGND 0.781f
//.ends
.tran 1n 20n
.control
run 
.endc
.end
```

For simulation, ngspice is invoked in the terminal:

```
ngspice sky130_inv.spice
```
The output "y" is to be plotted with "time" and swept over the input "a":
```
plot y vs time a
```
<br />

![ngspice_inv-skywater130](https://github.com/Y09mogal/Advanced_Physical_Design_using_openlane_skywater130/assets/79003694/7b0c2bf4-b476-4b87-bc03-cdb375dc7efc)

<br />

### Output Waveform:

<br />

![plot](https://github.com/Y09mogal/Advanced_Physical_Design_using_openlane_skywater130/assets/79003694/974dab67-d52a-4a60-bad7-47ec58552427)

<br />

The following timing parameters are used to characterise the inverter standard cell:

- Rise Transition: This is the time it takes for the output to transition from 20% to 80% of its maximum value.

- Fall Transition: This parameter indicates the amount of time it takes for the output to change from 80% to 20% of its maximum value.

- Cell climb Delay: This is determined as the time it takes for the output to reach 50% of its maximum climb from its minimum value minus the time it takes for the input to fall by 50%.

- Cell Fall Delay: This is calculated by subtracting the time when the output drops to 50% of its maximum value from the time when the input rises by 50%.

In digital circuit design, these parameters are critical for understanding the timing behaviour of the inverter standard cell.

The time parameters listed above can be calculated by taking notes on various values from the ngspice waveform.


```
Rise Transition : 2.25182 - 2.19362 = 0.0582 ns / 58.20ps
Fall Transitio : 4.10413 - 4.0631 = 0.04103ns/41.03ps
Cell Rise Delay : 2.21701 - 2.15989 = 0.057211ns/ 57.21ps 
Cell Fall Delay : 4.07816 - 4.05011 = 0.02805ns/28.05ps 

```
# MAGIC DRC
 Commands to download the package from the web and extract it:

 ```
wget http://opencircuitdesign.com/open_pdks/archive/drc_tests.tgz
tar xfz drc_tests.tgz
```
We can now see an instance where a set of rules in the Metal 1 layer are not followed when we execute the "met3.mag" file in Magic. This failure could be caused by problems with the metal layer's patterning, such as the presence of shorts or openings. These concerns may cause electrical connections inside an integrated circuit design to fail.

```
magic -d XR met3.mag
```

<br />

![magic-drc-1](https://github.com/Y09mogal/Advanced_Physical_Design_using_openlane_skywater130/assets/79003694/cba61a57-8645-4e61-9dc0-9e84eb4191c1)

<br />

Commands to see metal cuts:
```
cif see VIA2

```
<br />

![magic-drc-2](https://github.com/Y09mogal/Advanced_Physical_Design_using_openlane_skywater130/assets/79003694/5cc579e3-1c34-449b-8195-938216ca3dde)

<br />

### Lab To Fix poly.9 error in SKY130 Tech File
Command to load poly file
```
load poly.mag
```
following screen will appear

<br />

![magic-drc-3](https://github.com/Y09mogal/Advanced_Physical_Design_using_openlane_skywater130/assets/79003694/6187e387-24ea-4d99-8b7d-2da8107ec7f5)

<br />

The errors are evident, as we can see. To correct that, we must make certain changes to the SKY130 technology file. 

In line 
```
spacing npres *nsd 480 touching_illegal \
	"poly.resistor spacing to N-tap < %d (poly.9)"
```
<br />

![magic-drc-4](https://github.com/Y09mogal/Advanced_Physical_Design_using_openlane_skywater130/assets/79003694/7192f70c-8a80-4db2-bcb9-50091b157572)

<br />

Change to
```
spacing npres allpolynonres 480 touching_illegal \
	"poly.resistor spacing to N-tap < %d (poly.9)"
```


also 
```
spacing xhrpoly,uhrpoly,xpc alldiff 480 touching_illegal \

	"xhrpoly/uhrpoly resistor spacing to diffusion < %d (poly.9)"
```
<br />

![magic-drc-5](https://github.com/Y09mogal/Advanced_Physical_Design_using_openlane_skywater130/assets/79003694/1708b287-1340-4db8-9200-45e23b07f441)

<br />


change to
```
spacing xhrpoly,uhrpoly,xpc allpolynonres 480 touching_illegal \

	"xhrpoly/uhrpoly resistor spacing to diffusion < %d (poly.9)"

```
### Modified layout

<br />

![magic-drc-6](https://github.com/Y09mogal/Advanced_Physical_Design_using_openlane_skywater130/assets/79003694/deea8eff-c80c-4fdd-b9fd-74af1e8737f3)

<br />

</details>



# Day 4
## Pre-layout Timing Analysis And Importance Of Good Clock Tree

<details><summary>Timing Modelling Using Delay Tables</summary>
It is critical to confirm that the A and Y ports of the CMOS Inverter, which are located on the li1 layer, are properly positioned at the junction of horizontal and vertical tracks in order to guarantee that they comply with the port requirements. This can be done by checking the "tracks.info" file, which contains information about track spacing and orientation.

<br />

![tracks-info](https://github.com/Y09mogal/Advanced_Physical_Design_using_openlane_skywater130/assets/79003694/27a6335a-8ef3-440e-8b28-8fad58bbde38)

<br />

To ensure that the ports line properly at the crossing point, the grid spacing in Magic (tkcon) must be synchronised with the X and Y values of the li1 layer. The following command can be used to establish this grid-track alignment:
```
grid 0.46um 0.34um 0.23um 0.17um
```
<br />

![layout1](https://github.com/Y09mogal/Advanced_Physical_Design_using_openlane_skywater130/assets/79003694/ec928b0c-216a-49d9-ba0c-fdaf1296ea6a)

<br />

### Creating port defination
Following the completion of the layout, the cell's LEF (Library Exchange Format) file must be generated. It is critical to configure properties and definitions for the cell's pins throughout this step to assist the placer and router tools. A cell containing ports is represented as a macro cell in LEF files, and these ports are defined as the macro's declared PINs. The first stage in this technique is to specify the ports and ensure that the relevant class and use attributes for each port are defined in accordance with the standard format.
Follow these procedures in the Magic console to effectively setup the ports:

 Load your design's.mag file, specifically the inverter layout.

 Go to the "Edit" menu and choose "Text." This action will result in the display of a dialogue box.

   Double-click on the letter 'S' at the I/O labels on the layout in the dialogue box.

   The text field will be filled in automatically with the correct string name and size for the port.

   To validate the port definition, check the "Port enable" button, which indicates that it works as a port. Additionally, make sure the "Default" checkbox is unchecked.

By following these procedures, you may efficiently define and configure ports in your layout, making them easier to recognise and use in the later LEF file production process.

<br />

![port-def](https://github.com/Y09mogal/Advanced_Physical_Design_using_openlane_skywater130/assets/79003694/64e4c0fb-d273-495b-9d33-7265490df701)

<br />

# Standard cell LEF Generation
Before the extraction Of LEF file we have to define the function of each port using the following commands:
```
port A class input
port A use signal

port Y class output
port Y use signal

port VPWR class inout
port VPWR use power

port VGND class inout
port VGND use ground
```
Now to extract file  following commands is used:
```
lef write
```

<br />

![lef](https://github.com/Y09mogal/Advanced_Physical_Design_using_openlane_skywater130/assets/79003694/c06a1ec5-4cfc-48ad-b8d6-435445304f0a)

<br />

### Integrating Custom cell in Openlane
we should copy the extracted LEF file to picorv32a source directory, and also sky130_fd_sc_hd_typical.lib file from vsdcelldesign/libs ditrectory

```
cp sky130_vsdinv.lef /home/tyrionlanni/OpenLane/designs/picorv32a/src/
cp sky130_fd_sc_hd__* /home/tyrionlanni/OpenLane/designs/picorv32a/src/
```
We have to modify config.tcl file also
```

# Design
set ::env(DESIGN_NAME) "picorv32a"

set ::env(VERILOG_FILES) "$::env(DESIGN_DIR)/src/picorv32a.v"

set ::env(CLOCK_PORT) "clk"
set ::env(CLOCK_NET) $::env(CLOCK_PORT)

set ::env(GLB_RESIZER_TIMING_OPTIMIZATIONS) {1}

set ::env(LIB_SYNTH) "$::env(OPENLANE_ROOT)/designs/picorv32a/src/sky130_fd_sc_hd__typical.lib"
set ::env(LIB_SLOWEST) "$::env(OPENLANE_ROOT)/designs/picorv32a/src/sky130_fd_sc_hd__slow.lib"
set ::env(LIB_FASTEST) "$::env(OPENLANE_ROOT)/designs/picorv32a/src/sky130_fd_sc_hd__fast.lib"
set ::env(LIB_TYPICAL) "$::env(OPENLANE_ROOT)/designs/picorv32a/src/sky130_fd_sc_hd__typical.lib"

set ::env(EXTRA_LEFS) [glob $::env(OPENLANE_ROOT)/designs/$::env(DESIGN_NAME)/src/*.lef]

set filename $::env(DESIGN_DIR)/$::env(PDK)_$::env(STD_CELL_LIBRARY)_config.tcl
if { [file exists $filename] == 1} {
	source $filename
}
```
To invoke OpenLANE and run synthesis with the new standard cell library, use the following commands:

```
set lefs [glob $::env(DESIGN_DIR)/src/*.lef]
add_lefs -src $lefs
```

<br />

![synth-res](https://github.com/Y09mogal/Advanced_Physical_Design_using_openlane_skywater130/assets/79003694/61f96be8-3c75-4e87-89f7-88441401d7c3)

<br />


### Introduction to delay table
In chip design, delay has a substantial impact on a number of timing characteristics. The delay of a cell is controlled by parameters such as its size and threshold voltages, and it is frequently described in the form of a timing table. Notably, delay is not a constant; it fluctuates depending on parameters such as input transitions and output loads.

Data from input slew and load capacitance, coupled with various buffer sizes, are contained in delay tables. These tables function as critical timing models. When algorithms use these tables, they calculate buffer delays by taking input slew and load capacitance into account. Interpolation techniques are used to ensure accurate timing analysis and signal integrity when precise data is unavailable.

<br />

![delay-table](https://github.com/Y09mogal/Advanced_Physical_Design_using_openlane_skywater130/assets/79003694/d1c83758-3597-40a5-86a4-61a1b2806f03)

<br />

Now we will run placement

After placement we will see sky130_vsdinv is in the layout or not:
```
magic -T /home/tyrionlanni/OpenLane/vsdstdcelldesign/libs/sky130A.tech lef read /home/tyrionlanni/OpenLane/designs/picorv32a/runs/RUN_2023.09.17_12.29.33/tmp/merged.nom.lef def read /home/tyrionlanni/OpenLane/designs/picorv32a/runs/RUN_2023.09.17_12.29.33/results/floorplan/picorv32.def

```

<br />

![day-4-pnr](https://github.com/Y09mogal/Advanced_Physical_Design_using_openlane_skywater130/assets/79003694/499221b4-51fd-4529-a5c2-47024715fc1f)

<br />


</details>

<details><summary>Timing Analysis with ideal clocks using openSTA </summary>

## SETUP TIME AND HOLDTIME
In digital circuit design, "setup time" and "hold time" are crucial timing factors that determine when valid data must be stable in relation to the clock signal. These factors are particularly important in synchronous digital systems, where data must be correctly collected on either the rising or falling edge of a clock signal.

The following is a breakdown of setup and hold time:

- Setup Time (Tsu): The minimum length before the clock edge (rising or falling) during which the input data must be steady and legitimate.
        In plain terms, it is the time interval preceding the clock edge during which the data input must remain intact in order for the flip-flop or latch to collect the right data.
        If data changes too near to the clock edge, the flip-flop may not have enough time to sample the data accurately, potentially leading to errors.

- Hold Time (Th): The minimum amount of time after the clock edge that the input data must remain steady and legitimate.
To prevent data corruption, it assures that the data remains intact for a predetermined duration after the clock edge.
 Data changes that occur too soon after the clock edge might cause a hold time violation, compromising the circuit's reliability.

Therefore, setup time and hold time are critical timing limitations that provide correct data sampling by flip-flops and latches in digital circuitry. Violations of these limits may result in setup and hold time violations, which may cause faults in the circuit's operation. Designers must carefully evaluate these characteristics during the design and timing analysis phases to ensure that their digital systems operate reliably and robustly.

## Clock jitter

Clock jitter, another important aspect in digital circuit design, is caused by a variety of reasons including clock generator circuits, noise, power supply changes, and interference from surrounding components. Accounting for jitter as a key element in timing closure is critical, as it can drastically impair a circuit's performance.

A key indicator for evaluating the reliability of a clock signal is period jitter. It estimates the difference between the actual cycle time of a clock signal and the ideal period across a large number of randomly chosen cycles (often about 10,000). Period jitter can be reported as the average deviation (RMS value) across these cycles or as the difference between the greatest and minimum deviations within the chosen group, which is known as peak-to-peak period jitter. Period jitter must be evaluated to guarantee that the timing of a clock signal remains constant under varying operating conditions.

Cycle-to-cycle jitter (C2C) is the difference between two successive clock cycles within a randomly chosen set of cycles, which is typically around 10,000 cycles. C2C jitter is frequently expressed by engineers as the greatest observed value within this group. This measurement aids in the capture of high-frequency jitter variations that can have an impact on the functioning of a circuit.

Phase noise is a phenomena linked with clock jitter in the frequency domain. It exhibits random phase changes within a waveform that are fast and short-lived. Analysing phase noise in the frequency domain provides useful information about the quality and stability of a clock signal. Engineers can use phase noise data to generate jitter values for digital design analysis.

For the purpose of developing reliable digital circuits, it is crucial to comprehend and measure clock jitter, whether it is expressed in terms of period jitter, cycle-to-cycle jitter, or phase noise. Engineers may ensure that their designs meet timing specifications and perform reliably under a variety of operating circumstances by addressing the causes of jitter and implementing suitable design margins.

</details>
<details><summary> Clock tree synthesis TritonCTS and signal integrity </summary> 
	
## Clock Tree Synthesis

A method for evenly dividing the clock throughout all sequential components of a VLSI design is called clock tree synthesis. Clock Tree Synthesis is used to reduce skew and latency. The clock tree limits and placement data are provided as input to Clock Tree Synthesis. Clock Tree Synthesis (CTS) is a technique for balancing the clock delay to all clock inputs in an ASIC design by inserting buffers/inverters along the clock paths. 

As a result, CTS is employed to reduce insertion delay and balance the skew. All clock pins were formerly powered by a single clock source prior to Clock Tree Synthesis. Clock tree synthesis consists of clock tree creation as well as clock tree balance.Clock tree inverters can be used to build a clock tree with the correct transition (duty cycle), and clock tree buffers (CTB) can balance the clock tree to meet the skew and latency requirements.

Clock Tree Synthesis (CTS) is significant in the following ways:

- Synchronisation in Digital ICs: Clock signals are critical in synchronising the actions of various components in digital integrated circuits (ICs). They ensure that data is sampled or altered at the correct times, preserving the integrity of digital activities.

  - Avoiding Timing Violations: Maintaining properly synchronised clocks is critical for avoiding setup and hold time violations. These breaches can occur if data transitions are not synchronised with clock edges properly. CTS assists in the establishment of exact timing relationships, reducing the possibility of such violations and assuring proper data processing.

- Managing Complex ICs: In the context of modern integrated circuits (ICs), which can contain millions or even billions of transistors, effective and dependable clock distribution is becoming an increasingly difficult challenge. CTS helps to manage this complexity by optimising how the clock signal is routed and dispersed around the chip, ensuring that it reaches all portions of the design consistently and within specified time restrictions.
    
<br />

![img1](https://github.com/Y09mogal/Advanced_Physical_Design_using_openlane_skywater130/assets/79003694/6e1f8aa8-e9c4-4d10-a12d-56191d481044)

<br />

CTS Buffering


<br />

![img2](https://github.com/Y09mogal/Advanced_Physical_Design_using_openlane_skywater130/assets/79003694/fd15353e-489f-4859-a657-cdf6803f7b3f)

<br />

Cross talk & Cross Net Shielding
- Crosstalk: Unwanted interference between adjacent signal traces or conductors, causing signal degradation or errors.

- Cross Net Shielding: Using shielding layers or materials to physically isolate and protect different signal nets from each other, reducing crosstalk and maintaining signal integrity.

<br />

![img3](https://github.com/Y09mogal/Advanced_Physical_Design_using_openlane_skywater130/assets/79003694/867322bd-6e2a-42ce-b915-1f3d50c4c870)

<br />

  ## LAB
  Commands to run clock tree synthesis

  ```
run_cts
write_verilog ./designs/picorv32a/picorv32a_cts.v

```
<br />

![i1-final](https://github.com/Y09mogal/Advanced_Physical_Design_using_openlane_skywater130/assets/79003694/b74e323e-cc72-44ff-984e-7ae6fc292653)

<br />

Since clock tree synthesis has not been performed yet, the analysis is with respect to ideal clocks and only setup time slack is taken into consideration. The slack value is the difference between data required time and data arrival time. The worst slack value must be greater than or equal to zero. If a negative slack is obtained, following steps may be followed:

- Change synthesis strategy, synthesis buffering and synthesis sizing values
- Review maximum fanout of cells and replace cells with high fanout



  Commands

 ```
openroad
read_lef <path of merge.nom.lef>
read_def <path of def>
write_db pico_cts.db
read_db pico_cts.db
read_verilog /home/tyrionlanni/OpenLane/designs/picorv32a/runs/RUN_2023.09.17_12.29.33/results/synthesis/picorv32a.v
read_liberty $::env(LIB_SYNTH_COMPLETE)
read_sdc /home/shivangi/OpenLane/designs/picorv32a/src/my_base.sdc
set_propagated_clock (all_clocks)
report_checks -path_delay min_max -format full_clock_expanded -digits 4
```
<br />

![i2-final](https://github.com/Y09mogal/Advanced_Physical_Design_using_openlane_skywater130/assets/79003694/22b0fa91-f7d3-43ec-a640-f99d25f60619)

<br />


<br />

![i3](https://github.com/Y09mogal/Advanced_Physical_Design_using_openlane_skywater130/assets/79003694/3bcd2bf5-aa43-42ce-9875-0a7dd5a3e089)

<br />


Commands to check clock buffers :
```
echo $::env(CTS_CLK_BUFFER_LIST)
set $::env(CTS_CLK_BUFFER_LIST) [lreplace $::env(CTS_CLK_BUFFER_LIST) 0 0]
echo $::env(CTS_CLK_BUFFER_LIST)
```

  
</details>

# Day 5

## Final steps for RTL2GDS using tritonRoute and openSTA

<details><summary> Final steps in RTL2GDS </summary>

### Maze Routing and Lee's algorithm

The maze-routing algorithm reffered here, which is commonly employed in the context of chip multiprocessors (CMPs) and grid-based mazes, is meant to efficiently identify routes or paths between two places while minimising overhead. This algorithm is critical in the field of integrated circuit design and routing, where the goal is to connect numerous components on a chip while using the least amount of resources.
Routing activities are divided into four steps:
Routing at the global level:

- Establishes a high-level path for each net.
- Focuses on overall routing topology.
- Avoids obstacles and congestion areas.

Track Assignment:

- Divides routing area into tracks or channels.
-  Allocates tracks to specific nets.
-   Considers routing layer constraints.

Detail Routing:

- Determines precise routing paths for each net.
- Minimizes wirelength and avoids conflicts.
- Adheres to design rules and constraints.

Search and Repair:

- Identifies and resolves routing issues.
-   Handles design rule violations and congestion.
-   May require backtracking and iterative adjustments.


The Lee algorithm is a grid-based routing method that is commonly used in semiconductor design. It starts with a set of source and target points and labels grid cells to discover the shortest path between them, frequently favouring efficient L-shaped pathways over zigzags. While useful for global routing tasks, it can be time-consuming for sophisticated designs with a large number of pins. As a result, various methods for addressing scalability and specialised routing difficulties have arisen. The routing mechanism chosen is determined by the design's complexity and resource restrictions.


<br />

![i1](https://github.com/Y09mogal/Advanced_Physical_Design_using_openlane_skywater130/assets/79003694/ce0ee3ac-9816-4b2b-b9fa-f8d581dd47ea)

<br />

## DRC
Design Rule Checking (DRC) is an important stage in the physical design process that ensures a design adheres to production limits imposed by the process technology of choice. Each technology has its own set of rules, which grow in number and complexity as manufacturing technology advances to smaller nodes. DRC validates compliance with foundry-supplied established process standards, preventing chip failures. It is crucial in determining the quality of a chip. Physical wire qualities such as minimum width, spacing, and pitch are important DRCs, and they solve concerns such as signal short violations by utilising more metal layers while thoroughly inspecting vias, width, and spacing.

<br />

![i2](https://github.com/Y09mogal/Advanced_Physical_Design_using_openlane_skywater130/assets/79003694/a6f0c56e-a917-4674-8a1a-37356de02f77)

<br />

</details>

<details><summary>Power distribution Network And Routing </summary>


A Power Delivery Network (PDN) is the underlying infrastructure that ensures a continuous and dependable supply of electrical power to all components within an integrated circuit (IC) or chip. A well-designed PDN is crucial for ensuring that all devices on the chip receive the required voltage levels with low noise and voltage dips.

The first stage in creating a PDN is thorough power grid design. This includes estimating the overall power requirements of the chip, which includes voltage levels (usually VDD and VSS, or ground) as well as the current demands of individual functional blocks. Designers must also consider the power delivery network's topology, which includes the placement of power rails, ground lines, power domains, and their interconnections.

Strategically placed decapacitors (decaps) act as local energy reserves to improve voltage stability and reduce noise. They are critical in correcting for sudden fluctuations in current demand, especially during switching events. The projected load variations and voltage fluctuations in different chip regions dictate the selection and positioning of decaps.
 The following command is used to determine the design's most recent stage:

 ```
echo $::env(CURRENT_DEF)
```
Now run the following command after the cts:

```
gen_pdn
```

<br />

![d5-i1](https://github.com/Y09mogal/Advanced_Physical_Design_using_openlane_skywater130/assets/79003694/1695646e-8908-4425-80d1-75267ac4e8a5)

<br />

![i2-done](https://github.com/Y09mogal/Advanced_Physical_Design_using_openlane_skywater130/assets/79003694/7cf25220-2c0b-446a-97ba-e94df05fa044)

<br />

Following the creation of the Power Distribution Network (PDN), designers use numerous analysis techniques to simulate and assess its functioning. These evaluations include voltage drop, IR (voltage drop owing to resistance), electromigration, and other power-related issues.
Designers may use optimisation techniques such as buffer insertion, voltage islands, and voltage scaling to improve the performance of the PDN.
After the chip's layout has been finalised, designers begin post-layout verification to check that the actual layout adheres to the PDN plan and design guidelines. Any discrepancies or issues that are detected during this stage are rectified.
Once the PDN has been successfully developed and verified, and all design rules have been followed, the chip design is considered ready for "tape-out." This signifies that the final layout data is delivered to a semiconductor foundry for manufacture, which marks an important step in the chip manufacturing process.

## ROUTING

Global Routing:
- Purpose: Global routing serves as the first step in the routing process, defining the primary pathways for interconnections.
- Objective: It aims to establish approximate wire locations and high-level connections between components.
- Scope: Global routing focuses on the overall layout, determining the general routing topology.
- Efficiency: It is typically faster and less detailed than detailed routing.
- Use Cases: Global routing is crucial for creating a rough layout of interconnections, aiding in initial floorplanning, and providing an overview of the chip's connectivity.

 Detailed Routing:
        
- Purpose: Detailed routing follows global routing and concentrates on the precise routing paths for individual nets.
- Objective: It involves the selection of exact wires and vias, ensuring a functional and manufacturable layout.
- Scope: Detailed routing deals with the fine-grained routing of each net, considering design constraints and manufacturing rules.
- Precision: It ensures the highest level of precision and adherence to all constraints, including minimum spacing, width, and metal layer utilization.
- Use Cases: Detailed routing is the final step in the physical design process, where each wire's exact path is determined to meet timing closure and comply with design specifications.
       
<br />

![i3](https://github.com/Y09mogal/Advanced_Physical_Design_using_openlane_skywater130/assets/79003694/3963d835-3b5a-43ac-9006-ada78db08833)

<br />

</details>
<details><summary>Triton Routing Features</summary>

## Features of TritonRoute:
	
- TritonRoute supports pre-processed route guides, allowing designers to lead routing paths depending on specified standards.

- Assumes each net meets inter-guide connectivity needs: The tool assumes that each net meets inter-guide connectivity requirements, simplifying the routing procedure.

- Panel routing strategy based on MILP: TritonRoute uses a Mixed-Integer Linear Programming (MILP) approach for panel routing, which can yield optimal routing solutions.

- Intra-layer parallel and inter-layer sequential routing framework: TritonRoute efficiently navigates several layers in the chip architecture, optimising routing paths across different metal layers, by combining intra-layer parallel and inter-layer sequential routing.


## Pre-processed route guides:
TritonRoute's approach to pre-processed route guides involves several key actions:

- Initial Route Guide Analysis: The tool initially analyzes the directions specified in the preferred route guides. If it encounters non-directional guides, TritonRoute breaks them down into unit widths for routing clarity.

- Guide Splitting: When non-directional routing guides are identified, TritonRoute splits them into unit widths, making them more manageable for the routing process.

- Guide Merging: TritonRoute simplifies routing by merging guides that are orthogonal to the preferred guides, streamlining the routing path.

- Guide Bridging: In cases where guides run parallel to the preferred routing guides, TritonRoute introduces an additional layer to bridge them, ensuring efficient routing within the preprocessed guides.

- Inter Guide Connectivity: TritonRoute assumes that route guides for each net already satisfy inter-guide connectivity. This means guides should be on the same metal layer with touching guides or on neighboring metal layers with non-zero vertical overlap area (utilizing vias for connections). Additionally, each unconnected terminal (e.g., pins of standard cell instances) should have its pin shape overlapped by a routing guide, indicated by a black dot (pin) with a purple box (metal1 layer).

<br />

![i1](https://github.com/Y09mogal/Advanced_Physical_Design_using_openlane_skywater130/assets/79003694/f385c698-5ebf-4877-95ac-a724e71766d4)

<br />


## Inter guide connectivity and intra-inter layer routing:
Inter-Guide Connectivity:

1. Guides are considered connected if they:
        - Share the same metal layer.
        - Touch or intersect along their edges.
        - Exist on neighboring metal layers with a non-zero vertical overlap area (using vias for connections).

2. Intra-Inter Layer Routing:

    - This involves routing signals between different layers of the chip.
    - It ensures connections between different metal layers, often using vias.

<br />

![i2](https://github.com/Y09mogal/Advanced_Physical_Design_using_openlane_skywater130/assets/79003694/e6a28b5f-ee98-4854-b1dd-75f36c42a4f5)

<br />

## Handling connectivity:
In handling connectivity within Triton Detailed Route, the following components and concepts are essential:

Inputs:

- LEF File: Contains information about library elements, including standard cells and their characteristics.
- DEF File: Provides placement and location data for components in the chip.
- Preprocessed Route Guides: Guides that specify routing directions and paths.
- Constraint Files: These files include:
- Route Guide Honoring: Enforces adherence to preferred routing guides.
- Connectivity Constraints: Specify how components and guides should be interconnected.
- Design Rules: Define rules and constraints for the chip's physical design.

Access Point:

An "Access Point" is an on-grid metal point located on the route guide.Its purpose is to facilitate connections to lower-layer segments, upper-layer pins, or I/O ports.Access Points play a critical role in enabling routing between different layers and components.

Access Point Cluster:

An "Access Point Cluster" refers to a collection of all access points.These access points are derived from various sources, including lower-layer segments, upper-layer guides, pins, or I/O ports.Access Point Clusters help streamline and organize the connectivity options for routing between different layers and components.

These components and concepts are integral to Triton Detailed Route's ability to effectively handle and optimize connectivity in the chip design process. They ensure that routing solutions are both efficient in terms of wire length and via count while adhering to specified constraints and design rules.

<br />

![i3](https://github.com/Y09mogal/Advanced_Physical_Design_using_openlane_skywater130/assets/79003694/8e9e9721-a5d8-4401-98ef-4a1e5826f091)

<br />

##  Topology Algorithm :

<br />

![i4](https://github.com/Y09mogal/Advanced_Physical_Design_using_openlane_skywater130/assets/79003694/bd12dd1b-3f6e-4430-ba8a-2c6429f43c3a)

<br />

</details>


### Acknowledgments
* Kunal Ghosh, CEO, and Co-founder, VSD Corp. Pvt. Ltd.

### Contact Information
* Yash Dharemsh Mogal, IMtech 2020, International Institute of Information Technology, Bangalore Yash.Mogal@iiitb.ac.in
* Kunal Ghosh, CEO, and Co-founder, VSD Corp. Pvt. Ltd. kunalghosh@gmail.com


### References
- https://www.vsdiat.com
- https://github.com/Devipriya1921/Physical_Design_Using_OpenLANE_Sky130
- https://github.com/The-OpenROAD-Project/OpenLane
- http://opencircuitdesign.com/magic/
- https://github.com/nickson-jose/vsdstdcelldesign/
- https://chat.openai.com

