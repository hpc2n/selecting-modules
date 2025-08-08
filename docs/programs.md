# Examples

!!! note "Objectives" 

    - Be able to find some typical software modules that are installed 
    - Learn how to load those software modules (and any prerequisites)
    - Be able to find out if Python packages are installed as their own modules
    - Learn how to find bundles of R packages installed as modules 

In the previous section we looked at toolchains, which are often used to install or build your own software. In this chapter we will look at some examples of how to find and load some of the typical software modules that are installed at many of the Swedish HPC centres. 

Loading modules works the same whether the modules are toolchains or standalone packages, with and without prerequisites. The procedure is usually some variation on the following:

1. Use `ml spider <package>` to see whether a module is installed, and if so, view the available versions. (At facilities that do not hide packages depending on prerequisites, `module avail <package>` may be preferred.)
2. Use `ml spider <package>/<version>` to view prerequisites for a specific version of the module, if any.
3. Load the modules with `ml <prerequisite>/<version> <package>/<version>`.

Most of the examples below use outputs from Cosmos, but the workflows will be similar at other institutions except where otherwise noted.

!!! important

    Both R and Python packages often include many so-called extensions (dependent packages that are usually called modules when not dealing with LMOD modules) that are installed, but cannot be found with `ml spider` or `ml avail`.
    
    In these cases, if you have loaded at least the prerequisites of a standalone package containing the extension, you can use `ml show <package>` on the standalone package to view the Lua module file, which usually has a section on included extensions near the top.
    
    For example, NumPy, SciPy, and Pandas, among other packages, are all are included in `SciPy-bundle`. If you have at least loaded a GCC version, you can use `ml show SciPy-bundle` to view all of the included extensions (Python modules) in the compatible SciPy-bundle.

!!! tip
    
    `grep` does not work directly on the outputs of module commands like `ml show <module>`. To search for an extension in a very long Lua module file, copy the full path of the .lua file from the `ml show` output, and use `less /path/to/module.lua | grep <extension>`. If there is no output, the extension is not present.

## Python-based packages

It varies between centres how many packages are installed with the base Python packages, how many are installed as separate modules, what prerequsites are required (most centers apart from PDC are the same), and the available version numbers. 

!!! note 

    - **HPC2N:** Little is installed with the Python module, but most of the common Python packages are available as extra modules (SciPy-bundle, Jupyter, mpi4py, matplotlib, tensorflow, PyTorch, Python-bundle-PyPi, ...)
    - **LUNARC:** Little is installed with the Python module, but instead most of the common Python packages are available as extra modules (SciPy-bundle, Jupyter, mpi4py, matplotlib, tensorflow, PyTorch, Python-bundle-PyPi, ...). Anaconda3 bundles more into one module (SciPy, Pandas, Matplotlib, etc), but does not incorporate other modules as well unless they are installed in a custom environment.
    - **C3SE:** Little is installed with the Python module, but most of the common Python packages are available as extra modules (SciPy-bundle, matplotlib, mpi4py, PyTorch, Python-bundle-PyPi, Jupyter, Horovod) 
    - **NSC:** Little is installed with the Python module, but most of the common Python packages are available as extra modules (SciPy-bundle, matplotlib, mpi4py, PyTorch, Python-bundle-PyPi, Jupyter, ...). Most programs on Tetralith also have an extra prerequisite, `buildtool-easybuild/4.8.0-hpc<version>` that must be loaded before anything else.  
    - **PDC:** most modules included in SciPy-bundle (NumPy, SciPy, Pandas, etc.) are in `cray-python` modules, and these are compatible with a couple of the installed versions of matplotlib. Python modules that do not include the `cray-` prefix have very little installed in them and are not compatible with most other Python-adjacent modules; these are typically intended as bases for users to build their own environments. Most programs on Dardel also have an extra prerequisite, `PDC/XX.XX` or `PDCOLD/XX.XX` that must be loaded before anything else.

### Example 1: Matplotlib

