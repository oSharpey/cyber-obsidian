# The 2 Different Types of OS (Kernel)

**[[the-kernel|The Kernel]]** -->The core part of the [[what-is-an-os|OS ]]

###### Microkernel
- The kernel itself does not perform many functions
- The ***user space*** is kept separate from the ***kernel space***
- The majority of things on the system are ran by kernel modules (device drivers, file manager)

###### Monolithic Kernel
- The operating system runs entirely in the kernel space
- The kernel controls the majority of functions on the system
- Mostly seen in IOT devices


![[micro-vs-mono.jpg]]


###### Advantages of Microkernel
- More easily optimised compared to monolithic kernel
- Can be made more secure as very little can actually access the kernel space

###### What is *User Space*
- Where modules and applications run
- Modules are able to talk directly to the kernel 