# Examples

Loading modules works the same whether the modules are toolchains or standalone packages, with and without prerequisites. 

In the previous section we looked at toolchains, which are often used to install or build your own software. 

In this chapter we will look at some examples of how to find and load some of the typical software modules that are installed at many of the Swedish HPC centres. 


!!! important

    It is common for both R and Python packages to include many so-called extensions (dependent packages that are usually called modules when not dealing with LMOD modules at the same time) that are installed but cannot be founfd with `ml spider` or `ml avail`. In these cases, if you have loaded at least the prerequisites of the standalone package that the extension works with, you can use `ml show <package>` on the standalone package to view the Lua module file, which often has a section on included extensions near the top. 
    For example, several of the most commonly used Python packages are included with SciPy in `SciPy-bundle`, such as NumPy, Pandas, and NumExpr. If you have at least loaded a GCC version, you can use `ml show SciPy-bundle` to view all of the included extensions (Python modules) in the compatible SciPy-bundle.


## Python-based packages

It varies between centres how many packages are installed with the base Python packages, how many are installed as separate modules, what prerequsites are required (most centers apart from PDC are the same), and the available version numbers. 

!!! note 

    - HPC2N: Little is installed with the Python module, but instead most of the common Python packages are available as extra modules (SciPy-bundle, Jupyter, mpi4py, matplotlib, tensorflow, PyTorch, Python-bundle-PyPi, ...)
    - LUNARC: Little is installed with the Python module, but instead most of the common Python packages are available as extra modules (SciPy-bundle, Jupyter, mpi4py, matplotlib, tensorflow, PyTorch, Python-bundle-PyPi, ...). Anaconda3 bundles more into one module (SciPy, Pandas, Matplotlib, etc), but does not incorporate other modules as well unless they are installed in a custom environment.
    - C3SE: Little is installed with the Python module, but instead most of the common Python packages are available as extra modules (SciPy-bundle, matplotlib, mpi4py, PyTorch, Python-bundle-PyPi, Jupyter, Horovod) 
    - NSC: 
    - PDC:

!!! tip

    NumPy and other extensions in the SciPy-bundle typicaly require a BLAS library (usually OpenBLAS and/or FlexiBLAS) and some version of LAPACK (usually ScaLAPACK). Therefore, it is usually better to load a full `foss` toolchain, instead of just GCC, if you need any of them. At HPC centers that install modules with EasyBuild, the SciPy-bundle module is usually configured to load the rest of the associated `foss` toolchain automatically. However, loading `foss` first loads both GCC and OpenMPI, which allows `ml avail` and `ml show` to find and show the details of many more modules with fewer extra steps.
    

Example 1: Matplotlib

Matplotlib is technically a standalone package requiring only GCC, Python, and sometimes an extra GUI support package like Tkinter. However, for all practical purposes, it requires at least NumPy or Pandas to create or read in any data, so **you will need to load a SciPy-bundle** (see Important note above).

If you are constrained by the GCC version, you typically don't need to look up and set the versions of any dependent modules.


Example 2: MPI4Py



## R-based packages

Example: Bioconductor(?)

## Matlab

At most HPC centers, Matlab can be loaded directly.

Example:

## Specialized Applications

