
# **TITLE**: Collapsing Molecular Clouds with Star Particles
**About** : Rerunning https://ui.adsabs.harvard.edu/abs/2022MNRAS.tmp.2650C/abstract with sink particles

**Start Date**: 11/18/2022

**End Date**: --

## Goals
### Long Term Goal:
   * Repeat the simulation done in https://ui.adsabs.harvard.edu/abs/2022MNRAS.tmp.2650C/abstract ([Edited paper](https://share.goodnotes.com/s/iPlfCAy6n9a2HzL67PfF6e)) with Sink Particles and Radiation Feedback

### Short Term Goals:
   * Simple test with Sink Particles
   * Simple test with Feedback
   * Simple test with Moray radiation.
   * Accreting Tracer Particles

## Research Summary
### Simple Test with Sink Particles
**Aim:** To get sink particles to work - form, merge and accrete.
**Research Papers:**
   * *Outflow Feedback Regulated Massive Star Formation in Parsec-Scale Cluster-Forming Clumps*, Peng Wang 2010 ([link](https://ui.adsabs.harvard.edu/abs/2010ApJ...709...27W/abstract)) ([Edited paper](https://share.goodnotes.com/s/BCOsgmPjL1IzaAhVDh0Uoe)): 
      - How is star formation achieved objectively in literature
      - Implementation of sink particles in ENZO
      - Sink particles merge and accrete to physically represent star particles
   * *The Origin of Massive Stars: The Inertial-inflow Model*, Paolo Padoan 2020 ([link](https://ui.adsabs.harvard.edu/abs/2020ApJ...900...82P/abstract)) ([Edited paper](https://share.goodnotes.com/s/1eTUAUpUE4wn3XpoodhEEa)):
      - Sink Algorithm used by Padoan
   
**Work**
#### CollapseTestNonCosmological_DensityUNIFORM_SinkParticlesOFF
**Simulation Goal:** To run a simple uniform denisty collapse test as a sample to compare when turning on sink particles
**Details:** 
   * Ran a 128^3 square grid with 7 levels of AMR with a spherical gas of radius 0.15 with uniform density 500 times than the background with centre at (0.5,0.5,0.5). 
   * There are 27 data dumps. Each data dump corresponds to 1 kyr.
   * Ran a script (*CollapseTestNonCosmological_DensityUNIFORM_SinkParticlesOFF.py*) that outputs the x,y,z ProjectionPlots of gas density and Tempterature.
   

#### CollapseTestNonCosmological_DensityUNIFORM_SinkParticlesON
**Simulation Goal:** To run the previous Collapse test with sink particles on.
**Details:** 
   * Ran a 128^3 square grid with 7 levels of AMR with a spherical gas of radius 0.15 with uniform density 500 times than the background with centre at (0.5,0.5,0.5). 
   * There are 27 data dumps. Each data dump corresponds to 1 kyr.
   * Sink particles are turned on (StarParticleCreation = 16) (BigStarFormation = 1)
   * Ran a script (*CollapseTestNonCosmological_DensityUNIFORM_SinkParticlesON.py*) that outputs the x,y,z ProjectionPlots of gas density and Tempterature.
      - Sink particles are created between t=23 and t=24 kyr
      - A total of 9 sink particles are created. 8 are created in zones around (0.5,0.5,0.5) and 1 m=0, x=(-nan,-nan,-nan), v=-nan sink particle is created
   * Ran a script (*AnalysingSinkParticles_CollapseTestNonCosmological_DensityUNIFORM_SinkParticlesON.py*) that analyses various aspects of sink particles:
      - Plots the 8 valid sink particles against the background for t = 24, 25, 26, 27
         + Sink particles move around ***(figure out why)***
      -  Plots the mass evolution of all sink particles
         +  Mass remains the same, particles not accreting ***figure out why***
      -  ***Make a radial profile of average “cell_volume”***
      -  ***Make a radial profile of density***
      -  ***Plot profiles of (sink, non-sink) on top of each other***


#### CollapseTestNonCosmological_DensityPOWERLAW_SinkParticlesOFF
**Simulation Goal:** To run a simple r^-2 density collapse test as a sample to compare when turning on sink particles
**Details:** 
   * Ran a 16^3 square grid with 3 levels of AMR with a spherical gas of radius 0.15 with uniform density 10 times than the background with centre at (0.5,0.5,0.5).
   * There are 20 data dumps. Each data dump corresponds to 0.5 kyr.
   

#### CollapseTestNonCosmological_DensityPOWERLAW_SinkParticlesON
**Simulation Goal:** To run a simple r^-2 density collapse test as a sample to compare when turning on sink particles
**Details:** 
   * Ran a 16^3 square grid with 3 levels of AMR with a spherical gas of radius 0.15 with uniform density 10 times than the background with centre at (0.5,0.5,0.5).
      - Code creates sink particles, but fails to merge them ***(figure out why)***
         + Sink particles are forming, merging is working fine but when merged particles are added back to the grid, the code fails
         + ***code doesn't fail on 1 processor***
         + ***run it in 2 processors, print processor number to see where it fails***
            * Code outputs the following when number of processors is not 2^x: (output for run with 9 processors [here](./Project_A_stuff/CollapseTestNonCosmological_DensityPOWERLAW_SinkParticlesON/output_proc_9.txt))
               ```bash
               Caught fatal exception:
               
               'high (-1) < low (0) when searching for lower bound.'
               at SearchUtilities.C:29
               
               Backtrace:
               
               BT symbol: ./enzo.exe() [0x412c75]
               BT symbol: ./enzo.exe() [0x7f16fa]
               BT symbol: ./enzo.exe() [0x7f1820]
               BT symbol: ./enzo.exe() [0x7f1820]
               BT symbol: ./enzo.exe() [0x628b09]
               BT symbol: ./enzo.exe() [0x48842c]
               BT symbol: ./enzo.exe() [0x7e75e9]
               BT symbol: ./enzo.exe() [0x4dc050]
               BT symbol: ./enzo.exe() [0x40dd48]
               BT symbol: /lib/x86_64-linux-gnu/libc.so.6(__libc_start_main+0xf0) [0x7fc3181ea840]
               BT symbol: ./enzo.exe() [0x40cf49]
               Failure reported on processor 5
               ```
            * Code hangs when run on >1 2^x processors, outputs:
               ```bash
               star_maker9: density above thresh-hold - will now make a new star?!
               BigStarFormation: Star made at 0.496094, 0.503906, 0.511719 
                star_maker9: making new star, type = 10
               P(1): star_maker9[add]: 68 new sink particles
               BigStarFormation: Star made at 0.503906, 0.511719, 0.488281 
                star_maker9: making new star, type = 10
               P(0): star_maker9[add]: 68 new sink particles
               Grid_StarParticleHandler: New StarParticles = 68
               SinkParticle: Time = 0.045122891865029, TotalMass = 0
               CommunicationMergeParticle: total 0 != NumberOfGroups 1
               Group 0 failed to be added.
               m=0, x=(-nan, -nan, -nan), v=-nan
               Inserting merged particle into nearest grid.  RebuildHierarchy will place it
               into the correct grid in the next call.
               Adding particle to grid 0
               left edge  = 0.4375 0.4375 0.4375
               right edge = 0.5625 0.5625 0.5
               after adding particle to grid in Processor (1)
               inside debug, Processor(0)
               after adding particle to grid in Processor (0)
               ^C
               ```
         + ***reduce density to form sink particles in 1 timestep and merge in another timestep***
   * There are 20 data dumps. Each data dump corresponds to 0.5 kyr.
