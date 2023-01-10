TITLE: ENZO grackle build
About: Building ENZO with grackle and running some sims
Start Date: 11/05/2022
End Date: ---

[------------------------------------------------------------------------------------]

11/05/2022

> Getting ENZO- current stable source code can be found at:
   $ git clone https://github.com/enzo-project/enzo-dev 
> Setting up ENZO- Go to the ENZO directory and:
   $ ./configure 
   This wipes out all configuration settings and restores them to defaults, usually run once
>Various subdirectories:
   bin: 
   doc: contains both older and newer documentation (manual/) and can be read in plaintext
   input: files used as input to several test problems. Enzo startup fail is likely localized here
   run: has example parameter files with notes about expected output and scripts for plotting. Also used to compare results of one version of code with previous version (Enzo answer test suite)
   src: ENZO source and affiliated utilities
>Enzo Makefiles:
   change to src/enzo/ and:
   $ ls Make.mach.* 
   shows list of potential pre-made Makefiles. We run:
   $ make machine-cloudbreak
>Updating PATH:
   install yt-conda from: https://yt-project.org/
   run the install script and add the yt-conda path to the path variable
   add /usr/lib64/openmpi/bin to the path variable for openmpi
   add /usr/lib64:/usr/lib64/openmpi/lib to LD_LIBRARY_PATH
>Building Enzo:
   $ make show-config
   will show the current coniguration
   $ make help-config
   will describe how to turn these options on or off
   $ make
   compile enzo and produce an enzo.exe

>Running a simulation (CosmologySimulation/amr_cosmology)!

>Running a simulation (CosmologySimulation/amr_nested_cosmology)!

>Running a simulation (CosmologySimulation/ReionizationHydro)!

>Running a simulation (CosmologySimulation/ReionizationRadHydro)!

>Running a simulation (Hydro/Hydro-3D/AgoraGalaxy)!
 


 
