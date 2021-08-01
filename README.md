
# 130nm PLL Clock Muluplier
8x PLL Clock Multiplier IP on the Google-Skywater 130nm node.
Tested through spice simulations on skywater 130nm tt corner at room termperature
Generates 8x Multiplied Clock

# Google-SkyWater 130nm PDK:
This PLL circuit is built on the Google-Skywater 130nm node. It is a mature 180nm-130nm hybrid technology originally developed internally by Cypress Semiconductor. The SkyWater Open Source PDK is a collaboration between Google and SkyWater Technology Foundry to provide a fully open source Process Design Kit and related resources, which can be used to create manufacturable designs at SkyWater’s facility.
# PLL DAY 1
![PICTURE 8](https://user-images.githubusercontent.com/86368099/127774496-4b9278e4-631f-4b81-9f48-017f4bd84758.png)
## Phase Frequency Detector
A phase detector (PD) generates a voltage, which represents the phase difference between two signals. In a PLL, the two inputs of the phase detector are the reference input and the feedback from the VCO. The PD output voltage is used to control the VCO such that the phase difference between the two inputs is held constant, making it a negative feedback system.
## Charge Pump
Converts the Digital Signals to Analog Signals
## LPF
The primary function is to determine loop dynamics, also called stability. This is how the loop responds to disturbances, such as changes in the reference frequency, changes of the feedback divider, or at startup.In term of Control System it adds poles in the transfer functions
## VCO
The VCO is related via a phase stabilization mechanism. The most significant building block in PLL is the VCO. The VCO output frequency determines the efficacy of the PLL. It absorbs much of the system's power when we run the VCO at high frequencies. Therefore, in order to minimize power usage, we need to concentrate further on this block.it is used to provide variable oscilation
## Frequency Divider
PLLs also include a divider between the reference clock and the reference input to the phase detector. If the divider in the feedback path divides by 8 the Frequency is divided by 1/2*N*8

## Important terms used in PLL
1) Lock-in range
Range of freuencies which PLL can stay in lock in steady state. It mainly depends on the range of frequencies VCO can produce and is limited by the dead zone of PFD.
2)Capture range
Range of frequencies which PLL can lock when started from unlocked condition. Capture range is smaller than the lock-in range. It depends on the loop filter bandwidth.
3)Settling time
The time taken by the PLL to lock from an unlocked state is called the settling time. It depends on how quickly charge pump rises to the stable value.

## Labs
use the source code to build any software tool as it gives the latest updates and fixes.
Here, we will use two tools. They are:
Ngspice : for transistor level simulation
Magic : for layout design and parasitic extraction.
Using tools
From the ubuntu package , we will be downloading Ngspice directly. Magic we will compile it with the source code because we need the latest version of Magic to support Skywater130nm technology node.
Ngspic: ngsice<circuit_file_name>
It plots the results based upon the simulation instruction given in the circuit file after simulating the given circuit file.
Magic: magic - T <Technology_file_from_PDK><the_layout_file_to_open>
It opens the layout file, where we can view the layout and make modifications to it. Magic has many features, out of which we will be using parasitic extraction and gds features.

## Flow
With the PLL specifications, we will perform SPICE-level circuit development and pre-layout simulation. After the pre-layout simulation develop layout. Before layout phase it is not known the interconnection or proximity or size of the different circuits. After layout, one can extract the capacitive effect which is called the parasitic extraction. After this, we have spice netlist which is more closer to the real worls. Then, we do the post simulation which is more accurate than thr pre-layout simulation.
Its is important to note that, after each simulation step, we should make several changes to bring the working of the circuit much closer to the required specification.

## Ngspice setup
use the following command to isntall Ngspice:
sudo apt-get install ngspice
Then fetch the sky130 library which consists of the transistor models which is needed for the simulation.

## Magic setup
 use the source code from opendesigncircuit.com. We can do the git clone and intall the Magic. Then, compile it, for that one should install the dependencies form the opendesigncircuit.com. and Then run the ./configure command after that run the make command and after that run the command "sudo make install"
