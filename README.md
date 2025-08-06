# Xschem-stdcell-subcircuits - HeiChips2025

The current version of the standard cells in IHP-Open-PDK do not have a visual representation of the transistor level schematic
of the cell although spice level netlist extists and can be found [here](https://github.com/IHP-GmbH/IHP-Open-PDK/blob/main/ihp-sg13g2/libs.ref/sg13g2_stdcell/spice/sg13g2_stdcell.spice). 
The goal of this tutorial (repository) is to provide instructins and a space for delivering such schematics using `xschem`. 

This tutorial will walk you through eleboration of schematic and symbol of a simple `nand2` gate.

## Netilist entry of nand2 gate

Open the [file](https://github.com/IHP-GmbH/IHP-Open-PDK/blob/main/ihp-sg13g2/libs.ref/sg13g2_stdcell/spice/sg13g2_stdcell.spice) and search for nand2 gate. 
You should find names `sg13g2_nand2_1` and `sg13g2_nand2_2`. Let's focus on the first one which has the following architecure: 

```
* Library name: sg13g2_stdcell
* Cell name: sg13g2_nand2_1
* View name: schematic
.subckt sg13g2_nand2_1 Y A B VDD VSS
XP1 Y B VDD VDD sg13_lv_pmos w=1.12u l=130.00n ng=1 ad=3.808e-13 as=3.808e-13 pd=2.92e-06 ps=2.92e-06 m=1
XP0 Y A VDD VDD sg13_lv_pmos w=1.12u l=130.00n ng=1 ad=3.808e-13 as=3.808e-13 pd=2.92e-06 ps=2.92e-06 m=1
XN1 net1 B VSS VSS sg13_lv_nmos w=740.00n l=130.00n ng=1 ad=2.516e-13 as=2.516e-13 pd=2.16e-06 ps=2.16e-06 m=1
XN0 Y A net1 VSS sg13_lv_nmos w=740.00n l=130.00n ng=1 ad=2.516e-13 as=2.516e-13 pd=2.16e-06 ps=2.16e-06 m=1
.ends
* End of subcircuit definition.
```
Each entry between `.subckt` and `.ends` clauses describe an element. Considering the first element `XP1` is an instance name (`X` means it is an subcircuit), the section `Y B VDD VDD` means that it is a four terminal subcircuit and the terminal mapping is the following (1->Y, 2->B, 3->VDD, 4->VDD) where Y, B, VDD are names of nets/wires. 
The section `sg13_lv_pmos` denominates subcircuit name followed by a list of parameters. 
Following this convention we can state that there are four instances  `XP1`, `XP0`, `XN1`, `XN0` of two subcircuits `sg13_lv_pmos` and `sg13_lv_nmos` interconected using some nets.wires named `Y, B, VDD, VSS` and `net1`. Also `Y, B, VDD, VSS` are not only wires but also input/output terminals. The `net1` net means an internal node.  

## Drawing the schematic in xschem based on the netlist entry

Open `xschem` by executing `xschem &` command in the terminal. Got to `File-Create new window/tab` menu and create new empty viwew. Save it as `nand2_1.sch` in a location where you have a write access (`/headless` in the IIC-OSIC-TOOLS docker image)
From the main menu click the insert symbol icon marked in red to introduce new symbol.
<img width="830" height="95" alt="image" src="https://github.com/user-attachments/assets/4dbdf140-cf67-44ee-a2c8-9301b679e55d" />
Once the `Choose symbol` window new appears select the following options:
<img width="1066" height="582" alt="image" src="https://github.com/user-attachments/assets/1235009e-ebc3-4a72-80c3-8ed6692b1e65" />
This will redirect you to the library of symbols of IHP-Open-PDK where we have to find `sg13_lv_nmos` and `sg13_lv_pmos` devices. Once selected (one by one)
you can place it on the schematic. 

<img width="479" height="536" alt="image" src="https://github.com/user-attachments/assets/6e82c56a-da9b-4f9e-aaa9-34a1be4f90a1" />

Since we need 2 PMOS and 2 NMOS devices we have to make a copy. To do so click on the device once and then pres `c` what makes a copy (move the cursor to place it).
Once having all four devices move it (`m` enables move) it in the position you want (arrange a view of a nand gate from a textbook). 
Another step is to size the devices providing the values of `w`, `l`, `ng` and `m`, which correspond to the values given in the netlist. 
You can do so selecting a device pressing `q` in order to enter a property window. 

<img width="631" height="628" alt="image" src="https://github.com/user-attachments/assets/580e5e63-95e8-4690-b23f-375eb52153bf" />

Next step is to add pins `A, B, Y` (we will create a symbols that uses VSS and VDD power rails by default so it does not explicity exposes power rails). 
To do so select `Insert symbol` icon to add two `ipin` and  one `opin` from the default xschem library. 

<img width="1059" height="577" alt="image" src="https://github.com/user-attachments/assets/1c921416-344e-4802-bef3-19f0ee52846f" />

It is necessary to change the labels of the IO pins. You can do it by marking it an pressing `q` to enter properties window. 

<img width="1013" height="478" alt="image" src="https://github.com/user-attachments/assets/ed2f11df-05c5-4eb0-922c-d0bf0abd8c26" />

Change the inputs to `A` an `B` and assign `Y` to output pin. 

<img width="848" height="629" alt="image" src="https://github.com/user-attachments/assets/22a0af4e-2a4a-4728-bd4a-48a394adaedc" />

The next step is to wire the circuit. You can do pressing `w` key and laying the wire (use `Esc` to finsish or another `w` to change orientation). 

<img width="551" height="661" alt="image" src="https://github.com/user-attachments/assets/61b25e7e-9da7-4a9b-b329-a0dc8447c463" />

The last step is to assign names to the power straps. To do so click to `Insert symbol` icon and insert 2 lab `lab_pin` elements. 
Connect it to VDD and VSS lines and change the names (by pressing `q`) to `VDD` and `VSS` respectively. 

<img width="561" height="670" alt="image" src="https://github.com/user-attachments/assets/4b4f5605-beec-4ad3-bb50-079e76b64468" />

Save the file. 

## Making a symbol using xschem

In `Xschem` from the menu `Symbol->Make symbol from schematic`. The symbol was created and saved in the same location as the schematic with the same name 
`nand2_1.sym`. Open it using `File->Open menu` item.

<img width="1619" height="282" alt="image" src="https://github.com/user-attachments/assets/75fec88b-89b4-490e-bb72-8f05d7e0ecf8" />

Since the geometry does not represent a typical nand gate we have to change it. Xschem provides primitives to draw different geometries but we will reuse 
the symbols already existing in the PDK. To do so open new file  `sg13g2_stdcell library`

<img width="1057" height="571" alt="image" src="https://github.com/user-attachments/assets/1fb6649a-4bc3-4460-950b-5c2e76676785" />


Once done select all the geometry and copy it to the nand2_1.sym file. 

<img width="1012" height="444" alt="image" src="https://github.com/user-attachments/assets/3ac2125f-b1e5-4a09-8cea-41c35a8ffbec" />

Remove the terminals from the copied symbol and the rectangle 

<img width="916" height="398" alt="image" src="https://github.com/user-attachments/assets/d5a3074e-20ab-45ef-bfbb-1ce4198a6218" />

Move the ramaining ports to the nand gate symbol and move the geometry to the center. 

<img width="469" height="299" alt="image" src="https://github.com/user-attachments/assets/d50a7fcd-8193-4afc-aae9-f53eecf7f3a6" />

Save file.

In order to enable the power rails to be connected to the gate using standard VSS and VDD names we have to modify the symbol in the text mode. 
Open `nand2_1.sym` in a text editor and focus on the lines
```
K {type=subcircuit
format="@name @pinlist @symname"
template="name=x1"
}
```
We have to modify extend it adding the following:

```
K {type=subcircuit
format="@name @pinlist @VDD @VSS @symname"
template="name=x1 VDD=VDD VSS=VSS"
extra="VDD VSS"
}
```
Save the file

This way you shoudld have a schematic and a corresponding symbol of a `nand2_1` cell ready to be instantiated. 
