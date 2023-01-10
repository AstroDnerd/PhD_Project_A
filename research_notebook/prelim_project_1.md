
# **TITLE**: ENZO Bootcamp
**About** : An unofficial guide to using ENZO and YT.

**Start Date**: 10/31/2022

**End Date**: 11/18/2022


## Getting ENZO: 
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

## Enzo Makefiles:
   change to src/enzo/ and:
   ```bash
   ls Make.mach.*
   ``` 
   shows list of potential pre-made Makefiles. We run:
   ```bash
   make machine-cloudbreak
   ```

## Updating PATH:
   * install yt-conda from: https://yt-project.org/
   * run the install script and add the yt-conda path to the path variable
   * add */usr/lib64/openmpi/bin* to the path variable for openmpi
   * add */usr/lib64:/usr/lib64/openmpi/lib* to *LD_LIBRARY_PATH*

## Building Enzo:
   ```bash
   make show-config
   ```
   will show the current coniguration
   ```bash
   make help-config
   ```
   will describe how to turn these options on or off
   ```bash
   make
   ```
   compile enzo and produce an enzo.exe

## Analysing with yt:
   * basic yt analyses commands are available here: https://yt-project.org/doc/quickstart/index.html
   * getting stats for the data:
      * ```ds = yt.load("path_to_data")```: loads the data
      * ```ds.print_stats()```: shows some statistics related to the simulation 
      * ```ds.field_list```: shows all the fields in the data
      * ```ds.derived_field_list```: shows all the possible field yt can derive from the current fields
   * one can also use yt to select data in a specified region (see link for more details)
   * visualizing the data: yt can make many different types of plots like ProjectionPlot, ParticlePlot, SlicePlot, etc; https://yt-project.org/doc/visualizing/plots.html

## Running a simulation (Hydro/Hydro-3D/CollapseTestNonCosmological)!
   copy enzo.exe to *run/Hydro/Hydro-3D/CollapseTestNonCosmological* directory
   ```bash
   cp enzo.exe ../../run/Hydro/Hydro-3D/CollapseTestNonCosmological
   ```
   execute enzo in that directory with debug mode
   ```bash
   ./enzo.exe -d CollapseTestNonCosmological.enzo
   ```

   **Purpose**: This test problem initializes a constant density sphere at the center of the box with radius of 0.15 of the box width. The sphere is in pressure equilibrium with its surroundings. The sphere will collapse dynamically and reach its peak density at t ~ 5.2 in code units. Radiative cooling is turned off, so the sphere will bounce and reach an equilibrium state. This test runs to completion (t = 7) and produces 71 outputs in roughly an hour on a single processor.

## Running a simulation (GravitySolver/GravityTest)!
   **Purpose**: To check the Gravity Solver of Enzo. 
   This test places a single, massive particle at the center of the box and then places 5000 nearly massless particles throughout the rest of the box, randomly spaced in log radius from the center, and randomly placed in angle. A single small step is taken, and the velocity is then divided by the timestep to measure the acceleration. A single subgrid is placed in the center from 0.4375 to 0.5625 (in units where the box size is 1.0). 

   This tests the acceleration of the particles from a single point mass and so can be directed compared to the r^-2 expected result.  An output file is generated, called TestGravityCheckResults.out, which contains four columns with one entry for each of the 5000 particles.  The columns are the radius (in units of the cell length of the most refined grid), the tangential component of the measured force, as computed by the code,  the radial component of the computed force, and finally the "true" (unsoftened) force. 

   The tangential component of the force should be zero; the radial component should follow the r^-2 law, but is softened for radii less than about one cell length (or slightly larger). It also falls below r^-2 are large distances because this problem uses periodic boundary conditions (The code has been modified since this problem was originally written to use isolated boundary conditions, but this problem has not been changed).

   The test for this problem is to compute the rms force error between 1 and 8 cell distances from the center. For CIC, this is measured to be about 5%.  The test checks to make sure this has not changed by more than 1%. This is not an ideal check, as many errors could conceivably escape undetected (e.g. those having to do with force errors at small and large radii); however, the problem with a bitwise comparison is that the positions of the 5000 particles are random (with no setable seed).

 
## Running a simulation (StarParticle/StarParticleSingleTest)!
   **Purpose**: This test places a single star particle in the center of a box with uniform gas density and thermal energy.  The gas is initially at rest.  The particle will then produce feedback according to the method set for StarParticleFeedback. By default, the star particle produces feedback with method 14, kinetic feedback.  An inital timestep is set to account for the large bulk velocities created by the feedback event in the first timestep. The particle is also set to be at rest by default, but it can be given a motion relative to the gas with the parameter TestStarParticleStarVelocity = vx vy vz, where vx, vy and vz have units of km/s.

   
## Running a simulation (MHD/2D/RayleighTaylor_CT_Suppressed)!
   **Purpose**: MHD suppresses the Rayleigh Taylor instability (in 2D) This is set with a magnetic field that is just slightly below the critical value for complete suppression, but one does see a severe reduction in growth rate and secondary instability. Fun things to try include reducing the field strength and changing the direction!

