# Modules in batch scripts 

This section looks at how to use modules in batch scripts. This is done much the same as when you just load them on the command line. 

!!! note 

    - Any longer, parallel, heavy jobs should be run as a batch job in order not to overload the login node and make it slow/unusuable for everyone. 
    - When you write a batch script you have to **load the modules you need to run the job**, just as you would have otherwise. 
    - If the module has prerequisites, they have to be loaded before the module
    - The modules you load need to be compatible with each other, just like when you load them on the command line. 

When you load modules inside a batch script, there are only really three different cases: 

- serial jobs
- parallel/MPI jobs
- GPU jobs 

We will use Python and various Python package modules as an example. 

!!! note 

    A batch job is submitted to the batch queue with ``sbatch <batchjob.sh>``. 

## Serial jobs 

Here you do not need MPI-enabled modules. If the module has both a GPU and a CPU version, you should load the CPU version. 

!!! note "Example - serial job script" 

    The example Python script used here is <a href="../mmmult.py" target="_blank">mmmult.py</a>. 

    === "HPC2N" 

        Short serial example for running on Kebnekaise. Loading SciPy-bundle/2023.07 and Python/3.11.3 

        ```bash 
        #!/bin/bash
        #SBATCH -A hpc2nXXXX-YYY # Change to your own project ID 
        #SBATCH --time=00:10:00 # Asking for 10 minutes
        #SBATCH -n 1 # Asking for 1 core

        # Purge the environment of any modules 
        module purge > /dev/null 2>&1 
        # Load any modules you need, here for Python/3.11.3 and compatible SciPy-bundle
        module load GCC/12.3.0 Python/3.11.3 SciPy-bundle/2023.07

        # Run your Python script
        python mmmult.py        
        ``` 

    === "UPPMAX" 

        Short serial example script for Rackham. Loading Python 3.11.8. Numpy is preinstalled and does not need to be loaded. 

        ```bash 
        #!/bin/bash -l
        #SBATCH -A uppmaxXXXX-Y-ZZZ # Change to your own project ID 
        #SBATCH --time=00:10:00 # Asking for 10 minutes
        #SBATCH -n 1 # Asking for 1 core

        # module purge is not recommended - if you do it you need to "module load uppmax" after 
        # Load any modules you need, here Python 3.11.8.
        module load python/3.11.8

        # Run your Python script
        python mmmult.py
        ```

    === "LUNARC"

        Short serial example for running on Cosmos. Loading SciPy-bundle/2023.11 and Python/3.11.5 

        ```bash 
        #!/bin/bash
        #SBATCH -A luXXXX-Y-ZZ # Change to your own project ID 
        #SBATCH --time=00:10:00 # Asking for 10 minutes
        #SBATCH -n 1 # Asking for 1 core

        # Purge the environment of any modules
        module purge > /dev/null 2>&1
        # Load any modules you need, here for Python/3.11.5 and compatible SciPy-bundle
        module load GCC/13.2.0 Python/3.11.5 SciPy-bundle/2023.11

        # Run your Python script
        python mmmult.py
        ```

    === "NSC"
 
        Short serial example for running on Tetralith. Loading SciPy-bundle/2022.05 and Python/3.10.4 

        ```bash 
        #!/bin/bash
        #SBATCH -A naissXXXX-YY-ZZZ # Change to your own project ID 
        #SBATCH --time=00:10:00 # Asking for 10 minutes
        #SBATCH -n 1 # Asking for 1 core

        # Purge the environment of any modules - here it is neither recommended or not 
        module purge > /dev/null 2>&1
        # Load any modules you need, here for Python/3.10.4 and compatible SciPy-bundle
        module load buildtool-easybuild/4.8.0-hpce082752a2 GCC/11.3.0 OpenMPI/4.1.4 Python/3.10.4 SciPy-bundle/2022.05

        # Run your Python script
        python mmmult.py
        ``` 

    === "PDC"

        Short serial example for running on Dardel. Loading cray-python/3.11.7 

        ```bash 
        #!/bin/bash
        #SBATCH -A naissXXXX-YY-ZZZ # Change to your own
        #SBATCH --time=00:10:00 # Asking for 10 minutes
        #SBATCH -n 1 # Asking for 1 core

        # WARNING! Do not do "module purge" here! 
        # Load any modules you need, here for cray-python/3.11.7.
        module load cray-python/3.11.7

        # Run your Python script
        python mmmult.py
        ``` 

    === "C3SE" 

        Short serial example for running on Alvis. Loading Python/3.11.5-GCCcore-13.2.0 and SciPy-bundle/2023.11-gfbf-2023b 

        ```bash 
        #!/bin/bash
        #SBATCH -A naissXXXX-YY-ZZZ # Change to your own
        #SBATCH --time=00:10:00 # Asking for 10 minutes
        #SBATCH -n 1 # Asking for 1 core

        # Purge the environment of any modules
        module purge > /dev/null 2>&1
        # Load any modules you need, here for Python/3.11.5-GCCcore-13.2.0 and SciPy-bundle/2023.11-gfbf-2023b 
        module load Python/3.11.5-GCCcore-13.2.0
        module load SciPy-bundle/2023.11-gfbf-2023b 

        # Run your Python script
        python mmmult.py 
        ``` 

