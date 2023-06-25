# Input Files
## Namelists
### sensei.nml
The main namelist for SENSEI.
A description of its options can be found here: [[sensei.nml]]
### other namelists
Some other capabilities of the code are controlled by other namelists.

The turbulence models are controlled by [[sensei_turb.nml]].
This namelist will only be read if `flow_eqn = 'rans'`.

TODO: add descriptions of other namelists


## Boundary Conditions
The boundary conditions are found in `<job_name>.bc`
A description of this file can be found here: [[sensei.bc]]

## MMS Input file
The mms inputs are found in a file `<mms_config_file>`, typically `mms.config`.
A description of this file can be found here: [[mms.config]]

# Interaction Files
There are several files that may be used to interact with SENSEI while it is running

* `freeze_limiter.dat`
* `cfl.dat`
* `stop.dat`

# Output Files
SENSEI creates several output files. Some of these files are created every time SENSEI is run, others are only output if certain routines are run or certain options are set.

## Restart File
SENSEI will always create a restart file `<job_name>.rst.<date+time>`.
It can also output a restart file during the run every `<restart_save_freq>` iterations (set in the project namelist).
With this option, SENSEI will output alternating files named `<job_name>-b.rst` and `<job_name>-b.rst`. These two files are created in case an error occurs while outputting the last restart file.

With these files it is possible to restart a problem in order to continue convergence or change some settings (change settings at your own risk!). In order to run a restart problem move the restart file to `<job_name>.rst` and set `<primal_restart> = T` in the namelist. Note some of the output files (convergence history) will be rewritten in their entirety (including iterations from before the restart) while others (solution output) will just be appended to the current file.

## Data Output Files

### General Tecplot Files
These files are output for every run (the frequency of output can be modified in the namelist). For most of these if the frequency is negative it will only output after the final iteration.

* `<job_name>-soln.dat`
    * Tecplot file
    * Saves the solution every `soln_save_freq` iterations
    * Will always save the grid and can optionally save other variables based off of the `tecplot_io` namelist    
        * Primitive variables
        * Source terms (includes the exact source for MMS)
        * Residuals
        * Limiters
    * Also has the option to output the solution including one row of ghost cells (useful for BC debugging)
* `<job_name>.primal.residual_history.dat`
    * Provides the following every `residual_print_freq` iterations:
        * Iteration #
        * Mean flow CFL value
        * Turbulence flow CFL value
        * LHS primal order
        * RHS primal order
        * L2 norm of residual for each equation
* `<job_name>.primal.block<block#>.iter<iter#>.[mean/turb]flow.[l/r]hs.dat`
    * LHS are RHS output for given block and iteration
    * These are very large files and so should only be used for debugging or other investigations
    * These are output every `system_out_freq` iterations
        * -1 never outputs these files (sould be used most of the time)
        * -2 only outputs the final iteration
* `sensei.nml.<date+time>`
    * After a succesful run the namelist is output with all of the variables (including defaults not set)
    * This is helpful to check if you want to make sure the default values make sense or remember which setting you used to run a problem.
* `sensei.nml.updated`
    * After a unsuccesful read of the namelist the namelist is output with all of the variables (including defaults not set)
    * This is helpful to check what SENSEI is expecting to read in
    * Also helpful if you dont have a namelist already just touch sensei.nml and run sensei and it will create the up to date namelist
* `soln_array.dat`
    * Output of the solution matrix to file
    * Used for debugging and MNP inputs
* `timing.txt`
    * Timing results from running case

### Force Output
* `<job_name>-surface_forces.dat`
    * Tecplot file
    * Saves the force on the surfaces every `force_out_freq` iterations
    * This are 3D surface plots of the forces on the boundaries (can be manipulated to create cp vs x 2D plots)
    * This file will always be set up but will be empty if there are no boundaries
    * NOTE! Currently cp is: the pressure coefficient if a far-field bc or a subsonic outflow exists; or the nondimensional pressure calculated by wall_p/(0.5\*rho\*vref^2)!
* `<job_name>-<block#>-force_history.dat`
    * Tecplot file
    * Saves pressure and viscous force and coefficients in x, y, and z (not lift and drag)
    * Saves them every <force_out_freq> iterations on the given block
    * This file will always be set up and have values even if no boundaries (0.0 forces)
* `<job_name>-press_force.dat`
    * Plain text file
    * Saves all the pressure forces at boundary faces
    * Saves at the final step
* `<job_name>-visc_force.dat`
    * Plain text file
    * Saves all the viscous forces at boundary faces
    * Saves at the final step

### Exact Solution Files
These files will only be output if an exact solution is being used.
TODO: deexact and solnexact need to be updated/combined!
* `deexact.dat`
    * Discretization error over the domain in all of the non-dimensional primitive variables
* `solnexact.dat`
    * Final numerical solution and exact solution over the domain in non-dimensional primitive variables
* `<job_name>-te.dat`
    * Truncation error over the domain
* `<job_name>-te_norms.dat
    * TE L2 and Linf norms
* `<job_name>.deexact`
    * Primitive L1 DE
    * Primitive L2 DE
    * Conserved L1 DE
    * Conserved L2 DE
    * TODO update this file to include names (like TE norms)
* `<job_name>-te_norms.dat`
    * L2 TE
    * Linf TE

### Adaptation Files
These files will only be output if running an adaptation problem.

* `<adaptation_scheme>_residual_history.dat`
    * adaptation convergence history for last adaptation outer iteration
* `<job_name>_adapted_aux_grids.dat`
    * Aux grids used for neuman BCs
    * These are output for all outer and inner iterations
    * TODO add flag to output these or not
* `<job_name>_adaptive_convergence.dat`
    * iter, L1/L2/Linf of each TE, norms of equidist error, norms of movement, norms of weight function
* `<job_name>_adapted_grids.dat`
    * x,y,A,TE,TE*A,w,equidistribution for every outer iter
* `<job_name>_anderson_adapt_grid_wf.dat`
    * x,y,w at last grid (not sure the use of this when you can just use adaptive_convergence.dat)
* `<job_name>_<adaptation_scheme>.grd`
    * Final adapted grid