Matplotlib prerequisites vary significantly across HPC centers: some require none, some need one, some need two, and in some cases Matplotlib is only an extension of another module ([more info on how to find Matplotlib at different HPC centers here](https://uppmax.github.io/HPC-python/day2/matplotlib-intro.html#load-and-run)). A good starting point is to view the output of `ml spider matplotlib` (or `ml avail matplotlib` on NSC), pick an arbitrary version, and view `ml spider matplotlib/<version>`.

All of the following code blocks in this example are taken from Cosmos.

```bash
$ ml spider matplotlib

----------------------------------------------------------------------------
  matplotlib:
----------------------------------------------------------------------------
    Description:
      matplotlib is a python 2D plotting library which produces publication
      quality figures in a variety of hardcopy formats and interactive
      environments across platforms. matplotlib can be used in python
      scripts, the python and ipython shell, web application servers, and
      six graphical user interface toolkits.

     Versions:
        matplotlib/2.2.5-Python-2.7.18
        matplotlib/3.3.3
        matplotlib/3.4.2
        matplotlib/3.4.3
        matplotlib/3.5.2
        matplotlib/3.7.0
        matplotlib/3.7.2
        matplotlib/3.8.2
        matplotlib/3.9.2

----------------------------------------------------------------------------
```

If you try the above command at your local HPC center and get a "not found" error, that probably means Matplotlib is an extension of another module (e.g. on Rackham, it is part of the base Python module).

Let us look at `matplotlib/3.8.2`, for example:

```bash
$ ml spider matplotlib/3.8.2

---------------------------------------------------------------------------------
  matplotlib: matplotlib/3.8.2
---------------------------------------------------------------------------------
    Description:
      matplotlib is a python 2D plotting library which produces publication
      quality figures in a variety of hardcopy formats and interactive
      environments across platforms. matplotlib can be used in python scripts,
      the python and ipython shell, web application servers, and six graphical
      user interface toolkits.


    You will need to load all module(s) on any one of the lines below before the "mat
plotlib/3.8.2" module is available to load.

      GCC/13.2.0
```

This means that, on Cosmos, only GCC must be loaded before Matplotlib. However, Matplotlib is mostly unusable without the tools needed to read in or create the data arrays, so NumPy and/or Pandas are necessary. At most facilities, that means SciPy-bundle is also required.

Note that `ml show matplotlib/<version>` does **not** show which Python version is associated with that version of Matplotlib. However, once `GCC` is loaded, you can use `ml avail` with `Python`, `matplotlib`, and/or `SciPy-bundle` to see which versions of these are available to load. Usually there is only 1 version of each per GCC version, but if there are more, the version that will load if you do not specify a version number is the one with the (D) next to it. 

If you want to move code developed on a personal laptop to the cluster, you will typically only be constrained to a Python version (use `python --version` to check), or more practically, a *range* of versions Python/X.Y.Z in which X absolutely *must* match what you used, Y *should* match but may be flexible by one or rarely two versions, and Z is usually not important.

Say you built a Matplotlib-based script using Python 3.11.8 on your own laptop. Glob patterns do not work to filter `ml spider` or `ml avail`, so it is necessary to view the full list with `ml spider Python` (`ml spider cray-python` on Dardel). Here is the output on Cosmos:

```bash
$ ml spider Python
---------------------------------------------------------------------------------
  Python:
---------------------------------------------------------------------------------
    Description:
      Python is a programming language that lets you work more quickly and integrate
your systems more effectively.

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
        Biopython  GitPython  IPython  Python-bundle  Python-bundle-PyPI
        bx-python  flatbuffers-python  graphviz-python  meson-python  ...
```

The closest result is `Python/3.11.5` (though probably anything from 3.10.x to 3.12.x would work). Let us check what that requires:

```bash
$ ml spider Python/3.11.5
---------------------------------------------------------------------------------
  Python: Python/3.11.5
---------------------------------------------------------------------------------
    Description:
      Python is a programming language that lets you work more quickly and
      integrate your systems more effectively.


    You will need to load all module(s) on any one of the lines below before the
    "Python/3.11.5" module is available to load.

      GCCcore/13.2.0
 
    Help:
      
      Description
      ===========
      Python is a programming language that lets you work more quickly and integrate 
your systems more effectively.
      
      
      More information
      ================
       - Homepage: https://python.org/
      
      
      Included extensions
      ===================
      flit_core-3.9.0, packaging-23.2, pip-23.2.1, setuptools-68.2.2, setuptools-
      scm-8.0.4, tomli-2.0.1, typing_extensions-4.8.0, wheel-0.41.2
```

On this cluster, the base Python module requires GCCcore, but we already saw that Matplotlib requires GCC (whch GCCcore is part of). In fact, nearly every other Python-based module apart from the bare Python itself requires GCC, so you may as well use GCC every time.

On some facilities, each version of Matplotlib and SciPy-bundle is only be associated with one Python version, so you can load them all at once, using the GCC version to select for everything else, like this:

```bash
ml GCC/13.2.0 Python matplotlib SciPy-bundle
```

However, this is considered bad practice since sometimes additional versions are installed later. We should instead check `ml avail` to see what versions of Matplotlib and Scipy-bundle we can load, like so:


```bash
$ ml avail Scipy-bundle

----------------------------- /sw/easybuild_milan/modules/all/Compiler/GCC/13.2.0 -----------------------------
   SciPy-bundle/2023.11
```

(Note: some output omitted for brevity)

```bash
$ ml avail Matplotlib

----------------------------- /sw/easybuild_milan/modules/all/Compiler/GCC/13.2.0 -----------------------------
   matplotlib/3.8.2
```

Then the one-line loading command should look like this:

```bash
ml GCC/13.2.0 Python/3.11.5 matplotlib/3.8.2 SciPy-bundle/2023.11
```

If we check what was loaded with `ml` or `module list`, the output looks like this: 

```bash
$ ml

Currently Loaded Modules:
  1) SoftwareTree/Milan         (S)  26) expat/2.5.0
  2) GCCcore/13.2.0                  27) util-linux/2.39
  3) zlib/1.2.13                     28) fontconfig/2.14.2
  4) binutils/2.40                   29) xorg-macros/1.20.0
  5) GCC/13.2.0                      30) libpciaccess/0.17
  6) bzip2/1.0.8                     31) X11/20231019
  7) ncurses/6.4                     32) Tk/8.6.13
  8) libreadline/8.2                 33) Tkinter/3.11.5
  9) Tcl/8.6.13                      34) NASM/2.16.01
 10) SQLite/3.43.1                   35) libjpeg-turbo/3.0.1
 11) XZ/5.4.4                        36) jbigkit/2.1
 12) libffi/3.4.4                    37) gzip/1.13
 13) OpenSSL/1.1                     38) lz4/1.9.4
 14) Python/3.11.5                   39) zstd/1.5.5
 15) OpenBLAS/0.3.24                 40) libdeflate/1.19
 16) FlexiBLAS/3.3.1                 41) LibTIFF/4.6.0
 17) FFTW/3.3.10                     42) giflib/5.2.1
 18) cffi/1.15.1                     43) libwebp/1.3.2
 19) cryptography/41.0.5             44) OpenJPEG/2.5.0
 20) virtualenv/20.24.6              45) LittleCMS/2.15
 21) Python-bundle-PyPI/2023.10      46) Pillow/10.2.0
 22) pybind11/2.11.1                 47) Qhull/2020.2
 23) libpng/1.6.40                   48) matplotlib/3.8.2
 24) Brotli/1.1.0                    49) SciPy-bundle/2023.11
 25) freetype/2.13.2

  Where:
   S:  Module is Sticky, requires --force to unload or purge
```

If you are comfortable editing code in a basic text editor and running at the command-line, the modules used in the example above are all you need. For more information on choosing and loading IDEs to work with Matplotlib graphics interactively, we refer readers to [this documentation from the Python for HPC course](https://uppmax.github.io/HPC-python/day2/IDEs.html).

### Example 2: MPI4Py

Here is an example of what `ml spider MPI4Py` would output (note that `ml spider` and `ml avail` are only case-sensitive when the module version number is included):

```bash
$ ml spider MPI4Py

---------------------------------------------------------------------------------
  mpi4py:
---------------------------------------------------------------------------------
    Description:
      MPI for Python (mpi4py) provides bindings of the Message Passing Interface
      (MPI) standard for the Python programming language, allowing any Python
      program to exploit multiple processors.

     Versions:
        mpi4py/3.1.4
        mpi4py/3.1.5
        mpi4py/4.0.1

---------------------------------------------------------------------------------
  For detailed information about a specific "mpi4py" package (including how to load t
he modules) use the module's full name.
  Note that names that have a trailing (E) are extensions provided by other modules.
  For example:

     $ module spider mpi4py/4.0.1
---------------------------------------------------------------------------------
```

!!! note

    UPPMAX does not currently have mpi4py installed, per [their documentation page on parallel programming in Python](https://docs.uppmax.uu.se/software/python_parallel_jobs/#mpi).

Here again, you will typically only know which Python version you need and/or which GCC version to load, not which version of `mpi4py` you need. However, since the previous example showed how to work from a known Python version, this example will show a workflow based on working backwards from the latest version of a module.

In the above output from Cosmos, the latest version is `mpi4py/4.0.1`, so here is how to view its dependencies:

```bash
$ ml spider mpi4py/4.0.1

---------------------------------------------------------------------------------
  mpi4py: mpi4py/4.0.1
---------------------------------------------------------------------------------
    Description:
      MPI for Python (mpi4py) provides bindings of the Message Passing Interface
      (MPI) standard for the Python programming language, allowing any Python
      program to exploit multiple processors.


    You will need to load all module(s) on any one of the lines below before the "mpi
4py/4.0.1" module is available to load.

      GCC/13.3.0  OpenMPI/5.0.3
 
    Help:
      
      Description
      ===========
      MPI for Python (mpi4py) provides bindings of the Message Passing Interface (MPI
) standard for
       the Python programming language, allowing any Python program to exploit multip
le processors.
      
      
      More information
      ================
       - Homepage: https://github.com/mpi4py/mpi4py
      
      
      Included extensions
      ===================
      mpi4py-4.0.1
```

Now we can just copy and paste the dependencies, and then load the main module, like so: 

```bash
$ ml GCC/13.3.0  OpenMPI/5.0.3
$ ml mpi4py/4.0.1
```

Or you could combine them into one line, and omit the `mpi4py` version since the prerequisites make only 1 version available, as in `ml GCC/13.3.0  OpenMPI/5.0.3  mpi4py`.

Now, if we check what was loaded, we see the following:

```bash
$ ml

Currently Loaded Modules:
  1) SoftwareTree/Milan  (S)  10) hwloc/2.10.0      19) bzip2/1.0.8
  2) GCCcore/13.3.0           11) OpenSSL/3         20) ncurses/6.5
  3) zlib/1.3.1               12) libevent/2.1.12   21) libreadline/8.2
  4) binutils/2.42            13) UCX/1.16.0        22) Tcl/8.6.14
  5) GCC/13.3.0               14) libfabric/1.21.0  23) SQLite/3.45.3
  6) numactl/2.0.18           15) PMIx/5.0.2        24) libffi/3.4.5
  7) XZ/5.4.5                 16) PRRTE/3.0.5       25) Python/3.12.3
  8) libxml2/2.12.7           17) UCC/1.3.0         26) mpi4py/4.0.1
  9) libpciaccess/0.18.1      18) OpenMPI/5.0.3

  Where:
   S:  Module is Sticky, requires --force to unload or purge
```

As you can see, on the cluster where this code was run, Python was loaded automatically.

However, note that no BLAS or LAPACK libraries are loaded, which might be unexpected given the types of problems MPI is typically used for. These libraries are available through either a `foss` toolchain or a `SciPy-bundle`, and in practice, you will almost always load one or both of those when using `mpi4py`.

### Example 3: Check if a Python extension is loaded

Extensions can be hard to find without knowing what includes them, but it is easy to check if modules that are already loaded added the extension silently. If you cannot find a package you want with `ml avail`, `ml spider`, or `ml show <module>`, you should also check `pip list` and `grep` for the package after loading the rest of your modules. 

For example, `psutil` is part of Python-bundle-PyPI, which is silently loaded with any SciPy-bundle. Here is the easiest way to find `psutil`:

```bash
$ pip list | grep psutil
psutil                            5.9.5

[notice] A new release of pip is available: 23.1.2 -> 25.1.1
[notice] To update, run: pip install --upgrade pip
```

The `pip list | grep` approach is also helpful if you want to see the version of a package without having to open a Python interpreter.

!!! tip

    The same list (and grep) approach works for Anaconda3. The only difference is that you use `conda list` instead of `pip list`. The Anaconda3 module file does **not** list the included extensions, so `conda list | grep <package>` is also the *only* way to see if a package is included without starting up a Python command line interface.

## R-based packages

At most HPC centers, the base R module usually contains relatively few extensions. Most of the popular packages are in additional bundles like `R-bundle-CRAN` and `R-bundle-Bioconductor`. Most HPC centers have prerequisites for R, but at a few, like Alvis, R can be loaded directly. Always check the prerequisites with `ml spider` or `ml avail`.

!!! note

    - **HPC2N:** Little is installed with the basic R module, but most common packages are available as extensions of R-bundle-CRAN, R-bundle-CRAN-extra, or R-bundle-Bioconductor. RStudio is a separate module and only runs on the login nodes via Thinlinc, so it should be used sparingly.
    - **LUNARC:** Little is installed with the basic R module, but most common packages are available as extensions of R-bundle-CRAN or R-bundle-Bioconductor. RStudio is also a separate module, and is available as an On-Demand application that automatically loads R and various bundles at start-up.
    - **UPPMAX (Rackham) and C3SE (Alvis):** R can be loaded directly, but has few installed packages. More common modules are available as extensions with the `R_packages` module. RStudio is also a separate module.
    - **NSC (Tetralith):** R can be loaded directly, but contains few installed packages, and there are no bundles to provide more. Users are typically expected to install their own extension libraries. RStudio is included in the base R module, however.
    - **PDC (Dardel):** Like most programs on Dardel, R also has the prerequisite `PDC/XX.XX` or `PDCOLD/XX.XX`, but the compiler and MPI library are chosen for you. There are about 250 packages available in the basic R module, and there are no additional bundles to provide more packages. Users are typically expected to install their own extension libraries. 

!!! important

    Most facilities have only built a handful of R releases, so many of the dependent modules are adapted for multiple versions. The version of such any dependent package should always be specified to ensure reproducibility. If the version number is omitted, the latest will be loaded by default, and that version may change without warning.

### Example 1: Bioconductor

Many R-packages conveniently specify the version of R they are compatible with in the module name. One example of this is Bioconductor.

```bash
$ ml spider bioconductor

---------------------------------------------------------------------------------
  R-bundle-Bioconductor:
---------------------------------------------------------------------------------
    Description:
      Bioconductor provides tools for the analysis and coprehension of
      high-throughput genomic data.

     Versions:
        R-bundle-Bioconductor/3.15-R-4.2.1
        R-bundle-Bioconductor/3.18-R-4.3.2
        R-bundle-Bioconductor/3.18-R-4.4.1
        R-bundle-Bioconductor/3.19-R-4.4.1

---------------------------------------------------------------------------------
  For detailed information about a specific "R-bundle-Bioconductor" package (includin
g how to load the modules) use the module's full name.
  Note that names that have a trailing (E) are extensions provided by other modules.
  For example:

     $ module spider R-bundle-Bioconductor/3.19-R-4.4.1
---------------------------------------------------------------------------------
```

Notice that in this case, there are 2 versions of the Bioconductor bundle associated with `R/4.4.1`, and that there are 2 versions of R associated with `R-bundle-Bioconductor/3.18`. Do not rely on the prerequisites to set which version of `R-bundle-Bioconductor` gets loaded.

To check the prerequisites with `ml spider`, the specific version number must be included anyway, for all software.

```bash
$ ml spider R-bundle-Bioconductor/3.18-R-4.4.1

---------------------------------------------------------------------------------
  R-bundle-Bioconductor: R-bundle-Bioconductor/3.18-R-4.4.1
---------------------------------------------------------------------------------
    Description:
      Bioconductor provides tools for the analysis and coprehension of
      high-throughput genomic data.


    You will need to load all module(s) on any one of the lines below before the "R-b
undle-Bioconductor/3.18-R-4.4.1" module is available to load.

      GCC/12.3.0  OpenMPI/4.1.5
 
    Help:
      
      Description
      ===========
      Bioconductor provides tools for the analysis and coprehension
       of high-throughput genomic data.
      
      
      More information
      ================
       - Homepage: https://bioconductor.org
      
      
      Included extensions
      ===================
      affxparser-1.74.0, affy-1.80.0, affycoretools-1.74.0, affyio-1.72.0,
      AgiMicroRna-2.52.0, agricolae-1.3-7, ALDEx2-1.34.0, ALL-1.44.0, ANCOMBC-2.4.0,
      annaffy-1.74.0, annotate-1.80.0, AnnotationDbi-1.64.1,
      AnnotationFilter-1.26.0, AnnotationForge-1.44.0, AnnotationHub-3.10.0,
      anytime-0.3.9, aroma.affymetrix-3.2.1, aroma.apd-0.7.0, aroma.core-3.3.0,
      aroma.light-3.32.0, ash-1.0-15, ATACseqQC-1.26.0, AUCell-1.24.0,
      aws.s3-0.3.21, aws.signature-0.6.0, babelgene-22.9, ballgown-2.34.0,
      basilisk-1.14.2, basilisk.utils-1.14.1, batchelor-1.18.1, baySeq-2.36.0,
      beachmat-2.18.0, BH-1.84.0-0, Biobase-2.62.0, BiocBaseUtils-1.4.0, ...
```

The list of extensions is too long to copy here, but some popular extensions included in this module are: DeSeq2, GenomeInfoDb, MStats, Seurat, Rsamtools, and more.

The above prerequisites and the main package can be loaded either one at a time or all at once with,

```bash
$ ml GCC/12.3.0  OpenMPI/4.1.5  R-bundle-Bioconductor/3.18-R-4.4.1
```

In this case, R-bundle-Bioconductor loads the version of R that it is based on automatically (along with about 130 other modules!). That is not the case for all R-bundles at all HPC centers, so pay attention to the prerequisites.

## Matlab

At most HPC centers, Matlab can be loaded directly, but a couple of centers require a basic software tree as a prerequisite. On Dardel (PDC), all Matlab versions, like nearly every other module, have a module called something like `PDC/xx.xx` or `PDCOLD/xx.xx` as a prerequisite. The example further down in this section shows how finding and determining the prerequisites for Matlab works at another such facility.

Capitalization and other naming conventions also vary between HPC centers; for more information, refer to [this section of the R, Matlab, and Julia for HPC course](https://uppmax.github.io/R-matlab-julia-HPC/matlab/load_runMatlab.html#check-for-matlab-versions).

**All Add-Ons and Toolboxes should be available through the Matlab GUI.**

!!! important

    The Matlab GUI is prone to hogging resources if not launched carefully, which makes it risky to run on a login node. In general, the GUI should only be run via either Desktop On Demand or after booking interactive allocations on compute nodes with `salloc` or `interactive`. For more particulars on *running* Matlab, see the [relevant page of the R, Matlab, and Julia for HPC course materials](https://uppmax.github.io/R-matlab-julia-HPC/matlab/load_runMatlab.html).

### Example: Matlab on Tetralith (NSC)

At most centers, where modules are hidden if prerequisites are not loaded, it is better to use `ml spider` to see what versions are available before accounting for preconditions. At centers where all modules are searchable without loading prerequisites, it may be better to use `ml avail` to avoid listing modules that only exist as extensions or aliases of other modules, as in the case of Tetralith at NSC:

```bash
$ ml avail matlab

--------------------- /software/sse2/tetralith_el9/modules ---------------------
   MATLAB/recommendation (D)    MATLAB/2023b-bdist
   MATLAB/2023a-bdist           MATLAB/2024a-hpc1-bdist

  Where:
   D:  Default Module
```

Once you have chosen a specific version, use `ml spider` to check if there are prerequisites, like so:

```bash
$ ml spider MATLAB/2024a-hpc1-bdist
```

The full output was too verbose to reprint in full here, but the one important line reads: 

```bash
    This module can be loaded directly: module load MATLAB/2024a-hpc1-bdist
```

The command after the colon (:) can be copied, pasted, and entered directly into the bash prompt to load the module, or you can type the short version as follows:

```bash
$ ml MATLAB/2024a-hpc1-bdist
```

## Specialized Applications

For most specialized packages (Amber, GROMACS, Nextflow, VASP, etc), unless there is reason to believe it is included in a larger package or you include a spurious non-alphanumeric character, `ml spider` will tell you whether it is installed or not. If the full name of a module includes `CUDA`, then the relevant `CUDA` version will typically be loaded automatically, without the need to choose a CUDA-containing toolchain.

!!! important

    Some specialized modules (e.g., Abaqus, Gaussian, and VASP) are license-restricted, and may not load, or may load but refuse to run, if you are not part of the licensed user group. If you run `ml spider` on a specific version of licensed software, the description may (as with VASP) or may not (as with Gaussian) specify that a license is required. It is encumbant on users to determine the licensing requirements of specialized software packages.

### Example: OpenFOAM

As usual, we start by checking the versions available with `ml spider OpenFOAM` (or `ml avail OpenFOAM` at a facility like NSC where all modules are visible regardless of the presence of prerequisites). 

An example output might look like this (from Cosmos):

```bash
$ ml spider OpenFOAM

--------------------------------------------------------------------------------------
  OpenFOAM:
--------------------------------------------------------------------------------------
    Description:
      OpenFOAM is a free, open source CFD software package. OpenFOAM has an extensive
      range of features to solve anything from complex fluid flows involving chemical
      reactions, turbulence and heat transfer, to solid dynamics and
      electromagnetics.

     Versions:
        OpenFOAM/v2112
        OpenFOAM/v2206
        OpenFOAM/v2306
        OpenFOAM/v2406
        OpenFOAM/7-20200508
        OpenFOAM/9
        OpenFOAM/10
        OpenFOAM/11

--------------------------------------------------------------------------------------
  For detailed information about a specific "OpenFOAM" package (including how to load the 
modules) use the module's full name.
  Note that names that have a trailing (E) are extensions provided by other modules.
  For example:

     $ module spider OpenFOAM/11
--------------------------------------------------------------------------------------
```

Let us look at a recent version:

```bash
$ ml spider OpenFOAM/11
--------------------------------------------------------------------------------------
  OpenFOAM: OpenFOAM/11
--------------------------------------------------------------------------------------
    Description:
      OpenFOAM is a free, open source CFD software package. OpenFOAM has an extensive
      range of features to solve anything from complex fluid flows involving chemical
      reactions, turbulence and heat transfer, to solid dynamics and
      electromagnetics.


    You will need to load all module(s) on any one of the lines below before the "OpenFOAM
/11" module is available to load.

      GCC/11.3.0  OpenMPI/4.1.4
 
    Help:
      Description
      ===========
      OpenFOAM is a free, open source CFD software package.
       OpenFOAM has an extensive range of features to solve
       anything from complex fluid flows involving chemical
       reactions, turbulence and heat transfer, to solid dynamics
       and electromagnetics.
      
      
      More information
      ================
       - Homepage: https://www.openfoam.org/
```

On this system, GCC and OpenMPI must be loaded first, but this is not true for every system. Indeed, some versions on some systems (e.g. at NSC) load and use the compilers, MPI libraries, and mathematics libraries from Intel toolchains.

Now we can load everything all at once like so:

```bash
$ ml GCC/11.3.0  OpenMPI/4.1.4 OpenFOAM/11
```

or load each one at a time. The above command loads almost 90 modules, including several Python packages and visualization libraries, all of which can be viewed by entering `ml`.
