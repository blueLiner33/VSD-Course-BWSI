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
Aspect Ratio= Height of chip/ Width of chip
Logic system>floor_planning>placement>CTS>Routing

## Placement
The parts of chips like 'and', flip flops are represented by blocks or rectanglers on a chip. Based on the diagram put the blocks close to each other when connected. Then close to pins that are relvant to the blocks. To have signal intergerty need something to help signal reach the block, use repeaters(buffers). Based on distance inside of chip. Ones with high frequency place close togther. After check placement. Want to try to keep them close togther.

## Clocks 
Last stage Static Timing analysis is the last step and tells the max frequency.
All blocks need to recieve the clock at the same time.

# Commands
Logic of commands is important most have to be run one after another.

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

## Important files and how to view them
Navigate to the right directory and the use ls to find files of interest. Than use less to look at them. Example of what a less file might look at

<img width="698" height="468" alt="Screenshot 2025-07-17 061307" src="https://github.com/user-attachments/assets/afcc27bd-f6d0-4d66-a2bb-646ff18cc198" />

To view the floorplan cd to floorplan dir which is after picorv32a/runs/[your run]/results/ . Then run' magic -T /home/vsduser/Desktop/work/tools/openlane_working_dir/pdks/sky130A/libs.tech/magic/sky130A.tech lef read../../tmp/merged.lef def read picorv32a.floorplan.def
Should look like
<img width="875" height="906" alt="Screenshot 2025-07-17 094650" src="https://github.com/user-attachments/assets/a2a5aad6-a01d-433e-9c36-28a1bedf8a91" />

To zoom in on image right than left mouse click then press z. Hoover over and press s to select object. Then type 'what into selection window and it will show layer.
