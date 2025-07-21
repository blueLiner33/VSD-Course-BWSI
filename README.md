# Design Considerations
## Pin placement
Netlist connectivity of different gates. Described with HD. Common input left and output is on the right-hand side. Need to place close to input or output pins. The clock needs lower resistance bigger pin size so it goes out of the chip faster. If you want to go out faster, make the pins have less resistance.

## Floorplan using openlane
After syn the floor plan is the  next step. Standard cells are not placed.
Open up the config read for each stage, less ReadME shows what switches and what might be overwritten. The commands for package require openlane and prep-design picorv32a and run_synthesis, run_floorplan only seem to work when run one after another. Have to take priority from sky130A ex. .tcl file: When looking at less picrv32a floorplan.def
You can see the area of the computer die. First 0 lower left x and left y and upright x upright y. calc area of die. Above shows units

<img width="698" height="468" alt="Screenshot 2025-07-17 061307" src="https://github.com/user-attachments/assets/ff360cab-2d2f-4837-90f4-f42f5b79ed5d" />

## Decaps + IPs
IP can be reused repeativly. Pre-placed, then not touched. Pre placed have to be surrounded by decoupling caps because of voltage drops due to large physical distance. Caps solve this issue by acting as a recharge meaning they "pick" voltage back up closer to 1.  

## Power Planning
All blocks need power. For 16 bus switch signals, when the caps discharge at the same time, it causes a ground bounce. Ground bounce is unfiltered noise, meaning that switches are unsure if it is one or zero. The solution is to have power from many places think of power lines being grid-like. 

## Die details
utilization= netlist area/core area 
Utilization keep around 50% or 60%
Aspect Ratio =  Height of chip/ Width of chip
Logic system>floor_planning>placement>CTS>Routing

## Placement
The parts of chips like 'and', flip flops are represented by blocks or rectangles on a chip. Based on the diagram, put the blocks close to each other when connected. Then, close to the pins relevant to the blocks. To have signal integrity, you need something to help signal reach the block; use repeaters(buffers). Based on the distance inside the chip. Ones with high frequency place close together. After checking the placement. Want to try to keep them close together.

## Clocks 
The last stage, Static Timing analysis, is the last step and tells the max frequency.
All blocks need to receive the clock at the same time.

## Standard cells
They are kept in the Library. Some examples are Latch, DFF, and Inverter. The same function cell comes in different sizes and with different features. A higher threshold needs more time to shift. 

## CMOS inverter
switching threshold = VM
vm = vin = vout
supply a pulse to see what happens
try to find the rise delay and find fall delay. FInd poitn where output is falling and take 50%

## Timing
slew_low_rise_thr defines points to low point 20% or 30%
slew_high_rise_thr 20% 
slew is foudn time difference between the two.
slew_low_fall_thr bottom 20% other wave form same with slew_high_fall_thr
used to find slew
in_rise_thr for delay and one out point to calc delay
out_rise_thr out waveform point find delay 
in_fall_thr delay for fall waveform
out_fall_thr delay for fall 50% roughly

time(out_thr)-time(in_thr)=delay
negative delay poor choice of threshold point. 
transition time high-low for the fall (slew)

## Spice Deck
need to figure out parts values
PMOS>NMOS
PMOS 2 or 3 times bigger
Then need to find nodes and it in reference to other nodes what parts are between them
Need to name nodes

## 16-mask cmos process
selecting a substrate: doping is adding something else to a substrate
active region see n type and p type mosfets

creating active region for transistors 
need to clear an area with pocjets for active sites
then put p substrate then oxide than photoresist 
mask1 ex is layer in custom layer is a mask
mask is protection area is from light by using the mask
then remove mask
photoresist is stay proection layer for following steps of deprestion.
si3n4 is etched off then remove photoresist.
then place into oxidation furance to glow more oxidation
then that creates an isolation area called locos(local oxidation of silicon.) parts that are where transistors go are called birds beak. 

N-well and p-well formation.
### Cell Design Flow

#### Other
Can change varibles to change part of design flow

#### Inputs
Each cell has to go through the cell design flow. The inputs to design it are PDKs, foundry rules files (DRC&LVS, SPICE models), and user plus library specs. The user has to cell that fits in height, and supply voltage for the chip. Can also be based on layer requirements. 

#### Design Steps
Circuit desing have to get certain specs and based on SPICE simulations. I[drain_pmos]+I[drain_nmos]=0
Model the equation with pmos and nmos.
Then nmos/pmos network graph. Then find Euler's path(path traced only once). Then draw a stick diagram out of it. Into a proper layout next in the next step..

#### Outputs
CDL(circuit description language)
Take out the characteristics. Then get the timing of the cell.

#### characterization 
1. Read model files
2. Read SPICE netlist
3. Recognize the behavior of buffer
4. Read the subcircuit of the inverter
5. Connect power
6. Apply simiulus
7. Provide necessary output capacitance
8. Provide necessary simulation command
feed this into GUNA

# Commands
The logic of commands is important; most have to be run one after another.

## Order of important commands roughly
run package require openlane 0.9

<img width="1019" height="103" alt="Screenshot 2025-07-16 143323" src="https://github.com/user-attachments/assets/043cb7f0-b3c8-4619-82a6-2724bdfc9376" />
<img width="1444" height="761" alt="Screenshot 2025-07-16 094410" src="https://github.com/user-attachments/assets/32e36aef-bd13-4d4f-8290-f7c00675608f" />

Made mistake of not running -interactive flag. Correct command is './flow.tcl -interactive'

Then <img width="651" height="221" alt="Screenshot 2025-07-16 115030" src="https://github.com/user-attachments/assets/18a99a82-2e61-41fa-a16d-ef028acedb27" />
and 'prep -design picorv32a'

Then run 'run_synthesis'
Then run 'run_floorplan'
Then run 'run_placement'

Next important one in day 3 is
git clone https://www.bing.com/search?q=jose-vstdllstandard%20cells&qs=n&form=QBRE&sp=-1&lq=0&pq=jose-vstdllstandard%20cells&sc=5-25&sk=&cvid=B38878DF01D5462EA04B7C374C473050
then follow the screen shot

<img width="909" height="980" alt="Screenshot 2025-07-21 055140" src="https://github.com/user-attachments/assets/67bef143-836a-44b9-aa23-f17a8aad916c" />

Then run in your vsdstdcelldesign dir run magic -T sky130A.tech sky_inv.mag &
should see something like 

<img width="954" height="1189" alt="Screenshot 2025-07-21 055537" src="https://github.com/user-attachments/assets/56faf70e-00cd-446d-8980-86ac25c1d302" />

## Important files and how to view them
Navigate to the right directory and the use ls to find files of interest. Than use less to look at them. Example of what a less file might look at

<img width="698" height="468" alt="Screenshot 2025-07-17 061307" src="https://github.com/user-attachments/assets/afcc27bd-f6d0-4d66-a2bb-646ff18cc198" />

To view the floorplan cd to floorplan dir which is after picorv32a/runs/[your run]/results/ . Then run' magic -T /home/vsduser/Desktop/work/tools/openlane_working_dir/pdks/sky130A/libs.tech/magic/sky130A.tech lef read../../tmp/merged.lef def read picorv32a.floorplan.def
Should look like
<img width="875" height="906" alt="Screenshot 2025-07-17 094650" src="https://github.com/user-attachments/assets/a2a5aad6-a01d-433e-9c36-28a1bedf8a91" />

To zoom in on image right than left mouse click then press z. Hoover over and press s to select object. Then type 'what into selection window and it will show layer.
