# Introduction to the module system 

!!! note "Objectives"

    - Learn the basics of the module system which is used to access most of the software at the majority of the HPC centres in Sweden 
    - Try some of the most used commands for the module system:
         - find/list software modules
         - load/unload software modules
   
Most programs are accessed by first loading them as a ‘module’.

Modules are:

- used to set up your environment (paths to executables, libraries, etc.) for using a particular (set of) software package(s)
- a tool to help users manage their Unix/Linux shell environment, allowing groups of related environment-variable settings to be made or removed dynamically
- allows having multiple versions of a program or package available by just loading the proper module
- are installed in a hierarchial layout. This means that some modules are only available after loading a specific compiler and/or MPI version, etc. 
    - that is, the modules have *prerequisites* that needs to be loaded before they can be loaded. 
    - Whether or not modules have prerequisites vary by center. More on this later! 

!!! warning "Important" 

    Take care not to use any system-installed versions of ``gcc``, ``python``, etc. Always use the module instead, when available! 




