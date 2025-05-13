# Examples

Loading modules works the same whether the modules are toolchains or standalone packages, with and without prerequisites. 

In the previous section we looked at toolchains, which are often used to install or build your own software. 

In this chapter we will look at some examples of how to find and load some of the typical software modules that are installed at many of the Swedish HPC centres. 


## Python-based packages

It varies between centres how many packages are installed with the base Python packages, how many are installed as separate modules, what prerequsites are required (most centers apart from PDC are the same), and the available version numbers. 

!!! note 

    - HPC2N: Little is installed with the Python module, but instead most of the common Python packages are available as extra modules (SciPy-bundle, mpi4py, matplotlib, tensorflow, ...)
    - UPPMAX: The base Python module contains most Python packages. A few ML specific packages are available in ML_python_packages(_cpu, _gpu) 
    - LUNARC: Little is installed with the Python module, but instead most of the common Python packages are available as extra modules (SciPy-bundle, mpi4py, matplotlib, tensorflow, ...). Anaconda3 bundles more into one moduleu (SciPy, Pandas, Matplotlib, etc), but doesn't incorporate other modules as well unless they are installed in a custom environment.
    - C3SE: 
    - NSC: 
    - PDC:

!!! important

    Several of the most commonly used Python modules are bundled in SciPy-bundle and thus will not be found with `ml spider`, among them: NumPy, Pandas, ... 


Example 1: Matplotlib

Matplotlib is technically a standalone package requiring only GCC, Python, and sometimes an extra GUI support package like Tkinter. However, for all practrical purposes, it requires at least NumPy or Pandas to create or read in any data. **NumPy and Pandas are part of the SciPy-bundle**, which require a BLAS library (usually OpenBLAS and/or FlexiBLAS) and some version of LAPACK (usually ScaLAPACK). Therefore, it is usually better to load a full `foss` toolchain.

Example 2: MPI4Py



## R-based packages

Example: Bioconductor(?)

## Matlab

At most HPC centers, Matlab can be loaded directly.

Example:

## Specialized Applications

