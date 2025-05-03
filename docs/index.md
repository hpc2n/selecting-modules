# Welcome to the course: Selecting software modules 

This NAISS course is given under under NAISS, by staff working at the branches located at UPPMAX, LUNARC, and HPC2N.

!!! note "This material" 

    Here you will find the content of the workshop "Login and selecting software modules" 

    - Documentation about selecting modules at some of the Swedish HPC centres 
        - HPC2N: <a href="https://docs.hpc2n.umu.se/software/modules/" target="_blank">https://docs.hpc2n.umu.se/software/modules/</a> 
        - UPPMAX: <a href="https://docs.uppmax.uu.se/cluster_guides/modules/" target="_blank">https://docs.uppmax.uu.se/cluster_guides/modules/</a>  
        - LUNARC: <a href="https://lunarc-documentation.readthedocs.io/en/latest/manual/manual_modules/" target="_blank">https://lunarc-documentation.readthedocs.io/en/latest/manual/manual_modules/</a>  
        - PDC: <a href="https://support.pdc.kth.se/doc/basics/quickstart/#the-lmod-module-system" target="_blank">https://support.pdc.kth.se/doc/basics/quickstart/#the-lmod-module-system</a>   
        - NSC: <a href="https://www.nsc.liu.se/software/modules/" target="_blank">https://www.nsc.liu.se/software/modules/</a>  
        - C3SE: <a href="https://www.c3se.chalmers.se/documentation/module_system/" target="_blank">https://www.c3se.chalmers.se/documentation/module_system/</a> 

!!! note "Prerequisites"

    - Basic Linux 

    If you lack Linux experience, please follow a tutorial before the course. 

    This is from a recent NAISS course about basic Linux: 
  
    - Material: <a href="https://hpc2n.github.io/intro-linux/" target="_blank">https://hpc2n.github.io/intro-linux/</a>
    - Exercises: ``wget https://github.com/hpc2n/intro-linux/raw/refs/heads/main/exercises.tar.gz`` and then ``tar zxvf exercises.tar.gz`` 
    - Recordings from the course, on YouTube: <a href="https://www.youtube.com/playlist?list=PL6jMHLEmPVLzLr4i8ME2A-PUtawhkilbq" target="_blank">https://www.youtube.com/playlist?list=PL6jMHLEmPVLzLr4i8ME2A-PUtawhkilbq</a> 

!!! note "Learning outcome"   

     - What is the module system?
     - Why do I want to use the module system? 
     - What happens when I load/unload modules? 
         - paths
         - environment variables
         - more?
     - some talk about EasyBuild?
     - Useful commands
     - Load examples
     - Compiler toolchains 

!!! admonition "Cluster-specific approaches"

    The course will show you how to find and load modules. While the modules may be somewhat differently named and have somewhat different versions depending on the system, the procedure and commands are the same. 

    We will use Tetralith for those who do not have other access to a Swedish HPC system, and some examples will be shown for other centres. You should be able to code-along - with minor adjustments - regardless of which center you are at. 

## Important info

- There is an "important info" page for this course, containing info on the course project, login info for Tetralith, etc. 
    - It can be found here: <a href="https://umeauniversity.sharepoint.com/:w:/s/HPC2N630/EX7cOrKbXyhKrUfGvi1QYPIBY9OLYS1ecearjJefFiOhjg" target="_blank">https://umeauniversity.sharepoint.com/:w:/s/HPC2N630/EX7cOrKbXyhKrUfGvi1QYPIBY9OLYS1ecearjJefFiOhjg</a> 
- There is a Q/A page for use during the lectures. Since the lectures are recorded, you may get recorded if you ask questions in the Zoom, but you can always write questions on the Q/A and get answers there. It also has the advantage that you can go back and look at the answers later. 
    - The Q/A page can be found here: <a href="https://umeauniversity.sharepoint.com/:w:/s/HPC2N630/EVWzArI3hvpMnOHs17z23lQB-kFXnUaf3_wn_gxVgdUX6w" target="_blank">https://umeauniversity.sharepoint.com/:w:/s/HPC2N630/EVWzArI3hvpMnOHs17z23lQB-kFXnUaf3_wn_gxVgdUX6w</a>  

## Preliminary schedule

| Time | Topic | Activity |
| ---- | ----- | -------- |
| 10:00 | Welcome+Syllabus | Lecture |
| 10:15 | The module system: overview | Lecture | 
| 10:35 | The module system: Useful commands | Lecture+type along+exercise | 
| 10:55 | Break | | 
| 11:05 | Compiler toolchains | Lecture+type along |
| 11:20 | Software modules | Lecture+type along | 
| 11:40 | Modules in batch scripts | Lecture+type along |  
| 11:55 | Summary | |
| 12:00 | END | |

## Preparations 

In order to type along and do the exercises, please prepare your course environment now:

