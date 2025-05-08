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

## Unload software modules

## module show 


<img src="../images/shell-logo_small.png"> Exercise
