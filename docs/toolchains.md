# Compiler toolchains

A large proportion of the software at Swedish HPC centers is built using the
EasyBuild software deployment framework. Under this framework, software is built
around so-called "toolchains"---compatible groups of C compilers, linear algebra
packages, MPI libraries, and other fundamental packages that underlie the more
familiar packages that users typically work with. Most HPC centers recommend
using toolchains to build and compile software, whether you use the EasyBuild
framework or not, to keep software versions consistent and reproducible.

## Examples of currently provided toolchains

Not all of the toolchains below are available at all institutions.

### Toolchains for CPU nodes

=== "GCC compiler toolchains"

    * **GCC**: GCC
    * **foss**: GCC, OpenMPI, OpenBLAS, FFTW, BLACS, ScaLAPACK
    * **gompi**: GCC, OpenMPI
    * **gomkl**: GCC, OpenMPI, MKL
    * **gfbf**: GCC, FlexiBLAS, FFTW  (no MPI)

=== "Intel compiler toolchains"

    * **intel**: icc, ifort, Intel MPI, MKL
    * **iimpi**: icc, ifort, Intel MPI

### CUDA based toolchains for GPU nodes

=== "GCC compiler toolchains for GPU"

    * **gcccuda**:  GCC, CUDA
    * **fosscuda**: GCC, CUDA, OpenMPI, OpenBLAS, FFTW, ScaLAPACK

=== "Intel compiler toolchains for GPU"

    * **iccifortcuda**: icc, ifort, CUDA
    * **iimpic**: icc, ifort, CUDA, Intel MPI
    * **intelcuda**: icc, ifort, CUDA, Intel MPI, MKL

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

Most modules have a specific default version for each toolchain that you can
load without explicitly typing the version number. Not all modules have a
version associated with your choice of toolchain. In rare cases, there may be
two or more versions of a software package available for your chosen toolchain,
and in that case you need to specify the version you want.

Please note that if you switch toolchains, the module system will attempt to
upgrade or downgrade any associated modules to match. If one or more modules
that were loaded for the previous choice of toolchain do not have a version
installed that is compatible with the new toolchain, the module system will
raise an error. 

If there is a built-in system version of a package and one or more modules
provide alternative versions, loading any module version will override the
system version. This is good: the system version of a package is subject to
change and should generally be avoided if you want your programming to be
reproducible. A prime example is Python; at most HPC centers, the system version
of Python will be older than most of the module versions.

## Compiling serial code using a toolchain

If you have loaded a toolchain, build use the following commands to compile:

=== "GCC Toolchain"

    * **gcc**: C compiler
    * **g++**: C++ compiler
    * **gfortran**: Fortran compiler

=== "Intel Toolchain"

    * **icc**: C compiler
    * **icpc**: C++ compiler
    * **ifort**: Fortran compiler

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
