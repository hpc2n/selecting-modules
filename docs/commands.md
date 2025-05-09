# Module system commands 

!!! note "Objectives"

    - Find software module versions
    - Load a module (with potential prerequisites):
         - default (often not recommended!)
         - a specific version of software
    - Unload software modules, including specific versions 
    - Unload all software modules with ``module purge``
        - This should never be done at PDC! 
        - Also not generally recommended at UPPMAX. 
    - List the loaded modules 
    - Get some information about a module  

In the previous section we saw how to find out which modules are available at a centre, including looking for a specific software module.    

Now it is time to learn more useful module commands! 

## Finding software versions 

If you just load a module without specifying the version, you will (depending on centre) get the default version or an error. 

There are good reasons not to just load the default version, even when it is possible: 

- The default version is often the newest one installed, which means the default changes the next time an even newer version is installed. This could break your code or at least give a different result, which is bad for resproducibility. 
- Sometimes you actually need a specific version, which could differ from the one the centre has decided on as default. 

So how do you find out which versions are available for a specific software? 

!!! note 

    This is again done differently, depending on the center you are at: 

    - HPC2N, LUNARC, C3SE, PDC: ``module spider MODULE`` or ``ml spider MODULE``
    - UPPMAX, NSC: ``module avail MODULE`` or ``ml avail MODULE`` 

### With ``module spider`` 

Recommended at HPC2N, LUNARC, C3SE, and PDC. 

!!! note

    The command to find the available versions for a software module named MODULE is: 

    ```bash
    module spider MODULE
    ```

    or, in short form 

    ```bash
    ml spider MODULE
    ``` 

### With ``module avail`` 

Recommended at UPPMAX and NSC. 

!!! note

    The command to find the available version for a software module named MODULE is: 

    ```bash 
    module avail MODULE
    ```

    or, in short form 

    ```bash 
    ml av MODULE
    ``` 

### Example 

Finding available versions of Python. 

!!! tip 

    Type along!

**module spider**

=== "HPC2N" 

    ```bash
    module spider Python
    ```

    ??? note "Click to show" 

        ```bash
        b-an01 [~]$ module spider Python

        --------------------------------------------------------------------------------------------------------------------
          Python:
        --------------------------------------------------------------------------------------------------------------------
            Description:
              Python is a programming language that lets you work more quickly and integrate your systems more effectively.

             Versions:
                Python/2.7.15
                Python/2.7.16
                Python/2.7.18-bare
                Python/2.7.18
                Python/3.7.2
                Python/3.7.4
                Python/3.8.2
                Python/3.8.6
                Python/3.9.5-bare
                Python/3.9.5
                Python/3.9.6-bare
                Python/3.9.6
                Python/3.10.4-bare
                Python/3.10.4
                Python/3.10.8-bare
                Python/3.10.8
                Python/3.11.3
                Python/3.11.5
                Python/3.12.3
             Other possible modules matches:
                Biopython  Boost.Python  Brotli-python  GitPython  IPython  Python-bundle-PyPI  bx-python  flatbuffers-python  ...

        --------------------------------------------------------------------------------------------------------------------
          To find other possible module matches execute:

              $ module -r spider '.*Python.*'

        --------------------------------------------------------------------------------------------------------------------
          For detailed information about a specific "Python" package (including how to load the modules) use the module's full name.
          Note that names that have a trailing (E) are extensions provided by other modules.
          For example:

             $ module spider Python/3.12.3
        --------------------------------------------------------------------------------------------------------------------
        ```

=== "LUNARC"

    ```bash
    module spider Python
    ```

    ??? note "Click to show"

        ```bash
        [bbrydsoe@cosmos3 ~]$ module spider Python

        ----------------------------------------------------------------------------------------------------------------------------
          Python:
        ----------------------------------------------------------------------------------------------------------------------------
            Description:
              Python is a programming language that lets you work more quickly and integrate your systems more effectively.

             Versions:
                Python/2.7.18-bare
                Python/2.7.18
                Python/3.8.6
                Python/3.9.5-bare
                Python/3.9.5
                Python/3.9.6-bare
                Python/3.9.6
                Python/3.10.4-bare
                Python/3.10.4
                Python/3.10.8-bare
                Python/3.10.8
                Python/3.11.3
                Python/3.11.5
                Python/3.12.3
             Other possible modules matches:
                Biopython  GitPython  IPython  Python-bundle  Python-bundle-PyPI  bx-python  flatbuffers-python  graphviz-python  ...

        ----------------------------------------------------------------------------------------------------------------------------
          To find other possible module matches execute:

              $ module -r spider '.*Python.*'

        ----------------------------------------------------------------------------------------------------------------------------
          For detailed information about a specific "Python" package (including how to load the modules) use the module's full name.
          Note that names that have a trailing (E) are extensions provided by other modules.
          For example:

             $ module spider Python/3.12.3
         ----------------------------------------------------------------------------------------------------------------------------
        ```