## MPI jobs 

There is little difference between the job scripts for a serial job and an MPI job: 

- you ask for more cores since you want to run more tasks
- you may need other modules that are parallelized versions of the software 

Continuing with a Python example 

!!! note "Example - MPI job script" 

    Make sure you ask for enough tasks and your modules are for parallelized software. Here a program that needs mpi4py: <a href="../integration2d_mpi.py" target="_blank">integration2d_mpi.py</a>. 

    === "HPC2N"

        ```bash 
        #!/bin/bash
        # The name of the account you are running in, mandatory.
        #SBATCH -A hpc2nXXXX-YYY
        # Request resources - here for eight MPI tasks
        #SBATCH -n 8
        # Request runtime for the job (HHH:MM:SS) where 168 hours is the maximum at most centres. Here asking for 15 min.
        #SBATCH --time=00:15:00

        # Clear the environment from any previously loaded modules
        module purge > /dev/null 2>&1

        # Load the module environment suitable for the job, it could be more or
        # less, depending on other package needs. This is for a simple job needing
        # mpi4py. 

        ml GCC/12.3.0 Python/3.11.3 SciPy-bundle/2023.07 OpenMPI/4.1.5 mpi4py/3.1.4

        # And finally run the job - use srun for MPI jobs, but not for serial jobs
        srun ./integration2d_mpi.py
        ```

    === "UPPMAX"

        ```bash
        #!/bin/bash
        # The name of the account you are running in, mandatory.
        #SBATCH -A uppmaxXXXX-Y-ZZZ
        # Request resources - here for eight MPI tasks
        #SBATCH -n 8
        # Request runtime for the job (HHH:MM:SS) where 168 hours is the maximum at most centres. Here asking for 15 min.
        #SBATCH --time=00:15:00

        # Module purge is not recommended - if you do it you need to "module load uppmax" after

        # Load the module environment suitable for the job, it could be more or
        # less, depending on other package needs. This is for a simple job needing
        # mpi4py. 

        # Here mpi4py are not installed and you need a virtual env.
        # The following steps would need to be done before submitting the job script: 
        # module load python/3.11.8 python_ML_packages/3.11.8-cpu openmpi/4.1.5
        # python -m venv mympi4py
        # source mympi4py/bin/activate
        # pip install mpi4py

        module load python/3.11.8 python_ML_packages/3.11.8-cpu openmpi/4.1.5
        source mympi4py/bin/activate
        
        # And finally run the job - use srun for MPI jobs, but not for serial jobs
        srun ./integration2d_mpi.py
        ```

    === "LUNARC"

        ```bash
        #!/bin/bash
        # The name of the account you are running in, mandatory.
        #SBATCH -A luXXXX-Y-ZZ 
        # Request resources - here for eight MPI tasks
        #SBATCH -n 8
        # Request runtime for the job (HHH:MM:SS) where 168 hours is the maximum at most centres. Here asking for 15 min.
        #SBATCH --time=00:15:00

        # Clear the environment from any previously loaded modules
        module purge > /dev/null 2>&1

        # Load the module environment suitable for the job, it could be more or
        # less, depending on other package needs. This is for a simple job needing
        # mpi4py. 

        ml GCC/13.2.0 Python/3.11.5 SciPy-bundle/2023.11 OpenMPI/4.1.6 mpi4py/3.1.5

        # And finally run the job - use srun for MPI jobs, but not for serial jobs
        srun ./integration2d_mpi.py
        ```

    === "NSC"

        ```bash
        #!/bin/bash
        # The name of the account you are running in, mandatory.
        #SBATCH -A naissXXXX-YY-ZZZ
        # Request resources - here for eight MPI tasks
        #SBATCH -n 8
        # Request runtime for the job (HHH:MM:SS) where 168 hours is the maximum at most centres. Here asking for 15 min.
        #SBATCH --time=00:15:00

        # At NSC, module purge is safe and neither recommended nor not 
        module purge > /dev/null 2>&1

        # Load the module environment suitable for the job, it could be more or
        # less, depending on other package needs. This is for a simple job needing
        # mpi4py. 

        ml buildtool-easybuild/4.8.0-hpce082752a2 GCC/11.3.0 OpenMPI/4.1.4 Python/3.10.4 SciPy-bundle/2022.05

        # And finally run the job - use srun for MPI jobs, but not for serial jobs
        srun ./integration2d_mpi.py
        ```

    === "PDC" 

        ```bash
        #!/bin/bash
        # The name of the account you are running in, mandatory.
        #SBATCH -A naissXXXX-YY-ZZZ 
        #SBATCH  -p shared         # name of the queue
        # Request resources - here for eight MPI tasks
        #SBATCH -n 8
        #SBATCH --cpus-per-task=1  # nr. of cores per-task
        # Request runtime for the job (HHH:MM:SS) where 168 hours is the maximum at most centres. Here asking for 15 min.
        #SBATCH --time=00:15:00

        # WARNING! Do not do "module purge" here! 

        # Load the module environment suitable for the job, it could be more or
        # less, depending on other package needs. This is for a simple job needing
        # mpi4py. 

        ml cray-python/3.11.7

        # And finally run the job - use srun for MPI jobs, but not for serial jobs
        srun ./integration2d_mpi.py
        ```

    === "C3SE" 

        ```bash
        #!/bin/bash
        # The name of the account you are running in, mandatory.
        #SBATCH -A naissXXXX-YY-ZZZ
        # Request resources - here for eight MPI tasks
        #SBATCH -n 8
        # Request runtime for the job (HHH:MM:SS) where 168 hours is the maximum at most centres. Here asking for 15 min.
        #SBATCH --time=00:15:00

        # Clear the environment from any previously loaded modules
        module purge > /dev/null 2>&1

        # Load the module environment suitable for the job, it could be more or
        # less, depending on other package needs. This is for a simple job needing
        # mpi4py. 

        ml Python/3.11.5-GCCcore-13.2.0 SciPy-bundle/2023.11-gfbf-2023b mpi4py/3.1.5-gompi-2023b          

        # And finally run the job - use srun for MPI jobs, but not for serial jobs
        srun ./integration2d_mpi.py
        ```

## GPU jobs 

Here there are two things to pay attention to: 

- The modules you load must be for software that is GPU-aware (and often compiled with CUDA as well). 
- You need to ask for GPU resources in the batch script 

!!! note "Example - GPU job script" 

    === "HPC2N" 

    === "UPPMAX"

    === "LUNARC"

    === "NSC"
   
    === "PDC"

    === "C3SE" 

    

