DAY 4
#converting info to track
##spice notes
track are for every metal layer
ports need to be on insrection of horizontal and vertical tracks
press g for grid and see if they intersect
width of standard cell, whole multiple of the x width
port does mean anything to magic
extract lef file then ports are what the pins are
power ground atttach to metal 1
a any y have to atach to local
have define purpuse of each port
using port class output
port use signal

#Commands
First
in tkcon main window grid 0.46um 0.34um 023um 0.17um then save sky130_vsdinv.mag
magic -T sky130_vsdinv.mag &
write lef '''creates a left file for given file

then find place of lef file and cp <file_name> <location of something/picorv32a/src/
then have to change config.tcl

then edit config file to look right like the following/
want to use both lef files
so package require openlane 0.9
can add -tag(keep going with) -overwrite to prep -design picorv32a along with 2 .lef files

have to correct slack voilatons 

#delay tables
assumtions each node driving same load and that c1=c2=c3=c3=25F
indical buffer at same level. 

the load at the output varies it is not constant(capacitance)
verying input transations different delays
#delay table
output load and input slew was collected for each buffer
one buffer of fixed size and threshold voltage delay was caluclated for each and every cell.
each kind of gate
buffer size 1 mean p mos and nmos of size 2 defined size that is different.
change size change resistance
use it like a look up table
build equation to find values for given problem
delay find of first cell than find it of second cell
##usage
want to calculate the skrew
output of buffer goes into next buffer but need second transition table for one going into another
in example skrew is 0. branching logi. but if buffer and trunk of tree because it is varying the delay values 
would be different meaning different delay frequency. meaning non 0 skrew value
the small differene at one level will grow to a very big problem
# lab steps
wns is all the slack
what to strike balance between chip area and delay
can make changes like set $::env(SYNTH_STRATEGY)
line between area and time
can view chip area in synth then slack decreases
run_placement look at OVFL want to be close to 0
do same magic command as early to look at chip placement from different day
#timing analysis
T = time(clock period)
theta = combinational delay
flip flop have mux meaning delay
mux1 delay is set up time for it to settle
then becomes theata<(T-S)
clocks not perfect meaning jitter
set up time analizej
now clock delay theta<(T-S-SU)
max set up time delay has to be all wire delays and each module delay
# timeing anaylze
look at them in different
need to look up capacinty and then change cap load based on it.
read sdc file 
find pin A and Cap load
change the cap value in the config file
vim pre_sta.conf 
config where timing analysis is being done
sta pre_sta.conf
delay of cell is inmport slew and capacinenty of output
set ::even(SYNTH_MAX_FANOUT)
report_net -connections _14635
run_synthesis again. SLack has dropped again
help replace cell
report_checks -fields {net cap slew input_pins} -digits 4

replace_cell _44322_ sky130_fd_sc_hd_buf_4
report_checks -fields {net cap slew input_pins} -digits 4

report_check -from _50144_ -to _50075_ -through _44322_
need to make slack a postive value
report_tns
report_wns
# clock tree synthesis
time difference between reaching point is t2-t1=skew
has to be value close to 0
edge tree take distance to all other points 
then pick mid point between them
do midpoint strategy almost similar skew value close to 0
reachs through repeaters
nee buffers and they have delay
## clock net sharing
two pormblems 
glich logic high on reset pin and reset mem
shielding breaking coupling caps
impact of crosstalk delta delay skrew
thats why shield clock nets
## sttup timing
delta 1 to rerach from clk end to launch flop. clk edge to capture flop is delta 2
delta1-delta2 abslute value is screw
needs to be in equation (theat+delta1)<(T+delta2)-S-SU
data requried time is right hand side of <. while other side is data arrival time
not voilting able to work
slack should be +ve or '0'
theta > H meaning 
hold is data in capture flop until sent out of capture flop
theat+1+2>H+1+3+4
changes to thata+delta1>H+delta2+HU
want uncertainty as a low value. 
right is data rquired time and left is data arrival time.
## openroad
timing anaylis
Day 5
<img width="1178" height="438" alt="Screenshot 2025-07-26 210646" src="https://github.com/user-attachments/assets/1aa966dc-1fa8-4d99-88c6-46b14d6b2eb5" />
<img width="944" height="445" alt="Screenshot 2025-07-26 191101" src="https://github.com/user-attachments/assets/1cf06bd2-cbaf-4126-9620-3b84b5728ce6" />
<img width="962" height="355" alt="Screenshot 2025-07-26 184744" src="https://github.com/user-attachments/assets/01847d73-e6b5-49c0-bdfa-7bcdd36db625" />
