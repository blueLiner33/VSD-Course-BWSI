this in github do not change information or where pictures are. just make it more aesthetic and functional and complete sentences # Design Considerations
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

#### creating active region for transistors 
need to clear an area with pocjets for active sites
then put p substrate then oxide than photoresist 
mask1 ex is layer in custom layer is a mask
mask is protection area is from light by using the mask
then remove mask
photoresist is stay proection layer for following steps of deprestion.
si3n4 is etched off then remove photoresist.
then place into oxidation furance to glow more oxidation
then that creates an isolation area called locos(local oxidation of silicon.) parts that are where transistors go are called birds beak. 

#### N-well and p-well formation.
p well is done using boron diffused by ion implantation
phosphorous is n-well using ion implantation

#### formation of 'gate'
two things that control Vt are Na doping concentration and Cox odixde capacitance
yet again photresists and mask
for boron lower energy so just at surface in right area
voltage controled aslow low just to certain level
then fix the oxide using hydrofluric acid and regrow oxide
Put the polysilicon layer, then dope with more impurities to have low resistance. 

#### lightly doped drain formation 
Need p+, p-, N is what profile you want. 
for other side N+,n-,p
Why do you want a profile
2 reasons hot e- effect
short channel effect
have to protect slightly doped thick layer on whole structure and leave things on side walls

#### source and drain formation
thin screen oxide to avoid channeeling during implants
dope p well side with arsenic 
then same with p- implant and boron is implanted
then put into high-temperature furnace. more into n and p well, high temperature annealing. 

#### setps to form contacts and interconnects
titanium and intercunties using sputtering
heat wafer
form TiSi2
TiN for local communication
Then TiN is etched off using RCA cleaning

##### Higher level metal formation 
put SiO2 layer doped with boron or phosphorus 
Then chemically to get a flat layer using CMP 
Then deposite TiN
build contact holes acess to top layers. 
Then deposite W layer then make it flat usign CMP
Al layer deposition
plasma etched so metal is connect to top using Al
deposite So2 gain then build contact holes again
deposite TiN then W
Then if last level then use something to protect level then build last conact holes up.

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

extract spice
using the command extract all in spice terminal
then do ext2spice cthresh 0 rthesh 0
check if spice was created in place you just were in cmd
in format drain gate source substrate
then make file look like
<img width="1143" height="609" alt="Screenshot 2025-07-21 085853" src="https://github.com/user-attachments/assets/abd6b493-9275-45d2-b32a-bf07e00a2108" />
run nspice '+' NAMEOFYOURFILE
can plot using plot y vs time a
<img width="407" height="343" alt="Screenshot 2025-07-21 090404" src="https://github.com/user-attachments/assets/406ff9b1-3cb9-413b-8fc8-f117a7bd6e54" />

## Important files and how to view them
Navigate to the right directory and the use ls to find files of interest. Than use less to look at them. Example of what a less file might look at

<img width="698" height="468" alt="Screenshot 2025-07-17 061307" src="https://github.com/user-attachments/assets/afcc27bd-f6d0-4d66-a2bb-646ff18cc198" />

To view the floorplan cd to floorplan dir which is after picorv32a/runs/[your run]/results/ . Then run' magic -T /home/vsduser/Desktop/work/tools/openlane_working_dir/pdks/sky130A/libs.tech/magic/sky130A.tech lef read../../tmp/merged.lef def read picorv32a.floorplan.def
<img width="875" height="906" alt="Screenshot 2025-07-17 094650" src="https://github.com/user-attachments/assets/a2a5aad6-a01d-433e-9c36-28a1bedf8a91" />
<img width="1043" height="1077" alt="Screenshot 2025-07-23 232012" src="https://github.com/user-attachments/assets/3f452d52-00b1-4ec6-9625-5f3552d021a1" />

### Skywater
can look at layers on side and select different ones.
Use what tell what the highlighted portion is
connection between layers press s three times
via is a drevied layer
draw large area of contact with p key or paint
cif sec VIA2 represent mask layer 
measure with key b or box
make copy of role *poly short hand
DRC check to make sure nothing wrong was changed
challenging error need boolan operators applyed in sequence it is an error

to download lab files wget http://opencircuitdesign.com/open_pdks/archive/drc_tests.tgz
extract tar xfz drc_tests.tgz
<img width="2381" height="1501" alt="Screenshot 2025-07-21 112823" src="https://github.com/user-attachments/assets/7217b0ba-4721-46a5-8333-90a363a235c9" />
<img width="678" height="510" alt="Screenshot 2025-07-21 112230" src="https://github.com/user-attachments/assets/3d563cf8-abbc-4950-acbb-1df641258a89" />
<img width="788" height="512" alt="Screenshot 2025-07-30 091652" src="https://github.com/user-attachments/assets/c6f4ce8a-3d1f-471c-8cbf-4b69ffe8b423" />
<img width="759" height="505" alt="Screenshot 2025-07-30 090539" src="https://github.com/user-attachments/assets/d66566d7-14ba-45f8-a995-fe64f41a663a" />
<img width="955" height="662" alt="Screenshot 2025-07-30 090512" src="https://github.com/user-attachments/assets/34dcfb1a-0061-4535-a654-c32f53683c72" />

tracks are where the metal is
#### Helpful tools for skywater
http://opencircuitdesign.com/magic/
https://skywater-pdk.readthedocs.io/en/main/
### ngspice
plot y vs time a


To zoom in on image right than left mouse click then press z. Hoover over and press s to select object. Then type 'what into selection window and it will show layer.


