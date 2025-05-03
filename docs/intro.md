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

Many of the centres in Sweden are using the <a href="https://lmod.readthedocs.io/en/latest/" target="_blank">Lmod</a> module system. It is an environment module system that is Lua based. 

Many, but not all, of the centres in Sweden are using the <a href="https://docs.easybuild.io/" target="_blank">EasyBuild</a> framework for building and installing software modules.   

!!! warning "Important" 

    Take care not to use any system-installed versions of ``gcc``, ``python``, etc. Always use the module instead, when available! 

## Useful commands 

This is a list of some of the most useful commands for the module system. In the list below, ``MODULE`` is used as a stand-in for any software module. 

- See which modules exists: ``module spider or ml spider``
- See which versions exist of a specific module: ``module spider MODULE`` or ``ml spider MODULE``
- See prerequisites and how to load a specfic version of a module: ``module spider MODULE/VERSION`` or ``ml spider MODULE/VERSION``
- List modules depending only on what is currently loaded: ``module avail`` or ``ml av``
- See which modules are currently loaded: ``module list`` or ``ml``
- Loading a module: ``module load MODULE`` or ``ml MODULE``
- Loading a specific version of a module: ``module load MODULE/VERSION`` or ``ml MODULE/VERSION``
- Unload a module: ``module unload MODULE`` or ``ml -MODULE``
- Get more information about a module: ``ml show MODULE`` or ``module show MODULE``
- Unload all modules except the ‘sticky’ modules: ``module purge or ml purge``

### Finding existing modules 

!!! warning 

    This differs somewhat betweem centres. Some centres recommend using ``moudle avail`` while at others it is better to use ``module spider``! 

    This is mainly to do with whether or not modules in general have prerequisite modules that needs loading before, or not. 

    - ``module spider``: HPC2N, LUNARC, C3SE, PDC  
    - ``module avail``: UPPMAX, NSC 

#### With ``module spider``

#### With ``module avail`` 

