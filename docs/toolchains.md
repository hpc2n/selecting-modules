# Compiler toolchains

!!! note "Objectives"

    - Learn what compiler toolchains are and why they are used
    - Be able to find which compiler toolchains are installed
    - Be able load the compiler toolchain you want

A large proportion of the software at Swedish HPC centers is built using the
EasyBuild software deployment framework. Under this framework, software is built
around so-called "toolchains": mutually compatible groups of C and Fortran compilers,
linear algebra libraries, MPI libraries, and other fundamental packages that are
used internally by more familiar packages that users typically work with (e.g.,
Python, R, GROMACS, etc.).

If you can think of familiar software packages like cars, then toolchains can be
thought of like car *platforms*: it is common for similarly-sized cars from several
brands to share standardized engine components, power steering systems, wheel axles,
suspensions, and floorpans the same manufacturer.

Most HPC centers recommend using toolchains to build and compile software, whether
you use the EasyBuild framework or not, to keep software versions consistent and
reproducible.

## Available Toolchains by HPC Center

### Toolchains for regular CPU nodes

For most of the following toolchains, multiple versions are available on each of
the specified clusters. Use `ml spider <toolchain>` to determine which versions
are available and what modules are required as prerequisites to load them, if
any.

=== "HPC2N (Kebnekaise)"

    **GCC Compiler Toolchains**
    
       * **GCC**: GCC compiler only
       * **foss**: GCC, OpenMPI, OpenBLAS, FFTW, BLACS, ScaLAPACK
       * **gompi**: GCC, OpenMPI
       * **gomkl**: GCC, OpenMPI, MKL
       * **gfbf**: GCC, FlexiBLAS, FFTW  (no MPI)

    **Intel Compiler Toolchains**
    
       * **intel**: icc, ifort, Intel MPI, MKL
       * **intel-compilers**: icc, icpc, ifort, icx, icpx, ifx (no MPI or MKL)
       * **iimpi**: icc, ifort, Intel MPI

=== "LUNARC (Cosmos)"

    **GCC Compiler Toolchains**
    
       * **GCC**: GCC compiler only
       * **foss**: GCC, OpenMPI, OpenBLAS, FFTW, BLACS, ScaLAPACK
       * **gompi**: GCC, OpenMPI
       * **gomkl**: GCC, OpenMPI, MKL
       * **gfbf**: GCC, FlexiBLAS, FFTW  (no MPI)
       
    **Intel Compiler Toolchains**
    
       * **intel**: icc, ifort, Intel MPI, MKL
       * **intel-compilers**: icc, icpc, ifort, icx, icpx, ifx (no MPI or MKL)
       * **iimpi**: icc, ifort, Intel MPI

=== "NSC (Tetralith)"
      
    Note that unlike most clusters, on Tetralith, users must load a module of the form
    `buildtool-easybuild/X.X.X-hpcXXXXXXXXX` before loading any of the toolchains below.
    (putting this up here temporarily to see if compilation is affected)

    **GCC Compiler Toolchains**
    
       * **GCC**: GCC compiler only
       * **foss**: GCC, OpenMPI, OpenBLAS, FFTW, BLACS, ScaLAPACK
       * **gfbf**: GCC, FlexiBLAS, FFTW  (no MPI)
       
    **Intel Compiler Toolchains**
    
       * **intel**: icc, ifort, Intel MPI, MKL
       * **intel-compilers**: icc, icpc, ifort, icx, icpx, ifx (no MPI or MKL)

=== "UPPMAX (Pelle)"

    **GCC Compiler Toolchains**
    
       * **GCC**: GCC compiler only
       * **foss**: GCC, OpenMPI, OpenBLAS, FFTW, BLACS, ScaLAPACK
       * **gfbf**: GCC, FlexiBLAS, FFTW  (no MPI)
       
    **Intel Compiler Toolchains**
    
       * **intel**: icc, ifort, Intel MPI, MKL
       * **intel-compilers**: icc, icpc, ifort, icx, icpx, ifx (no MPI or MKL)
       * **iimpi**: icc, ifort, Intel MPI