=== "C3SE" 

    ```bash
    module spider Python
    ```

    ??? note "Click to show"

        ```bash
        [brydso@alvis1 ~]$ module spider Python

        -----------------------------------------------------------------------------------------------------------------------------
          Python:
        -----------------------------------------------------------------------------------------------------------------------------
            Description:
              Python is a programming language that lets you work more quickly and integrate your systems more effectively.

             Versions:
                Python/2.7.18-GCCcore-11.2.0-bare
                Python/2.7.18-GCCcore-11.3.0-bare
                Python/2.7.18-GCCcore-12.2.0-bare
                Python/3.9.5-GCCcore-10.3.0-bare
                Python/3.9.5-GCCcore-10.3.0
                Python/3.9.6-GCCcore-11.2.0-bare
                Python/3.9.6-GCCcore-11.2.0
                Python/3.10.4-GCCcore-11.3.0-bare
                Python/3.10.4-GCCcore-11.3.0
                Python/3.10.8-GCCcore-12.2.0-bare
                Python/3.10.8-GCCcore-12.2.0
                Python/3.11.3-GCCcore-12.3.0
                Python/3.11.5-GCCcore-13.2.0
                Python/3.12.3-GCCcore-13.3.0
             Other possible modules matches:
                Biopython  Boost.Python  CUDA-Python  GitPython  IPython  Python-bundle-PyPI  flatbuffers-python  meson-python  ...

        -----------------------------------------------------------------------------------------------------------------------------
          To find other possible module matches execute:

              $ module -r spider '.*Python.*'

        -----------------------------------------------------------------------------------------------------------------------------
          For detailed information about a specific "Python" package (including how to load the modules) use the module's full name.
          Note that names that have a trailing (E) are extensions provided by other modules.
          For example:

             $ module spider Python/3.12.3-GCCcore-13.3.0
        -----------------------------------------------------------------------------------------------------------------------------
        ```

**module spider, mostly**
    
=== "PDC"

    ```bash
    module spider python
    ```

    ??? note "Click to show - python"

        ```bash
        bbrydsoe@login1:~> ml spider python
        -----------------------------------------------------------------------------------------------------------------------------
          python:
        -----------------------------------------------------------------------------------------------------------------------------
             Versions:
                python/2.7.6
                python/2.7.9
                python/2.7.11
                python/2.7.15
                python/3.3
                python/3.3.1
                python/3.4.3
                python/3.5.0
                python/3.6.0
                python/3.6.8
                python/3.7.2
                python/3.8.7
                python/3.9.5
                python/3.9.6
                python/3.10.8
                python/3.11.4
                python/3.11.6-gcc-oc3
                python/3.11.6-gcc-qyw
                python/3.11.6-gcc-spv
                python/3.11.8
                python/3.11.9-gcc-epm
                python/3.12.1
                python/3.12.3
                python/3.13.0-gcc-cgo
             Other possible modules matches:
                biopython  cray-python  dbus-python  pdc-python  py-meson-python  py-python-dateutil  python-2.7.18-gcc-11.2.0-namldiz  python-3.8.12-gcc-11.2.0-nwj732o  python-3.8.12-gcc-11.2.0-vhhulma  ...

        -----------------------------------------------------------------------------------------------------------------------------
          To find other possible module matches execute:

              $ module -r spider '.*python.*'

        -----------------------------------------------------------------------------------------------------------------------------
          For detailed information about a specific "python" package (including how to load the modules) use the module's full name.
          Note that names that have a trailing (E) are extensions provided by other modules.
          For example:

             $ module spider python/3.13.0-gcc-cgo
        -----------------------------------------------------------------------------------------------------------------------------
        ```

    ??? note "Click to show - cray-python"

        ```bash 
        bbrydsoe@login1:~> module avail cray-python

        ------------------------------------ /opt/cray/pe/lmod/modulefiles/core -------------------------------------
           cray-python/3.10.10    cray-python/3.11.5 (D)    cray-python/3.11.7

          Where:
           D:  Default Module

        If the avail list is too long consider trying:

        "module --default avail" or "ml -d av" to just list the default modules.
        "module overview" or "ml ov" to display the number of modules for each name.

        Use "module spider" to find all possible modules and extensions.
        Use "module keyword key1 key2 ..." to search for all possible modules matching any of the "keys".
        ```

