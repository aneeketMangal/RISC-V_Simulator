<br />
<p align="center">
    <img src="GUI/Images/logo.png" alt="Logo" width="580" height="200">

  <h3 align="center">RISC-V Simulator</h3>

  <p align="center">
    A python implementation of RISC-V Simulation.
    <br />
    <a href="https://github.com/tanmayaeron/RISC-V_Simulator/blob/phase4/Documentation/Phase4/Documentation.pdf"><strong>Explore the docs »</strong></a>
    
  </p>
</p>

<details open="open">
  <summary>Table of Contents</summary>
  <ol>
    <li>
      <a href="#about-the-project">About The Project</a>
      <ul>
        <li><a href="#contributors">Contributors</a></li>
      </ul>
    </li>
    <li>
      <a href="">Getting Started</a>
      <ul>
        <li><a href="#libraries-used">Libraries Used</a></li>
        <li><a href="#installation">Installation</a></li>
      </ul>
    </li>
    <li><a href="#instructions-to-use-gui">Usage</a></li>
  </ol>
</details>

## About The Project

The aim of this project is to simulate the machine level execution of RISC V as well as the execution of RISC-V 32-bit instructions using a high level language.. The Project also aims to give updates to the user regarding each step of the execution of the program.
It also returns the final status of the memory and registers as output for the user to analyse the working of their programs thoroughly. The Project currently allows the user to use 29 different instructions and can be extended to allow the use of any number of instructions by editing the .csv files as long as the instructions are supported by 32-bit RISC V ISA.
For each instruction the program gives various updates like IR, PC, decoded instruction, temporary registers like RA, RB, RZ, RY, etc. during each cycle and prints the number of cycles.
The program executes each instruction using five stages as described in the RISC V architecture.

## Getting Started

This is an example of how you may give instructions on setting up your project locally.
To get a local copy up and running follow these simple example steps.

### Prerequisites

- `pip` (>21.0.3)
- `python` (>3.7)

### Libraries Used

#### Back-end - Python3

- `os: ` for getting and adding path to certain file locations.
- `sys: ` for reading and editing files with ease.
- `json: ` for crunching generated data.
- `glob: ` for file management
- `regex: `for making the cleaned file by splitting the RISC-V instructions and removing comments
- `pandas:` for reading .csv files.
- `random: `for generating random number in Random Replacement Policy
- `argparse: `for taking arguments from the user
- `defaultdict: ` to make a hash map for memory.

#### Front-end - Python3

- `PyQT5: ` for the Graphic User Interface.
- `QtAwesome:` for using icons
- `qdarkstyle: ` for dark theme

### Installation

1. Clone the repository using git clone and open terminal in project directory.

2. Install required libraries using
   ```sh
   pip install -r requirements.txt
   ```
3. To run the GUI version enter
   ```sh
   python main.py -g
   ```
4. To run the non-GUI version
   ```sh
   python main.py
   ```

### Instructions to run using CLI

Following flags are present to configure simulator:

- `-g, -gui` enable GUI
- ` -h, --help` show this help message and exit

- `-k1, -knob1 ` enable Pipelining
- `-k2, -knob2 ` enable Data Forwarding
- `-k3, -knob3 ` show value in registerFile at end of each cycle
- `-k4, -knob4 ` show value in
- `-k5 K5, -knob5 K5 ` show value in Pipeline Registers at end of each cycle for particular instruction
- `-f F, -filename F` specify file which is to be run for non-GUI version
  Pipeline Registers at end of each cycle

- `-ICache cacheSize blockSize noOfWays` configure input cache in format cache size block size number of ways
- `-DCache cacheSize blockSize noOfWays` configure data cache in format cache size block size number of ways

> File should be present in the test directory.\
> Support for changing block replacement policy and branch predictor is not present for Non-GUI version currently.

### Instructions to use GUI

1.  Write the code you wish to run in the editor window.
2.  You may save the file using save button.
3.  Set the required knobs according to the documentation
4.  Set the details of cache in the control box.
5.  Tick the Machine Code button if the file is in Machine level of RISC-V.
6.  You may change the Inst Replacement Policy as well as the Data Replacement Policy by clicking on them. The Default is LRU.
7.  You may set branch predictor and initial state of predictor. The Default is static Always Taken predictor.
8.  To run the file using step function first load the file by pressing on the top right third (down arrow key) button and then click on the step button(last button).
9.  To run the file, press compile button. Once the code completes execution, a tick sign will be visible on the button.
10. In case you run the pipelined version the datapath can visualised using the "datapath" and "info" tab
11. You can get the info about data cache and instruction cache from respective tabs.
12. Look for the generated files

### Input Format

Input format of the Machine Code file instructions

- In text segment, data is word by word while in data segment it is byte by byte.
- Text segment followed by data segment demarcated by '$', each line is of format: "address data".
- Example:

  ```python
    0x0 0x00500513
    0x4 0x008000EF
    0x8 0x0440006F
    $
    0x10000000 0x64
  ```

Input format of the RISC-V instructions

- The instructions are RISC-V 32 bit instructions.
- For these instructions keep the Machine Code unchecked
- Example

  ```javascript
    .data
    array: .word 1 2 10 9 3 8 4 7 5 6

    .text
    auipc x11,0x10000 # x11=array.begin()
    addi x11 x11 0
    addi x12 x0,10 # x12=array.size()
    addi x31, x0,2
    bubble_sort:
    addi x12,x12,-1 #n-=1
    beq x12,x0,exit # if (n-1==0){break;}
    addi x13, x0,0 # x13= counter
    loop:
    beq x13,x12,break # if (i==n-1)
    sll x14,x13,x31 # x14= 4\* i
    add x14,x14,x11 # x14=&arr[i]
    lw x15,0(x14) # x15=arr[i]
    lw x16,4(x14) # x16=arr[i+1]
    addi x13,x13,1 #i++
    blt x15,x16,loop
    beq x15,x16,loop
    sw x16,0(x14) # arr[i]=arr[i+1]
    sw x15,4(x14) # arr[i+1]=arr[i] # thus swapped
    jal x0 loop
    break:

    jal x0 bubble_sort
    exit:
  ```

### Output Format

Check the generated folder for details of compilation. It contains:

- `stats.txt :` contains general stats about the code compilation.
- `memory.txt:` details of memory
- `register.txt:` details of registers
- `outputLog.txt:` details of changes in temporary registers for each cycle
- `forwarding.txt:` details of data forwarding paths taken in each cycle

### ScreenShots

<p align="center">
  <img src="Documentation/Phase4/images/FrontGUI.png" alt="Logo" width="1080" height="500">
</p>
<p align="center">
  <img src="Documentation/Phase4/images/RegistersPhase4.png" alt="Logo" width="1080" height="500">
</p>
<p align="center">
  <img src="Documentation/Phase4/images/MemoryPhase4.png" alt="Logo" width="1080" height="500">
</p>
<p align="center">
  <img src="Documentation/Phase4/images/Stats.png" alt="Logo" width="1080" height="500">
</p>

### Contributors

<div align="center">
  <strong>
    <a href="https://github.com/soggycake0312">Aditya Agarwal</a> &emsp;
    <a href="https://github.com/aneeketMangal">Aneeket Mangal</a> &emsp;
    <a href="https://github.com/HETFADIA">Het Fadia</a> &emsp;
    <a href="https://github.com/Shikhar-Soni">Shikhar Soni</a> &emsp;
    <a href="https://github.com/tanmayaeron">Tanmay Aeron</a> &emsp;
  </strong>
</div>
