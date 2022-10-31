### What is a debugger
- A program that allows you to view the execution state and data (values in registers) of a program that allows you to find errors and fix bugs

### Source level vs assembly level
- Source level --> allows you to step through the source code, set breakpoints and examine variable states
- Assembly Level --> does the same as a source level debugger by operates on assembly code

### Kernel vs User level
- Kernel --> Debug at the kernel level, user has unrestricted access to system resources 
- User level --> allows you to debug in the user space

### Debugging symbols
- Contains info in locations of variables and functions in the binary file
- Specified during compile time
- Can be introduced as part of the binary or as a separate file 