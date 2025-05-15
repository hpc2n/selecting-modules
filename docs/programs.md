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
    - NSC: Little is installed with the Python module, but instead most of the common Python packages are available as extra modules (SciPy-bundle, matplotlib, mpi4py, PyTorch, Python-bundle-PyPi, Jupyter, ...). Most programs on Tetralith also have an extra prerequisite, `buildtool-easybuild/4.8.0-hpc<version>` that must be loaded before anything else.  
    - PDC: most modules included in SciPy-bundle (NumPy, SciPy, Pandas, etc.) are included with `cray-python` modules, and these are compatible with a couple of the installed versions of matplotlib. The Python modules that do not include the `cray-` prefix have very little installed in them and are not compatible with most other Python-adjacent modules; these are typically intended as bases for users to build their own environments. Most programs on Dardel also have an extra prerequisite, `PDC/XX.XX` or `PDCOLD/XX.XX` that must be loaded before anything else.

!!! tip

    For packages that require GCC, OpenMPI, and at least one of BLAS and LAPACK, it is a good idea to find and remember a `foss` toolchain that provides them all to save on typing and make more compatible modules visible to `ml avail`. Some modules with these dependencies are configured to load the rest of the toolchain automatically, but many do not. With `ml show foss/20YYr`, you can preview the module file for a particular release and see which versions of GCC, OpenMPI, Open/FlexiBLAS, and ScaLAPACK are loaded. It takes a bit of time to explore them since `ml avail` will not filter `foss` toolchains based on already loaded versions of GCC and OpenMPI, but it is a worthwhile investment.

### Example 1: Matplotlib

Matplotlib is technically a standalone package requiring only GCC, Python, and sometimes an extra GUI support package like Tkinter. However, for all practical purposes, it requires at least NumPy or Pandas to create or read in any data, so **you will need to load a SciPy-bundle** (see Important note above).

If you are constrained by an exact GCC version, you typically don't need to look up and set the versions of any dependent modules. For instance, if you only care that you use `GCC/12.3.0`, then you can do the following:

```bash
ml GCC/12.3.0 Python matplotlib SciPy-bundle
```

However, if you are moving code developed on a personal laptop to the cluster, you will typically be constrained to a range of Python versions (e.g., >=3.10, or 3.9 to 3.12) and a narrower range of 2-3 GCC versions, so you will have to explore a bit and find the closest approximation.

Let's say you built a Matplotlib-based script using Python 3.11.8 on your own laptop. Glob patterns don't work to filter `ml spider` or `ml avail` (`ml -r spider '.*Python/.*'` will expand the results to every module containing `Python` in the name, but adding `3.11` will **not** limit the results to only those with `Python/3.11`), so you'll have to look at the full list first:

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

The closest result is `Python/3.11.5`, so let's see what that requires:

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

It's usually safer to load GCC than GCCcore. Many modules that have Python as a prerequisite require the full GCC instead of only GCCcore. Matplotlib is a prime example:

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

On some centers, loading GCC and then Matplotlib will automatically load Python as well, so Python is not always listed as a prerequisite. In practice, though, you will typically know which Python you need, not which Matplotlib to use, and `ml show matplotlib/<version>` will not show you which Python is associated with that version.

Each version of Matplotlib and SciPy-bundle should only be associated with one Python version, so now we can load them all at once like this:

```bash
ml GCC/13.2.0 Python/3.11.5 Matplotlib SciPy-bundle
```

And if we check what's loaded, we get this: 

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

Note that in this example, `Tkinter/3.11.5` is loaded automatically with Matplotlib. That is not true everywhere.

### Example 2: MPI4Py

Here is an example of what `ml spider MPI4Py` would output (note that `ml spider` is not case-sensitive, unlike most other LMOD commands):

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

Here again, you will typically only know which Python version you need and/or which GCC version to load, not which version of `mpi4py` you need. However, since the previous example showed how to work from a known Python version, this example will show a workflow based on working backwards from the latest version of a module. In the above output, that is `mpi4py/4.0.1`, so here is how to view the dependencies for that:

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

As you can see, on the cluster where this code was run, Python was loaded automatically. However, note that no BLAS or LAPACK libraries are loaded, which might be unexpected given the types of problems MPI is typically used for. These libraries are available through either a `foss` toolchain or a `SciPy-bundle`, and in practice, you will almost always load one or both of `foss` and `SciPy-bundle` when using `mpi4py`.

## R-based packages

### Example 1: Bioconductor
Some R-packages conveniently specify the version of R they are compatible with right in the name. One example of this is Bioconductor.

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

Notice that in this case, there are 2 versions of the Bioconductor bundle associated with `R/4.4.1`, and that there are 2 versions of R associated with `R-bundle-Bioconductor/3.18`. That means you should not rely on the prerequisites to set which version of `R-bundle-Bioconductor` gets loaded---that will load whatever happens to be the latest version at the time. R releases are built rarely enough that there are often multiple versions of an R-dependent package, and thus the latest version is subject to change. Let's look at one.

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

The lists of extensions with R bundles can also get extremely long.

## Matlab

At most HPC centers, Matlab can be loaded directly.

Example:

## Specialized Applications