Then we should install the sky130nm technology information which Magic needs for desiging the layout.

## PLL Specifications
1)Corner = TT
2)Supply voltage = 1.8V
3)Temperature = Room temperature
4)Input Fmin= 5MHz , Fmax= 12.5MHz
5)Multiplier = 8x
6)Jitter (RMS) < 20ns
7)Duty cycle = 50%

# DAY 2 PLL
A spice file is a text file with .cir extenstion.
For example, if one want to write code for frequency divider circuit, write the following command in terminal:
touch FD.cir
![PICTURE 2](https://user-images.githubusercontent.com/86368099/127780639-93b97f5e-730f-4206-ae69-7cb0e7e4b479.png)

then page will be created where we should write the code for the desired circuit. Don't forget to include the SPICE library file in it.
Similarly we do it for all the blocks of PLL.
After creating the .cr files we run the Ngspice simulation.
# Pre-layout simulations
## Phase Frequency Detector
Firstly cloning the transistor modules needed for simulation from the primitive libraries of sky130.lib The files needed nfet_1v8 and pfet_1v8 and "tt.pm3.spice" Copying these files in a seperate folder spice_lib Generating the sky130.lib by using the commands :
![PICTURE 1](https://user-images.githubusercontent.com/86368099/127780540-c1503a51-cb40-4db4-b84f-560d48cdd010.png)

### The command to be run to obtain the PFD waveform is as shown below:
![PICTURE 9](https://user-images.githubusercontent.com/86368099/127779446-07716782-06df-4698-8e4f-19c3c9c041f6.png)

### The PFD waveform is as shown below:
![127760347-cac474f0-bd49-4eae-a8f5-6c7492be2f07](https://user-images.githubusercontent.com/86368099/127779473-0d07cfb6-f05c-4831-b91a-ef1e71a32ede.jpg)

## Charge Pump
response when UP signal is high:
![127760599-9c5bc28d-81e8-4af5-b811-cea57816073d](https://user-images.githubusercontent.com/86368099/127779535-762de74b-8b1f-45c5-ae07-2931d5ad475d.jpg)

output rise due to charge leakage:
![127760602-cd0bd984-2aa0-4069-bb1e-e0a7175ee05f](https://user-images.githubusercontent.com/86368099/127779595-841ffadd-44e5-49ba-9a94-48ff8d508725.jpg)
Leakage: 40uV

## VCO
### VCO waveform
![127760904-d2aabad9-c442-495e-98ae-555d6ffbb244](https://user-images.githubusercontent.com/86368099/127779698-263ec056-3792-4401-9b1e-9e5e6aa25e7c.jpg)

## Frequency Divider
### The Frequency Divider waveform
![127760907-64091b47-a446-41d1-8060-17bcb3c86b6e](https://user-images.githubusercontent.com/86368099/127779764-4564a393-5a60-4cdd-b28b-d1f39988d632.jpg)

## PLL
# The PLL waveform
![127768478-affcafc3-d75f-4acb-b51c-dcd16953f494](https://user-images.githubusercontent.com/86368099/127779786-fe018b7b-9188-4293-8d20-464ee1004599.jpg)
![127767982-92ef59b0-ab66-4669-8aa1-fa20243aab79](https://user-images.githubusercontent.com/86368099/127780576-121be783-2ddf-4a5d-a82d-a6de396c597b.png)


## Troubleshooting
![PICTURE 7](https://user-images.githubusercontent.com/86368099/127779820-0b8cff62-2380-4608-8a0b-25427cc34192.png)

# Layout
Layout colour code
p-diffusion - plane orange colour
n-diffusion - plane green colour
polysilicon - plane red
n-well - slash lines
metal1 layer - purple
local interconnect layer - blue
To connect transistors, we use interconnect layer. To connect metal layers, we use the contact/via.

## Command to open MAGIC LAYOUT

Magic -T sky130A.tech PFD.mag
Highest width in sky130 node = 720nm
smallest width in sky130 node = 360nm
highest lenght in sky130 node = 150nm

## Phase Frequency Detector
![127762217-c5c3df73-665a-4179-b2d1-4df9f50f7d8e](https://user-images.githubusercontent.com/86368099/127780032-5fe06b4e-8158-4a61-ad34-b8e37d4be322.jpg)

## Charge Pump
![127762213-996c224e-7b1e-42f6-9b50-a8107cf0cc1f](https://user-images.githubusercontent.com/86368099/127780025-4f38e681-f214-458f-aa83-8d59e15cf87b.jpg)

## VCO
![127762210-81c50482-4f7a-47d0-8951-a326ee792f15](https://user-images.githubusercontent.com/86368099/127780010-ef67adfc-6a03-4df4-8ee8-89c1489709e8.jpg)

## Frequency Divider
![127763592-768a8bda-0612-49ca-9db1-f5d218d1736d](https://user-images.githubusercontent.com/86368099/127779985-e20585be-9cef-458a-98af-7e939854d7a0.jpg)

## Integrated PLL
![127762206-7b097ca1-0888-4d52-9484-5188fd702c6e](https://user-images.githubusercontent.com/86368099/127779955-5f1d6642-ecdc-4483-95b9-cd301e53a54c.jpg)

## Parasitic Capacitance
![127769266-1abd90ab-5144-47c6-bace-ef535823eb6e](https://user-images.githubusercontent.com/86368099/127780899-6b170302-d753-40d3-8f45-8b8e516ebec5.png)


# Post-Layout Simulation

## Phase Frequency Detector
![127766198-5b39072a-7a95-4b99-8272-1f658c7d8e50](https://user-images.githubusercontent.com/86368099/127780723-2ef942c3-8fdc-48e9-a36a-61681b0adf89.jpg)

## Charge Pump
### UP signal is high:
![127766841-8cdf0080-05f1-48c4-bfd7-3e30cf7f7d12](https://user-images.githubusercontent.com/86368099/127780753-1edb7757-bfde-486b-a2cd-54fa2f9f0f36.jpg)
### output rise due to charge leakage
![127768463-6d6e443a-828e-46d0-9dba-ee8a24bef279](https://user-images.githubusercontent.com/86368099/127780764-6d82ebd3-2cab-4060-a680-1f5d03438f4c.jpg)

## VCO
![127768371-fd0bd192-f68c-471c-997d-5077ee143037](https://user-images.githubusercontent.com/86368099/127780818-f7ff803b-3b0a-4339-9bd7-1d2e179544ea.jpg)

## Frequency Divider
![127766630-8942e55a-9c6d-48c7-8846-a3dc53005983](https://user-images.githubusercontent.com/86368099/127780849-c8e634cf-d88a-4d9b-9882-617b68089b2b.jpg)

## PLL
![127770984-7c90ec30-d566-4f25-936a-c7d98592a715](https://user-images.githubusercontent.com/86368099/127780865-957b71be-9c39-4dbc-a3d3-c4f6899660e6.jpg)



# Tapeout
![127770518-5cd6c9cf-e3ec-4d5e-be78-931630c07d30](https://user-images.githubusercontent.com/86368099/127780931-65fcd5a5-7462-4f07-b049-6bcd0fc671e3.png)

Tapeout means to send our design to the fab after we prepare it with all the additional support we require.
one should connect silicon wafer with the real world. For that  use I/O pads. Then for any kind of serial connectivity like I2C, UART and other peripherals, use their designs. Memory also has to be incorporated which takes a lot of space. Testing mechanisms should also be added. Taking care of all of this becomes complicated hence, one should choose a driver to enable IP to meet the desired requirements to undergo fabrication process. For this one can use Efabless Caravel SoC template.

# ACKNOWLEDGEMENT
1)Kunal Ghosh From VSD
2)Lakshmi Sathi
3)Mili Anand
4)Mansi Mohapatra

