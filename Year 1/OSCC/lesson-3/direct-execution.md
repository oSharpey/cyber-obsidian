# Direct Execution


## What is direct execution
- Running the program directly on the CPU


## Running a process with direct execution
 
**(OS)**
- OS will create a new entry in the process list
- OS will allocate memory to store instructions, static data etc.
- The program will be loaded into main memory
- The stack will be set up --> one entry for each function called
- The bottom of the stack (first entry) will contain the main() function
- Process will now be in 'ready' state ready to be executed by the CPU
- OS will clear the registers 
- Execute call to main() function

**(Process)**