**module avail** 

=== "UPPMAX" 

    ```bash 
    module avail python
    ``` 

    ??? note "Click to show" 

        ```bash
        [bbrydsoe@rackham1 ~]$ module avail python

        --------------------------------- /sw/mf/rackham/applications ----------------------------------
           python_GIS_packages/3.10.8      python_ML_packages/3.9.5-gpu         wrf-python/1.3.1
           python_ML_packages/3.9.5-cpu    python_ML_packages/3.11.8-cpu (D)

        ----------------------------------- /sw/mf/rackham/compilers -----------------------------------
           python/2.7.6     python/3.4.3    python/3.9.5         python3/3.6.0     python3/3.11.4
           python/2.7.9     python/3.5.0    python/3.10.8        python3/3.6.8     python3/3.11.8
           python/2.7.11    python/3.6.0    python/3.11.4        python3/3.7.2     python3/3.12.1
           python/2.7.15    python/3.6.8    python/3.11.8        python3/3.8.7     python3/3.12.7 (D)
           python/3.3       python/3.7.2    python/3.12.1        python3/3.9.5
           python/3.3.1     python/3.8.7    python/3.12.7 (D)    python3/3.10.8

          Where:
           D:  Default Module

        Use "module spider" to find all possible modules and extensions.
        Use "module keyword key1 key2 ..." to search for all possible modules matching any of the "keys".
        ```

        Ignore the comment about using ``module spider``! 

=== "NSC"

    ```bash 
    module avail python 
    ``` 

    ??? note "Click to show" 

        ```bash 
        [x_birbr@tetralith3 ~]$ module avail python

        --------------------- /software/sse2/tetralith_el9/modules ---------------------
           ASE/3.22.1-hpc1-python
           ASE/3.23.0-hpc1-python
           GaussSum/3.0.2-Python-3.7.8-hpc1
           NCL/6.6.2-hpc2-Python
           Pylint/3.2.5-hpc1-Python-3.11.5
           Python/recommendation                (D)
           Python/2.7.18-bare-hpc1-gcc-2022a-eb
           Python/3.10.4-bare-hpc1-gcc-2022a-eb
           Python/3.10.4-env-hpc1-gcc-2022a-eb
           Python/3.10.4-env-hpc2-gcc-2022a-eb
           Python/3.11.5-bare-hpc1-gcc-2023b-eb
           Python/3.11.5-env-hpc1-gcc-2023b-eb

          Where:
           D:  Default Module
        ```

!!! note "<img src="../images/shell-logo_small.png"> Exercise" 

    1. At your chosen centre, check the different output for ``module avail Python`` and ``module spider Python``
    2. Do you get the same output for "Python" and "python"? 

## List loaded modules 

There is a very useful command to list which modules you have loaded. 

It is 

``module list``

or, in short form 

``ml``

!!! tip 

    Try it now! 

With nothing loaded, only the "sticky" modules are listed. They are modules that are needed for the environment to function correctly, so do not remove them! 

## Load software module 

In order to load modules, we need to know *how*. This differs by center only inasmuch as some centers have prerequisites (usually **compiler toolchains**) for most of the software modules. 

The centers that recommend ``module spider`` for finding the software modules generally have prerequisites (HPC2N, LUNARC, C3SE, PDC), while those who recommend ``module avail`` generally do not (UPPMAX, NSC). 

Thus; if the centers suggests you do ``module spider`` you will need to check the required prerequisites to load. 