## Running a simulation (MHD/2D/SedovBlast-MHD-2D-Fryxell)!
   **Purpose**: This test sets up a two-dimensional blast wave problem for MHD.  While the initial condition  essentially describes a circular overpressurized region in a low-pressure magnetized medium, more detailed description of the initial set up can be found in the papers above.  This test problem is new for enzo2.0.  It uses the new Stanford hydro_rk solver.

   Unfortunately for some users, some of the key parameters are hard-coded in Grid_MHD2DTestInitializeGrid.C.  Depending on B-field values, this test can be a pure Sedov blast wave problem, or a Gardiner blast wave problem.  The current parameter will give Sedov blast wave solution.
      1. Setting LowerBx=LowerBy=0 will give the traditional Sedov test with no B-field, essentially very similar to SedovBlast-AMR.enzo; but with different hydro solver (Stanford HD/MHD) 
      2. Setting LowerBx=LowerBy=5 will give the Sedov blast with the presence of B-field, very similar to SedovBlast-MHD-2D-Gardiner.enzo; but the exact setup is somewhat different (see the code)
   Success in *test_fryxell.py* is determined by nearly exact match (3e-5) in Density and Pressure. 

## Running a simulation (MHD/2D/SedovBlast-MHD-2D-Gardiner)!
   **Purpose**: This test sets up a two-dimensional blast wave problem for MHD, and has become a useful standard test for any MHD solver.  While the initial condition  essentially describes a circular overpressurized region in a low-pressure magnetized medium, more detailed description of the initial set up can be found in the papers above.  This test problem is new for enzo2.0.  It uses the new Stanford hydro_rk solver.

   Unfortunately for some users, most of the key parameters are hard-coded in Grid_MHD2DTestInitializeGrid.C.  With zero B-field, this test should be a pure Sedov blast wave problem.    

   This test runs to completion while generating 12 outputs, and *scripts.py* will produce the plots for Density, x-velocity, By, Internal Energy for the last snapshot (t=0.02).  This last snapshot should be compared to Figure 13 of Gardiner & Stone (2005) or Figure 16 of Wang & Abel (2009)

   Success in *test_gardiner.py* is determined by nearly exact match (3e-5) in Density, Pressure, Bx, and By. 

## Running a simulation (Hydro/Hydro-2D/KelvinHelmholtz)!
   **Purpose**: The KH Test problem creates two fluids moving antiparallel to each other in a periodic 2D grid (inner fluid and outer fluid).  The inside fluid has a higher density than the outside fluid.  There is a slight ramp region in density and x-velocity connecting the two regions so there are no discontinuities in the flow.  The y-velocity is perturbed with small sinusoidal perturbation.  As the flows shear past each other, the KH instability is excited, which develops over time.  This test watches the evolution of those instabilities.  --Cameron Hummels, 2013

## Running a simulation (Hydro/Hydro-2D/KelvinHelmholtzAMR)!
   **Purpose**: Same as KH test before but with AMR. This version incorporates 1 level of AMR using the shear criterion on the interface between the two fluids.  --Cameron Hummels, 2013

## Running a simulation (Hydro/Hydro-2D/SedovBlast)!
   **Purpose**: Simulates a Sedov Blast. 

## Running a simulation (Hydro/Hydro-3D/Athena-RayleighTaylor)!
   **Purpose**: Classic Rayleigh Taylor setup with sharp contact. Compare to http://www.astro.princeton.edu/~jstone/tests/rt/rt.html 

## Running a simulation (Hydro/Hydro-3D/NFWCoolCoreCluster)!
   **Purpose**: The NFW Cool-Core Cluster is a simulation of the cooling flow in an idealized cool-core cluster that resembles Perseus cluster. It can be a test for cooling, and maybe gravity too. 

   The default set up is with a root grid of 64^3, a maximum refinement level of 12, and MinimumMassForRefinementLevelExponent of -0.2 (for better resolution, use -1.2) which can be changed based on the resolution one needs.

   The default set up has a static gravity and no self-gravity of the gas since the latter is much smaller than the gravity of the dark matter and does not change much during the early stage of the cooling flow evolution.

   As the cooling catastrophe happens, the temperature drops to the bottom of the cooling function ~ 10^4 K in the center within ~1kpc with the default resolution. The size of the region becomes smaller with higher resolution.

   The projected gas density shows a disk of size ~ 1kpc (inside the cooling catastrophe region) at late times along z axis which is the direction of the initial angular momentum of the gas.  

## Running a simulation (Hydro/Hydro-3D/Noh3D)!
   **Purpose**: The Noh Problem test sets up a a uniform gas of density of 1.0 that has a uniform inward radial velocity of 1.0

   Noh (1987) J. Comp. Phys. 72, 78 introduced an infinite shock reflection problem that has an exact analytical solution. Gas with initially uniform density of 1 and zero pressure converges onto the origin with a uniform radial velocity of 1 in a unit domain x, y in [0, 1].

   In spherical geometry (3D) the postshock density is 64 and in the preshock region the density varies as (1 + t/sqrt(x^2 + y^2))^2. All other dependencies remain the same as in the 2D case.

   We set the initial pressure to be 10^-6 instead of zero for numerical reasons. We use reflecting boundaries at x=0 and at y=0 and set up the outer boundaries at x=1 and y=1 based on the exact analytical solution.  We follow the propagation of the shock until it reaches a radius of 2/3 at t=2. At this point we compare our results with similar tests performed with other Eulerian numerical schemes, see Liska & Wendroff (2003), Section 4.5 in this PDF document or in SIAM J. Sci. Comput. 25, 995, 2003. See also Rider (2000), J. Comp. Phys. 162, 395 for a discussion of "wall heating" phenomenon near the origin that seriously affects the results obtained with Lagrangian schemes. Numerically, this is a difficult problem.



 
