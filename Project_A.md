
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
   * 




## Regular Updates



### November - December, 2022:
#### First steps:
   
   current stable source code can be found at:
   ```bash
   git clone https://github.com/enzo-project/enzo-dev    
   ```

## Setting up ENZO:
   Go to the ENZO directory and:
   ```bash
   ./configure
   ``` 
   This wipes out all configuration settings and restores them to defaults, usually run once
## Various subdirectories:
   * bin: 
   * doc: contains both older and newer documentation (manual/) and can be read in plaintext
   * input: files used as input to several test problems. Enzo startup fail is likely localized here
   * run: has example parameter files with notes about expected output and scripts for plotting. Also used to compare results of one version of code with previous version (Enzo answer test suite)
   * src: ENZO source and affiliated utilities