### Prerequisites 

- This is done with ``module spider <module>/<version>`` 
- You can also do this at the centers recommending ``module avail``, but that will generally just tell you the module can be loaded directly. 

!!! note "Example, HPC2N" 

    Finding out how to load Python, version 3.12.3 

    ```bash 
    b-an01 [~]$ module spider Python/3.12.3

    --------------------------------------------------------------------------------------------------------------------
      Python: Python/3.12.3
    --------------------------------------------------------------------------------------------------------------------
        Description:
          Python is a programming language that lets you work more quickly and integrate your systems more effectively.


        You will need to load all module(s) on any one of the lines below before the "Python/3.12.3" module is available to load.

          GCCcore/13.3.0
 
        This module provides the following extensions:

           flit_core/3.9.0 (E), packaging/24.0 (E), pip/24.0 (E), setuptools/70.0.0 (E), setuptools_scm/8.1.0 (E), tomli/2.0.1 (E), typing_extensions/4.11.0 (E), wheel/0.43.0 (E)

        Help:
          Description
          ===========
          Python is a programming language that lets you work more quickly and integrate your systems more effectively.
      
      
          More information
          ================
           - Homepage: https://python.org/
      
      
          Included extensions
          ===================
          flit_core-3.9.0, packaging-24.0, pip-24.0, setuptools-70.0.0, setuptools_scm-8.1.0, tomli-2.0.1, typing_extensions-4.11.0, wheel-0.43.0
    ```
   
    There are some things to pay attention to here: 

    - You are told the prerequisites are "GCCcore/13.3.0"
    - After loading that, you can load the module itself, "Python/3.12.3" 
    - You are told about the extensions that are included. Here, this would be the Python packages that are included, which is not very many. HPC2N and some of the other centres use separate modules or *module bundles* (SciPy-bundle for instance) for any extensions/packages that you might need. 

!!! note "Example, UPPMAX" 

    Finding out how to load Python, version 3.11.8 

    ```bash 
    [bbrydsoe@rackham1 ~]$ ml avail python/3.11.8

    ----------------------------------- /sw/mf/rackham/compilers -----------------------------------
       python/3.11.8

    Use "module spider" to find all possible modules and extensions.
    Use "module keyword key1 key2 ..." to search for all possible modules matching any of the "keys".
    ``` 

    This did not tell us much! Let us try ``module spider`` 

    ```bash
    [bbrydsoe@rackham1 ~]$ module spider python/3.11.8

    --------------------------------------------------------------------------------------------
      python: python/3.11.8
    --------------------------------------------------------------------------------------------

        This module can be loaded directly: module load python/3.11.8

        Help:
      
          Python - use python/3.11.8
        
              Version 3.11.8
      
              https://python.org
      
          This module was built with 
      
              gcc/12.3.0 sqlite/3.34.0 Tcl-Tk/8.6.11
      
      
          This module provides the executable names 'python' and 'python3'.
      
          Several additional python packages are also installed in this module. The complete list of packages in this module, produced using 'pip list', is:
      
          Package                   Version
          ------------------------- ---------------
          anndata                   0.10.5.post1
          anyio                     4.2.0
          argon2-cffi               23.1.0
          argon2-cffi-bindings      21.2.0
          array_api_compat          1.4.1
          arrow                     1.3.0
          asteval                   0.9.31
          asttokens                 2.4.1
          async-lru                 2.0.4
          ... 
    ```

    Here we learn: 

    - You can load "python/3.11.8" directly
    - It was built with "gcc/12.3.0 sqlite/3.34.0 Tcl-Tk/8.6.11" (might be useful for compatibility with other things). 
    - It lists all the extensions (here Python packages) loaded with it - and it is a lot, around 250 packages. 

### Loading 

To load a software module, do: 

- (if needed) ``module load <prerequisite>/<suitable version>``
- ``module load <module>/<compatible version>``

and the versions you got from ``module spider``. 

Again, ``ml load <module>/<version>`` can be used as a short form. 

When you have loaded the module, you can see that your list of loaded modules has changed. This is done with ``module list`` or ``ml``. 

!!! hint 

    Type along! 