!!! note "1. Login"

    Login to the system you are using (Tetralith/Dardel, other Swedish HPC system)

    - You will not need a graphical user interface for this course. 
    - Even so, if you do not have a favourite SSH client, we recomment using <a href="https://www.cendio.com/thinlinc/download/" target="_blank">ThinLinc</a>
    
    Connection info for some Swedish HPC systems - use the one you have access to: 

    - **NSC**
        - SSH: ``ssh <user>@tetralith.nsc.liu.se``
        - ThinLinc:
            - Server: ``tetralith.nsc.liu.se``
            - Username: ``<your-nsc-username>``
            - Password: ``<your-nsc-password>``
        - Note that you need to setup <a href="https://www.nsc.liu.se/support/2fa/" target="_blank">TFA</a> to use NSC!
    - **HPC2N**
        - SSH: ``ssh <user>@kebnekaise.hpc2n.umu.se`` 
        - ThinLinc: 
            - Server: ``kebnekaise-tl.hpc2n.umu.se``
            - Username: ``<your-hpc2n-username>``
            - Password: ``<yout-hpc2n-password>``
        - ThinLinc Webaccess: 
            - Put ``https://kebnekaise-tl.hpc2n.umu.se:300/`` in browser address bar 
            - Put ``<your-hpc2n-username>`` and ``<your-hpc2n-password>`` in th e login box that opens and click ``Login`` 
    - **UPPMAX** 
        - SSH: ``ssh <user>@rackham.uppmax.uu.se``
        - ThinLinc: 
            - Server: ``rackham-gui.uppmax.uu.se`` 
            - Username: ``<your-uppmax-username>`` 
            - Password: ``<your-uppmax-password>`` 
        - ThinLinc Webaccess: 
            - Put ``https://rackham-gui.uppmax.uu.se`` in browser address bar 
            - Put ``<your-uppmax-username>`` and ``<your-uppmax-password>`` in the login box that opens and click ``Login`` 
        - Note that you may have to setup <a href="https://docs.uppmax.uu.se/getting_started/get_uppmax_2fa/" target="_blank">TFA for Uppmax</a> when using either of the ThinLinc connections. 
    - **LUNARC** 
        - SSH: ``ssh <user>@cosmos.lunarc.lu.se``
        - ThinLinc: 
            - Server: ``cosmos-dt.lunarc.lu.se``
            - Username: ``<your-lunarc-username>``
            - Password: ``<your-lunarc-password>``
        - Note that you need to setup <a href="https://lunarc-documentation.readthedocs.io/en/latest/getting_started/login_howto/" target="_blank">TFA (PocketPass)</a> to use LUNARC! 
    - **PDC** 
        - SSH: ``ssh <user>@dardel.pdc.kth.se`` 
        - ThinLinc: 
            - Server: ``dardel-vnc.pdc.kth.se`` 
            - Username: ``<your-pdc-username>``
            - Password: ``<your-pdc-password>`` 
        - Note that you need to setup <a href="https://support.pdc.kth.se/doc/login/ssh_login/" target="_blank">SSH keys</a> or kerberos in order to login to PDC!    
    - **C3SE**: 
        - SSH: ``ssh <user>@alvis1.c3se.chalmers.se``
               or 
               ``ssh <user>@alvis2.c3se.chalmers.se``
        - ThinLinc: 
            - Server: ``alvis1.c3se.chalmers.se`` 
                      or 
                      ``alvis2.c3se.chalmers.se``
            - Username: ``<your-c3se-username>``
            - Password: ``<your-c3se-username>`` 
        - ThinLinc Webaccess: 
            - Put ``https://alvis1.c3se.chalmers.se:300`` or ``https://alvis2.c3se.chalmers.se:300`` in browser address bar 
            - Put ``<your-c3se-username>`` and ``<your-c3se-password>`` in the login box that opens and click ``Login`` 
        - OpenOndemand portal: 
            - Put ``https://alvis.c3se.chalmers.se`` in browser address bar 
            - Put ``<your-c3se-username>`` and ``<your-c3se-password>`` in the login box 
        - Note that Alvis is accessible via SUNET networks (i.e. most Swedish university networks). If you are not on one of those networks you need to use a VPN - preferrably your own Swedish university VPN. If this is not possible, contact ``support@chalmers.se`` and ask to be added to the Chalmers's eduVPN. 

!!! note "2. Setup a working directory"

    You will not need a lot of space for the exercises, so you can create the directory under your home directory. 

    - Create a directory to work in (``mkdir $HOME/selecting-modules``) 
    - and then switch to it (``cd $HOME/selecting-modules``)

!!! note "3. Download the exercises" 

    - ``wget ...`` 

!!! note "4. Extract the exercises" 

    - ``tar zxvf exercises.tar.gz``

!!! note "5. Enter the directory that was created"

    - ``cd exercises``

