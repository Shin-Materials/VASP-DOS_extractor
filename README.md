# VASP_DOS_extractor
------------------
### Simple description of this code:
Pymatgen-based python script to extract density of states (DOS) and projected DOS (pDOS) data from vasprun.xml file.
  
### What does this do:
This script intends to simplify the plotting process of DOS from [VASP](http://cms.mpi.univie.ac.at/vasp/) output, which is a density functional theory (DFT) calculation program. I used to use [p4vasp](http://www.p4vasp.at/#/) for this purpose, but since the program became out of service, the alternatives are typically difficult to use for beginners.   
**A typical process with VASP-related packages involves following:**
  - Download xml file to local computer **(often over 100 MB)**
  - Requires learning specific syntax for each package
  - Required usage of **built-in** plotting tool, which is difficult to customize for publication quality
  
Instead of this hassle, the DOS-extractor script exports data file in array form (**typically less than 1MB**), which is **ready to plot in python plotting script**.
(Note that you should install pymatgen (http://pymatgen.org/) before using this script.)

------------------------------------
### With DOS_extractor.py, these four process can be done with one command line:
 ```
  $ python DOS_extractor.py [xml_filename] [out_filename] [entries_or_options]
 ```
- [xml_filename]: name of vasprun.xml file.
- [out_filename]: name of DOS data file. By default it follows the printing format of ***p4vasp***. For spin polarized system, spin up/down 
  - p4vasp format: This format is collection of data arrays. Each data array represents each entry, including total DOS, which are composed of two columns; column1 for energy and column2 for DOS of entry. line1 is header starting with #, which shows the Band gap, and list of data in the output file.
  - block format: This format is intuitive structured data with row of energy grid and column of each entry. The line1 contains labels.
- [entries]: Entry can be element or specific atom. Atom label follows the [VESTA](http://jp-minerals.org/vesta/en/), for example atoms in BaTiO3 unit cell would be Ba1, Ti1, O1, O2, O3. The script also supports orbital projection by specifiying orbital after dash (-). Examples and nomenclatures are as follows.
  - Ex) Fe: DOS of all Fe atoms
  - Ex) Fe-d: d-orbitals of Fe
  - Ex) Fe-dxy: dxy-orbital of Fe 
  - Ex) Fe1: DOS of Fe1 atom
  - Ex) Fe1-dxy: dxy-orbital of Fe1 atom
  - Ex) O-px: px-orbital of O
  - Note that these nomenclatures are basically the order in which the orbitals are reported in VASP and has no special meaning.
    - p-orbitals: px, py, pz
    - d-orbitals: dxy, dyz, dz2, dxz, dx2
    - f-orbitals: f_3, f_2, f_1, f0, f1, f2, f3
- [options]: option can be stated with --. At this moment there are three options. 
  - --elements: Include all elements in the system to the entries.
  - --atoms: Include all individual atoms in the system to the entries.
  - --block: Change printing option to block data. Otherwise the printing option will follow p4vasp.

------------------------------------
### Example case 
For example, if you want to extract PDOS of all elements in Sr2Fe2O5, and Fe1 atom, you might do the command as follows.</br>
  ```
  $ python DOS_extractor.py vasprun.xml Sr2Fe2O5.dat Sr Fe O Fe1-d
  ```
**Corresponding output file is as follows (basically same with p4vasp format)**</br>
![alt text](https://github.com/why-shin/VASP-DOS_extractor/blob/master/Example1_p4v_format.png?raw=true)

  ```
  $ python DOS_extractor.py vasprun.xml Sr2Fe2O5_block.dat --elements Fe1-d --block
  ```
**Output file will contain same data as above, but with block data format (Note that '--elements' substitutes Sr Fe O command in this case)**</br>
![alt text](https://github.com/why-shin/VASP-DOS_extractor/blob/master/Example2_block_data_format.png?raw=true)
 