!!! note "Example, HPC2N"

    Loading Python 3.12.3 and prerequisites, and checking before and after which modules are loaded.

    ```bash
    module list
    ```

    ??? note "Click to show output!" 

        ```bash
        b-an01 [~]$ module list

        Currently Loaded Modules:
          1) snicenvironment (S)   2) systemdefault (S)

          Where:
           S:  Module is Sticky, requires --force to unload or purge
        ```
  
    ```bash
    module load GCCcore/13.3.0 Python/3.12.3
    ```

    or load them on separate lines 

    ```bash
    module load GCCcore/13.3.0
    module load Python/3.12.3
    ```

    What is the advantage to loading them one at a time? You can then easier find compatible modules that depend on that version, using ``module avail``. 

    ??? note "Click to show output!" 

        ```bash
        b-an01 [~]$ module load GCCcore/13.3.0 Python/3.12.3
        b-an01 [~]$
        ```

    And now do ``module list`` again! 

    ??? note "Click to show output!" 

        ```bash 
        b-an01 [~]$ module list

        Currently Loaded Modules:
          1) snicenvironment (S)   4) zlib/1.3.1      7) ncurses/6.5      10) SQLite/3.45.3  13) OpenSSL/3
          2) systemdefault   (S)   5) binutils/2.42   8) libreadline/8.2  11) XZ/5.4.5       14) Python/3.12.3
          3) GCCcore/13.3.0        6) bzip2/1.0.8     9) Tcl/8.6.14       12) libffi/3.4.5

          Where:
           S:  Module is Sticky, requires --force to unload or purge
        ```
    
!!! note "Example, NSC" 

    Loading Python version 3.11.5 (the module is called Python/3.11.5-env-hpc1-gcc-2023b-eb and can be loaded directly). Also do ``module list`` before and after! 

    ```bash
    module list
    ```

    ??? note "Click to show output!" 

        ```bash
        [x_birbr@tetralith3 ~]$ module list

        Currently Loaded Modules:
          1) hpc/.1.10.1 (H,S)

          Where:
           S:  Module is Sticky, requires --force to unload or purge
           H:             Hidden Module
        ```
        
    ```bash
    module load Python/3.11.5-env-hpc1-gcc-2023b-eb
    ```

    ??? note "Click to show output!" 

        ```bash
        [x_birbr@tetralith3 ~]$ module load Python/3.11.5-env-hpc1-gcc-2023b-eb
        [x_birbr@tetralith3 ~]$ 
        ```

    ```bash
    ml
    ```

    ??? note "Click to show output!" 

        ```bash
        [x_birbr@tetralith3 ~]$ ml

        Currently Loaded Modules:
          1) hpc/.1.10.1 (H,S)   2) Python/3.11.5-env-hpc1-gcc-2023b-eb

          Where:
           S:  Module is Sticky, requires --force to unload or purge
           H:             Hidden Module
        ```

!!! warning "Important"

    - You can do several ``module load`` on the same line. Or you can do them one at a time, as you want.
        - The modules have to be loaded in order! You cannot list the prerequisite after the module that needs it!
    - One advantage to loading modules one at a time is that you can then find compatible modules that depend on that version easily.
        - Example: you have loaded GCC/13.2.0 and Python/3.11.5 at HPC2N or LUNARC. You can now do ``ml av`` or ``module avail`` to see which versions of other modules you want to load, say ``SciPy-bundle``, are compatible. 
        - If you know the name of the module you want, you can even start writing ``module load SciPy-bundle/`` (for instance) and press TAB - the system will then autocomplete to the compatible one(s). 

### Loading several modules

There are situations where you need to load several modules, even at centres where there are no prerequisites. 