=== "UPPMAX (Rackham/Bianca)"

    Toolchains less visible than on other clusters. Most software are built manually without EasyBuild. Instead, you load compilers, MPI libraries, math libraries, and other modules individually. *The Easybuild toolchains below are provided to demonstrate common combinations of packages.*
    
       * Need only a compiler? Search for available GCC or intel modules.
       * Need math libraries, like OpenBLAS? Search for that and GCC or intel will be a dependency.
       * Need MPI? See UPPMAX documentation about [combinations of compilers and MPI libraries](https://docs.uppmax.uu.se/software/parallel_comb/)

    The **Easybuild toolchains** below may be found for *some* tools in their names and dependencies:
    
    **GCC Compiler Toolchains**
    
       * **GCC**: GCC compiler only
       * **foss**: GCC, OpenMPI, OpenBLAS, FFTW, BLACS, ScaLAPACK
       * **gompi**: GCC, OpenMPI
       
    **Intel Compiler Toolchains**
    
       * **intel**: icc, ifort, Intel MPI, MKL
       * **iimpi**: icc, ifort, Intel MPI

=== "PDC (Dardel)"

    On Dardel, nearly all software packages have a prerequisite called something like PDC/xx.xx or PDCOLD/xx.xx that will have to be loaded first. If you load the wrong one, use `ml unload`! **Do not purge!**
    
    Toolchains at PDC bear little resemblance to any other cluster covered here because PDC uses different hardware architecture and the Cray Programming Environment (CPE).
    
    - On Dardel, `PrgEnv-cray` and `cray-libsci` are loaded by default, and provide the compiler and linear algebra libraries, respectively.
    - Loading `cpe/YY.XX` adds `cray-mpich` as the MPI library, and modifies `PrgEnv-cray` and `cray-libsci` if needed to maintain compatibility.
    - The FFT library, `cray-fftw`, must be loaded separately.
    - Alternative compilers are provided by `PrgEnv-gnu` (loads the GNU compiler suite) and `PrgEnv-aocc` (loads the AMD AOCC compilers).
    
    For software built or intended to be built with EasyBuild, there are 3 toolchains that help to assemble the appropriate combinations of the modules mentioned above, among other tools to support parallel programming.

    - cpeCRAY: based on `PrgEnv-cray`
    - cpeAMD: based on `PrgEnv-gnu`
    - cpeGNU: based on `PrgEnv-aocc`
    
    In the interest of time, we refer readers to [this page on the CPE](https://support.pdc.kth.se/doc/software_development/development/#the-cray-programming-environment) and [this page on CPE-based toolchains](https://support.pdc.kth.se/doc/software_development/easybuild/#toolchains).


### CUDA based toolchains for GPU nodes

=== "HPC2N (Kebnekaise)"

    **GCC Compiler Toolchains for GPU**
    
       * **gcccuda**:  GCC, CUDA
       * **fosscuda**: GCC, CUDA, OpenMPI, OpenBLAS, FFTW, ScaLAPACK

    **Intel Compiler Toolchains for GPU**
    
       * **iccifortcuda**: icc, ifort, CUDA (2019 releases only)
       * **iimpic**: icc, ifort, CUDA, Intel MPI (2019 releases only)
       * **intelcuda**: icc, ifort, CUDA, Intel MPI, MKL

=== "LUNARC (Cosmos)"

    **GCC Compiler Toolchains for GPU**
    
       * **gcccuda**:  GCC, CUDA
       * **fosscuda**: GCC, CUDA, OpenMPI, OpenBLAS, FFTW, ScaLAPACK

    **Intel Compiler Toolchains for GPU**
    
       * **iccifortcuda**: icc, ifort, CUDA
       * **iimpic**: icc, ifort, CUDA, Intel MPI
       * **intelcuda**: icc, ifort, CUDA, Intel MPI, MKL

=== "NSC (Tetralith)"

    **GCC Compiler Toolchains for GPU**
    
       * **buildenv-gcccuda**: GCC, CUDA, OpenMPI, OpenBLAS, FFTW, ScaLAPACK

=== "UPPMAX (Pelle)"

    - As of today you load GCC and CUDA separately.
    - New tools are typically build with the latest versions of both.

=== "UPPMAX (Rackham)"

    !!! important

        Rackham only has GPUs on the login nodes. You will need an account on Snowy to use GPUs intensively.

    **GCC Compiler Toolchains for GPU**
    
       * **gcccuda**:  GCC, CUDA
       * **fosscuda**: GCC, CUDA, OpenMPI, OpenBLAS, FFTW, ScaLAPACK


## Selecting a toolchain

The above toolchain choices can be a bit overwhelming, especially for new users. 
Good choices for general use are the toolchains:

* **foss**, to use the GCC compiler suite
* **intel**, to use the Intel compiler suite
* **gomkl**, to use the GCC compiler suite with Intel's Math Kernel Library (MKL)

**Example:** To check the foss versions available, use

```bash
  module avail foss
```

and you will get an output similar to this example on COSMOS:

```bash
----------------- /sw/easybuild_milan/modules/all/Core ------------------
   foss/2020b    foss/2022a    foss/2023b          fosscuda/2020b
   foss/2021a    foss/2022b    foss/2024a
   foss/2021b    foss/2023a    foss/2024.05 (D)

  Where:
   D:  Default Module

If the avail list is too long consider trying:

"module --default avail" or "ml -d av" to just list the default modules.
"module overview" or "ml ov" to display the number of modules for each
name.

Use "module spider" to find all possible modules and extensions.
Use "module keyword key1 key2 ..." to search for all possible modules
matching any of the "keys".
```

The version numbers indicate roughly when each version was released. Version
2023a was released at the beginning of 2023, 2023b in the middle of 2023, and
2024a at the start of 2024. If you load e.g. the `foss/2024a` module as shown
below,


```bash
module load foss/2024a
```

It will load several modules for you, including the compiler, libraries, and
other utilities. The command `module list`, or `ml` for short, can then show you
what modules are now loaded and available for use. Below is the output of `ml`
after loading `foss/2024a` on COSMOS:

```bash
Currently Loaded Modules:
  1) SoftwareTree/Milan  (S)  13) UCX/1.16.0
  2) GCCcore/13.3.0           14) libfabric/1.21.0
  3) zlib/1.3.1               15) PMIx/5.0.2
  4) binutils/2.42            16) PRRTE/3.0.5
  5) GCC/13.3.0               17) UCC/1.3.0
  6) numactl/2.0.18           18) OpenMPI/5.0.3
  7) XZ/5.4.5                 19) OpenBLAS/0.3.27
  8) libxml2/2.12.7           20) FlexiBLAS/3.4.4
  9) libpciaccess/0.18.1      21) FFTW/3.3.10
 10) hwloc/2.10.0             22) FFTW.MPI/3.3.10
 11) OpenSSL/3                23) ScaLAPACK/2.2.0-fb
 12) libevent/2.1.12          24) foss/2024a

  Where:
   S:  Module is Sticky, requires --force to unload or purge
```

After loading a toolchain, many new modules become available, each reliant on
the specific versions of the modules included in the toolchain. Use `ml avail`
to see what modules you can load given the toolchain you've loaded.

For some packages with very long or very short release intervals, there may be
multiple versions of a software package available for your chosen toolchain.
The version that loads by default if no version number is given will have a
`(D)` to the right of it when listed using `ml avail`. This default version is
subject to change, so it is recommended to always provide version numbers.

Not all modules have a version associated with your choice of toolchain. If you
switch toolchains, keep the following in mind:

    * The module system will attempt to upgrade or downgrade any previously
    loaded modules to match the new choice of toolchain.
    * If one or more modules loaded with the previous choice of toolchain do
    not have a version installed that is compatible with the new toolchain,
    then the module system will raise an error.
    * For some packages (e.g., Python, Perl, etc.) there is a system version
    built into the operating system that runs without loading any module or
    toolchain. These should be avoided since they are subject to change.
    
## Compiling serial code using a toolchain

If you have loaded a toolchain, choose your compiler from the following table
based on your coding language and toolchain choice. Note that there are 2 Intel
versions per language; versions ending with x are newer OneAPI versions supported
from 2023 onward.

| Coding Language | GCC Toolchain | Intel Toolchain |
| :-------------- | :------------ | :-------------- |
| C               | gcc           | icc, icx        |
| C++             | g++           | icpc, icpx      |
| Fortran         | gfortran      | ifort, ifx      |

For some open-source toolchains, there may be no difference between compiling
serial code with a toolchain and using a built-in compiler. The differences are
more visible when using an Intel toolchain or when using MPI.

### Toolchains using MPI

The table below shows which MPI compiler to use given your choices of toolchain
and coding language:

| Coding Language | Toolchain with OpenMPI | Intel Toolchain |
| :-------------- | :--------------------- | :-------------- |
| C               | mpicc                  | mpiicc          |
| C++             | mpicxx                 | mpiicpc         |
| Fortran         | mpifort                | mpiifort        |
 
Inside your job script, executables built with OpenMPI need to be run using the
`mpirun` command. For OpenMPI jobs not using threads, we recommend task binding
(i.e., use the `mpirun` option `--bind-to core`)

For Intel-based MPI scripts, the script should be run with `srun` command.

Here is a template for a script using OpenMPI (adapted from LUNARC
documentation):

```bash
#!/bin/sh
#SBATCH -N 3                 # number of nodes (arbitrary number >1)
#SBATCH --ntasks-per-node=16 # adapt for your needs
#SBATCH --exclusive          # reserve whole nodes
#SBATCH -t 0:60:0            # run time
#SBATCH -J my_mpi_job        # job name in queue
#SBATCH -o result_%j.out     # output file
#SBATCH -e result_%j.err     # error file
#SBATCH -A <project-ID>      # replace with your project ID

cat $0

# module load statement(s) - adapt for your package(s)
module load foss/2023a

# run the jobs - here we assume an executable name: my_program
mpirun --bind-to core my_program
# for intel toolchains, the command is srun
```