!!! note "Example" 

    You need some Python packages not included in the base Python you loaded, for instance TensorFlow or maybe AlphaFold. 

    You now need to load a version of that module which is compatible with the Python you want. 
   
    === "HPC2N"

        Tensorflow needs Python and SciPy-bundle. SciPy-bundle also needs OpenMPI. TensorFlow *loads* Python and SciPy-bundle itself, but you need their prerequisites: 

        ```bash 
        b-an01 [~]$ module spider TensorFlow/2.15.1-CUDA-12.1.1

        --------------------------------------------------------------------------------------------------------------------
          TensorFlow: TensorFlow/2.15.1-CUDA-12.1.1
        --------------------------------------------------------------------------------------------------------------------
            Description:
              An open-source software library for Machine Intelligence


            You will need to load all module(s) on any one of the lines below before the "TensorFlow/2.15.1-CUDA-12.1.1" module is available to load.

              GCC/12.3.0  OpenMPI/4.1.5
 
            This module provides the following extensions:

               absl-py/2.1.0 (E), astor/0.8.1 (E), astunparse/1.6.3 (E), cachetools/5.3.3 (E), google-auth-oauthlib/1.2.0 (E), google-auth/2.29.0 (E), google-pasta/0.2.0 (E), gviz-api/1.10.0 (E), keras/2.15.0 (E), Markdown/3.6 (E), oauthlib/3.2.2 (E), pyasn1-modules/0.4.0 (E), requests-oauthlib/2.0.0 (E), rsa/4.9 (E), tblib/3.0.0 (E), tensorboard-data-server/0.7.2 (E), tensorboard-plugin-profile/2.15.1 (E), tensorboard/2.15.2 (E), tensorflow-estimator/2.15.0 (E), TensorFlow/2.15.1 (E), termcolor/2.3.0 (E), Werkzeug/3.0.2 (E), wrapt/1.14.1 (E)
        ```

        ```bash 
        b-an01 [~]$ module load GCC/12.3.0 OpenMPI/4.1.5
        b-an01 [~]$ module load TensorFlow/2.15.1-CUDA-12.1.1
        ```

    === "UPPMAX" 

        TensorFlow is included in the Python_ML_packages. Python gets loaded with the Python_ML_packages so nothing extra needs loading here 

        ```bash
        [bbrydsoe@rackham1 ~]$ ml spider python_ML_packages/3.11.8-cpu

        --------------------------------------------------------------------------------------------
          python_ML_packages: python_ML_packages/3.11.8-cpu
        --------------------------------------------------------------------------------------------

            This module can be loaded directly: module load python_ML_packages/3.11.8-cpu

            Help:
              python_ML_packages version 3.11.8-cpu
        ```    
        
        ```bash 
        [bbrydsoe@rackham1 ~]$ ml python_ML_packages/3.11.8-cpu
        [bbrydsoe@rackham1 ~]$
        ``` 

        However, if you for instance need Matlab together with Gurobi, then you need to load both modules: 

        ```bash 
        [bbrydsoe@rackham1 ~]$ ml spider matlab/R2023b
        [bbrydsoe@rackham1 ~]$ ml Gurobi/11.0.3
        ``` 

## Unload software modules

Aside from ``module unload`` this section will also cover ``module purge``! 

!!! note 

    Why would you want to unload a module?

    - If you want to use a different version of a module, you need to unload the current one first 
    - If you need a different version of a dependent module, but it is not compatible with the current prerequisite you have loaded 
  
Modules can be unloaded with: 

- ``module unload <MODULE>`` **may or may not work**
- ``module unload <MODULE>/<version>``
- ``ml unload <MODULE>/<version>`` **short form of the above**
- ``module -<MODULE>/<version>`` 
- ``ml -<MODULE>/<version>`` **short form of the above** 
- ``ml -<MODULE>/<version>`` **short form of the above**

Unloading a module **will not unload the prerequisites**. 

This is not usually a problem at the centres where there rarely are prerequisites (UPPMAX, NSC), but can be annoying at centres who usually have prerequisites for the modules (HPC2N, LUNARC, C3SE, PDC). However, there the easy solution is ``module purge`` (HPC2N, LUNARC, C3SE). 

The command ``module purge`` removes all the loaded modules, except the "sticky" modules. It is recommended at HPC2N, LUNARC, and C3SE. 

!!! warning 

    Do not use ``module purge`` at PDC! 

    At PDC, there are a lot of necessary, "system-modules" that are preloaded. When you do ``module purge`` they will also be unloaded and things may not work as it should! 

    At UPPMAX, the system-module "uppmax" will get unloaded with ``module purge``, but can easily be reloaded with ``module load uppmax``. 

### Unloading examples 

Unloading one module, with no prerequisites (for clarity, we also do ``module list`` before and after to show what is happening. 

!!! tip 

    Type along! 







## module show 


<img src="../images/shell-logo_small.png"> Exercise
