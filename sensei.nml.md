You can create a default namelist by running SENSEI in a folder with an empty (or incorrect) sensei.nml file.
When SENSEI finds errors in the namelist file it will produce an error and print out the default namelist (in `sensei.nml.updated`).

This wiki page explains what each entry means and other useful information about the namelist.
If you notice any omissions or discrepancies please fix or ask the person who changed the namelist to fix.
The namelist is split into groups (the headings on this page) with several variables in each group.
For each variable a description, data type, and default value must be given.
If applicable also give a list of possible values, other special values, or general notes.

TODO: make a script and format namelist.F90 so that this list is automatically generated.

# project
* `job_name`
    ```
    Description: Name of the current job. Same name as the grid and BC file
                 (<job_name>.grd, <job_name..bc).
    Data Type: string
    Default Value: 'sensei'
    ```
* `flow_eqn`
    ```
    Description: Governing equations being solved.
    Data Type: string
    Default Value: 'euler'
    Possible Values: 'euler'      - inviscid Euler equations
                     'laminar_ns' - laminar Navier-Stokes equations
                     'rans'       - turbulent Reynolds-Averaged Navier-Stokes
                                    equations (will read turb.nml)
    ```
* `is_axisymmetric`
    ```
    Description: Should the 2D grid be treated as axisymmetric?
    Data Type: boolean
    Default Value: false
    ```
* `desired_primal_order`
    ```
    Description: What order scheme is desired?
    Data Type: Integer
    Default Value: 1
    Possible Values: 1 or 2 (2 is for MUSCL)
    ```
* `temporal_order`
    ```
    Description: What order scheme is desired for the temporal terms?
    Data Type: Integer
    Default Value: 1
    Possible Values: 1 or 2
    ```
* `verbose_level`
    ```
    Description: What level of warnings do you want?
    Data Type: Integer
    Default Value: 0
    Special Values: 0 - Only see always warning
                    1 - Only see always and sometimes warning
                    2 - See all warnings
    ```
* `primal_restart`
    ```
    Description: Are you starting from a previous run
                 (starts from <job_name>.rst or <job_name>-#.rst (if using MPI)) or not?
    Data Type: boolean
    Default Value: false
    ```
* `restart_limiters`
    ```
    Description: Are you starting the limiter values from a previous run
                 (starts from <job_name>.rst or <job_name>-#.rst (if using MPI)) or not?
    Data Type: boolean
    Default Value: false
    Note: If a previous run does not use a limiter, then setting this to be true will cause issues.
    ```
* `restart_wall_distance`
    ```
    Description: Are you starting the wall distance values from a previous run
                 (starts from <job_name>.rst or <job_name>-#.rst (if using MPI)) or not?
    Data Type: boolean
    Default Value: true
    ```
* `restart_save_freq`
    ```
    Description: How often do you want to output a restart file
                 (<job_name>_[ab].rst or <job_name>-#._[ab].rst(if using MPI))?
    Data Type: integer
    Default Value: -1
    Special Values: -1 - only save at the final iteration
    ```
* `residual_print_freq`
    ```
    Description: Sets how often to print the residual to screen and file.
    Data Type: integer
    Default Value: 1
    ```
* `soln_save_freq`
    ```
    Description: Sets how often to write the solution to a file.
    Data Type: integer
    Default Value: -1
    Special Value: -1 - only save the final solution
    ```
* `system_out_freq`
    ```
    Description: Sets how often to write the linear system to a file.
    Data Type: integer
    Default Value: -1
    Special Value: -1 - only save the final linear system
    ```
* `force_out_freq`
    ```
    Description: Sets how often to write the forces to a file.
    Data Type: integer
    Default Value: -1
    Special Value: -1 - only save the final force
    ```
* `write_final_soln`
    ```
    Description: Should SENSEI write the solution at the final step or not?
    Data Type: boolean
    Default Value: T
    ```
* `show_phys_check_info`
    ```
    Description: Should SENSEI report warnings if there is any non-physical values?
    Data Type: boolean
    Default Value: F
    ```
* `output_yplus`
    ```
    Description: Should the turbulence modeling in SENSEI output the yplus value?
    Data Type: boolean
    Default Value: F
    ```

# project
* `verif`
    ```
    Description: This is a variable for unsteady flow code verification
    Data Type: boolean
    Default Value: F
    Possible Values: T - temporal time step size is refined at the same as the spatial grid size
                     F = temporal time step size is fixed for all levels of spatial grids

# grid
* `rst_input_fmt`
    ```
    Description: Format for inputting restart file.
    Data Type: string
    Default Value: 'ascii'
    Possible Values: 'ascii' - human readable
                     'sdf'   - binary format
    ```
* `rst_output_fmt`
    ```
    Description: Format for outputting restart file.
    Data Type: string
    Default Value: 'ascii'
    Possible Values: 'ascii' - human readable
                     'sdf'   - binary format
    ```
# reference
* `dimensional`
    ```
    Description: Do you want to run dimensional (true) or non-dimensional (false)
    Data Type: boolean
    Default Value: false
    ```
* `l_ref`
    ```
    Description: Dimensional reference length
    Data Type: real
    Default Value: 1.0
    ```
* `l_ref_grid`
    ```
    Description: Reference length in non-dimensional grid units
    Data Type: real
    Default Value: 1.0
    ```
* `rho_ref`
    ```
    Description: Reference density used for non-dimensionalization
    Data Type: real
    Default Value: 1.0
    ```
* `a_ref`
    ```
    Description: Reference speed of sound used for non-dimensionalization
                 (does not need to correspond to t_ref)
    Data Type: real
    Default Value: 1.0
    ```
* `t_ref`
    ```
    Description: Reference temperature used for non-dimensionalization
    Data Type: real
    Default Value: 1.0
    ```
* `vmag_ref`
    ```
    Description: Reference velocity magnitude used for computing coefficients
    Data Type: real
    Default Value: 1.0
    ```

# fluid
* `gamma`
    ```
    Description: Ratio of specific heats.
    Data Type: real
    Default Value: 1.4
    ```
* `pr`
    ```
    Description: The (laminar) Prandtl number.
    Data Type: real
    Default Value: 0.72
    ```
* `pr_t`
    ```
    Description: The turbulent Prandtl number.
    Data Type: real
    Default Value: 0.9
    ```
* `mol_weight`
    ```
    Description: Molucular weight in kg/kmol.
    Data Type: real
    Default Value: 28.97
    Special Values: 28.97  - air
                    31.999 - oxygen
                    28.013 - nitrogen
    ```
* `mu`
    ```
    Description: Viscosity in kg/(m*s). Used when viscosity_law = 'constant'.
    Data Type: real
    Default Value: 1.7894e-5
    ```
* `viscosity_law`
    ```
    Description: Viscosity law to use.
    Data Type: string
    Default Value: 'sutherlands'
    Possible Values: 'sutherlands' - Sutherland's Law
                     'constant'    - use value of mu given
    ```

# flux
* `higher_order_scheme`
    ```
    Description: Scheme for higher order methods (for the spatial terms).
    Data Type: string
    Default Value: 'muscl'
    Possible Values: 'muscl'
                     'kexact' (Not fully implemented independently, may need to use it with the reconstruction)
                     'dseno'  (Currently disabled)
    ```
* `flux_scheme`
    ```
    Description: Flux scheme to use.
    Data Type: string
    Default Value: 'vanleer'
    Possible Values: 'vanleer'
                     'roe'
                     'sw'
                     'swb'
                     'ausm'
                     'ausm+'
    ```
* `kappa`
    ```
    Description: Kappa variable for muscl extrapolation.
    Data Type: real
    Default Value: 1/3
    Special Values: -1  - Fully up-winded
                    0   - up-wind biased
                    1/3 - 3rd order upwind biased
                    1/2 - Leonard's QUICK
                    1   - central
    ```
* `mean_flux_lim`
    ```
    Description: Mean flow flux limiter to use.
    Data Type: string
    Default Value: 'none'
    Possible Values: 'none'
                     'ospre'
                     'vanalbada'
                     'vanleer'
                     'minmod'
                     'superbee'
                     'michalak'
                     'first_order'
    ```
* `turb_flux_lim`
    ```
    Description: Turbulence flow flux limiter to use.
    Data Type: string
    Default Value: 'none'
    Possible Values: 'none'
                     'ospre'
                     'vanalbada'
                     'vanleer'
                     'minmod'
                     'superbee'
                     'michalak'
                     'first_order'
    ```
* `freeze_mean_lim`
    ```
    Description: Iteration number at which to freeze the mean flux limiters.
    Data Type: integer
    Default Value: 5000000
    Special Values: -1 (never freeze the mean flux limiters)
    Note: Setting the value to be -1 will cause the restart to freeze mean limiters in the beginning of a restart
    ```
* `freeze_turb_lim`
    ```
    Description: Iteration number at which to freeze the turb flux limiters.
    Data Type: integer
    Default Value: 5000000
    Special Values: -1 (never freeze the turb flux limiters)
    Note: Setting the value to be -1 will cause the restart to freeze turb limiters in the beginning of a restart
    ```
* `mean_no_lim`
    ```
    Description: Whether limiters are applied to the mean flow inviscid flux?
    Data Type: boolean
    Default Value: T
* `turb_no_lim`
    ```
    Description: Whether limiters are applied to the turb flow inviscid flux?
    Data Type: boolean
    Default Value: T
* `turb_1st_order`
    ```
    Description: Whether using 1st order for the turbulence inviscid flux?
    Data Type: boolean
    Default Value: T

# iterative_solver
* `which_iterative_solver`
    ```
    Description: Type of implicit iterative solver to use.
    Data Type: string
    Default Value: 'gmres'
    Possible Values: 'gmres'
                     'bicgstab' (disabled)
                     'cg'       (disabled)
                     'cgnr'     (disabled)
                     'cgne'     (disabled)
                     'bicg'     (disabled)
                     'cgs'      (disabled)
    ```
* `restart`
    ```
    Description: Number of restarts in the Krylov subspace method.
    Data Type: integer
    Default Value: 50
    ```
* `n_iterations`
    ```
    Description: Maximum number of iterations for the linear solver.
    Data Type: integer
    Default Value: 100
    ```
* `itsol_absolute_tolerance`
    ```
    Description: Absolute tolerance for the linear solver
    Data Type: real
    Default Value: 1e-10
    ```
* `itsol_relative_tolerance`
    ```
    Description: Relative tolerance for the linear solver
    Data Type: real
    Default Value: 1e-6
    ```
* `preconditioner`
    ```
    Description: Relative tolerance for the linear solver
    Data Type: string
    Default Value: 'none'
    Possible Values: 'none'
                     'ilu0'
                     'iluk'
                     'ilutp'
    ```
* `precond_side`
    ```
    Description: Side to apply the preconditioning to
    Data Type: string
    Default Value: 'none'
    Possible Values: 'left'
                     'right'
                     'split'
    ```
* `ilu_lfil`
    ```
    Description: Incomplete LU Factorization w/ Fill In
    Data Type: real
    Default Value: 40 (must be no less than 0)
    ```
* `ilu_droptol`
    ```
    Description: Incomplete LU Factorization drop tolerance
    Data Type: real
    Default Value: 1.0E-4 (must be no less than 0)
    ```
* `jacobian`
    ```
    Description: Type of LHS jacobian to be used (can be different than RHS for
                 stability).
    Data Type: string
    Default Value: 'vanleer'
    Possible Values: 'vanleer'
                     'roe'
                     'sw'
                     'swb'
                     'ausm'
                     'ausm+'
    Note: vanleer and steger-warming appear to be the most stable, buf more oftern roe is used.
    ```

# time_integration
* `time_integrator`
    ```
    Description: Solver type.
    Data Type: string
    Default Value: 'explicit'
    Possible Values: 'explicit'
                     'implicit'
    ```
* `time_accurate_sim`
    ```
    Description: Is this a time accurate simulation?
    Data Type: boolean
    Default Value: false
    ```
* `n_nonlinear_iterations`
    ```
    Description: Number of iterations to perform
    Data Type: integer
    Default Value: 1
    ```
* `convergence_tolerance`
    ```
    Description: Tolerance for iterative residual convergence
    Data Type: real
    Default Value: 1e-10
    Note: this is an absolute value, not relative value
    ```
* `first_order_lhs`
    ```
    Description: Do you want to use first order for the LHS?
    Data Type: boolean
    Default Value: true
    ```
* `ramp_order`
    ```
    Description: Do you want to ramp up to desired_order?
    Data Type: boolean
    Default Value: false
    ```
* `ramp_order_tolerance`
    ```
    Description: If ramp_order then what residual level to ramp order?
    Data Type: real
    Default Value: 1.0E-4
    Special Value: Typically 1.0E-4
    ```
* `rk_steps`
    ```
    Description: Number of Runge-Kutta  steps for explicit scheme
    Data Type: integer
    Default Value: 0
    Special Values: usually 0, 1, 2, 3, or 4
    Note: implicit backward Euler requires temporal_order = 1 and rk_steps = 0
          three point backward requires temporal_order = 2 and rk_steps = 0
          implicit RK multi-step requires temporal_order = 2 or 4, and rk_steps = 1, 2 or 3
          explicit RK multi-step requires temporal_order = 1, 2 or 4, and rk_steps = 1, 2 or 4
          for implicit RK multi-step, if temporal_order = 2, rk_steps = 1 or 2 or 3
          for explicit RK multi-step, if temporal_order = 1, rk_steps = 1 or 2 or 4

    ```
* `cfl_init`
    ```
    Description: Initial CFL number for solve.
    Data Type: real
    Default Value: 1.0
    ```
* `cfl_max`
    ```
    Description: Maximum CFL number for solve.
    Data Type: real
    Default Value: 1000000.0
    ```
* `start_cfl_advance_iter`
    ```
    Description: Iteration number to CFL start advancement scheme.
    Data Type: integer
    Default Value: 2
    ```
* `cfl_advance_scheme`
    ```
    Description: Which method to advance the CFL number.
    Data Type: string
    Default Value: 'constant'
    Possible Values: 'constant'
                     'ramp'
                     'SER'
                     'EXPur'
                     'RDM'
                     'mRDM'
    Note: For more details, please look into the code. Usually, the 'constant' scheme is used, since the CFL # can be adjusted through cfl.dat
    ```
* `ramp_end_iter`
    ```
    Description: Final iteration to end CFL ramping.
    Data Type: integer
    Default Value: 1
    ```
* `ramp_end_cfl`
    ```
    Description: Value of CFL to ramp to.
    Data Type: real
    Default Value: 1.0
    ```
* `expur_omega_min`
    ```
    Description: Constant for CFL advancement scheme.
    Data Type: real
    Default Value: 1.0E-2
    Note: For more details, please look into the code.
    ```
* `expur_beta`
    ```
    Description: Constant for CFL advancement scheme.
    Data Type: real
    Default Value: 1.05
    Note: For more details, please look into the code.
    ```
* `expur_kappa`
    ```
    Description: Constant for CFL advancement scheme.
    Data Type: real
    Default Value: 0.1
    Note: For more details, please look into the code.
    ```
* `rdm_beta`
    ```
    Description: Constant for CFL advancement scheme.
    Data Type: real
    Default Value: 2.0
    Note: For more details, please look into the code.
    ```
* `prescribed_dt`
    ```
    Description: Do you want to use a prescribed value for the time step?
    Data Type: boolean
    Default Value: false
    ```
* `dt_set`
    ```
    Description: Time step size when prescribed_dt = true
    Data Type: real
    Default Value: 0.001
    ```
* `global_dt`
    ```
    Description: Do you want to perform global (true) or local (false)
                 time-stepping?
    Data Type: boolean
    Default Value: false
    ```
* `check_soln_tolerance`
    ```
    Description: Residual level to stop checking solution physicality
    Data Type: real
    Default Value: 1.0e-6
    ```
* `reject_local_update`
    ```
    Description: This variable may not be used. Setting it to be True for unsteady simulations
    Data Type: boolean
    Default Value: false
    ```

# flow_init
* `use_mach_init`
    ```
    Description: Use mach_init (true) or vmag_init (false)
    Data Type: boolean
    Default Value: true
    ```
* `mach_init`
    ```
    Description: Initial condition for Mach number
    Data Type: real
    Default Value: 1.0
    ```
* `vmag_init`
    ```
    Description: Initial condition for velocity magnitude number
    Data Type: real
    Default Value: 100.0
    ```
* `p_init`
    ```
    Description: Initial condition for pressure
    Data Type: real, dimension(11)
    Default Value: 101325.0
    ```
* `t_init`
    ```
    Description: Initial condition for temperature
    Data Type: real
    Default Value: 300
    ```
* `alpha_init`
    ```
    Description: Angle of attack for initial condition
    Data Type: real
    Default Value: 0.0
    ```
* `beta_init`
    ```
    Description: Side-slip angle for initial condition
    Data Type: real
    Default Value: 0.0
    ```

# bc
* `bc_order`
    ```
    Description: Order of accuracy for the boundary conditions.
    Data Type: integer
    Default Value: 2
    ```

# exact
* `use_exact`
    ```
    Description: Does an exact solution exist that you want to use?
    Data Type: boolean
    Default Value: false
    ```
* `exact_solution`
    ```
    Description: Exact solution to use.
    Data Type: string
    Default Value: 'crossterm_sinusoidal'
    Possible Values:
    Note: there are multiple possible values for exact_solution, including:
    'crossterm_sinusoidal': crossterm sinusoial MMS (steady)
    'crossterm_sinusoidal_unsteady': crossterm sinusoial MMS (unsteady)
    'es_2Dunsteady_vortex': 2D Euler convecting flow (unsteady)
    etc.
    ```
* `initialize_exact`
    ```
    Description: Do you want to initialize your solution to the exact solution?
    Data Type: boolean
    Default Value: false
    ```
* `mms_number`
    ```
    Description: MMS case to use.
    Data Type: int
    Default Value: -1
    Possible Values: it depends on what you have in your mms.config
    ```
* `mms_config_file`
    ```
    Description: MMS input file to read different cases.
    Data Type: string
    Default Value: 'mms.config'
    ```
* `flux_source`
    ```
    Description: Use the weak (true) or strong (false) to calculate the
                 source term
    Data Type: boolean
    Default Value: true
    ```
* `expfan_origin`
    ```
    Description: Corner location for the expansion fan case
    Data Type: real, dimension(2)
    Default Value: 2*-0.4
    Note: this is for the expansion fan MMS case
    ```
* `shock_origin`
    ```
    Description: Corner location for the shock case
    Data Type: real, dimension(2)
    Default Value: 2*0.0
    Note: this is for the shock MMS case
    ```

# tecplot_io
* `prim_print`
    ```
    Description: Do you want to output primitive (true) or conserved (false)
                 variables?
    Data Type: boolean
    Default Value: true
    ```
* `source_print`
    ```
    Description: Do you want to output source terms?
    Data Type: boolean
    Default Value: false
    ```
* `resid_print`
    ```
    Description: Do you want to output residuals?
    Data Type: boolean
    Default Value: false
    ```
* `lim_print`
    ```
    Description: Do you want to output limiter values?
    Data Type: boolean
    Default Value: false
    ```
* `print_ghost_cells`
    ```
    Description: DO you want to include ghost cells in output?
    Data Type: boolean
    Default Value: false
    ```

# te_estimation
Note: this scope is only for truncation error estimation
* `kexact_order`
    ```
    Description: ???
    Data Type: integer
    Default Value: 4
    ```
* `weak_form`
    ```
    Description: Are you using the flux form or the ghost cell form?
    Data Type: boolean
    Default Value: false
    Note: Setting the value to be true means using the flux form, otherwise the ghost cell form.
    ```
* `grid_order`
    ```
    Description: ???
    Data Type: integer
    Default Value: 1
    ```
* `te_method`
    ```
    Description: ???
    Data Type: string
    Default Value: 'exact'
    Possible Values: ???
    ```
* `compute_all_te`
    ```
    Description: ???
    Data Type: boolean
    Default Value: false
    ```
* `node_output`
    ```
    Description: ???
    Data Type: boolean
    Default Value: false
    ```
* `interp_order`
    ```
    Description: Interpolation order?
    Data Type: integer
    Default Value: 1
    ```
* `te_filter_size`
    ```
    Description: ???
    Data Type: integer
    Default Value: 1
    ```
* `te_adapt`
    ```
    Description: ???
    Data Type: boolean
    Default Value: false
    ```

# de_estimation
* `residual_de_method`
    ```
    Description: whether to use defect correction or ete, or only calculate truncation error?
    Data Type: string
    Default Value: 'none'
    Possible Values: "dc", "ete", "te only"
    ```
* `compute_residual_de`
    ```
    Description: whether to compute discretization error or not (for MMS)
    Data Type: boolean
    Default Value: false
    ```
* `use_ete`
    ```
    Description: Are you using the ETE to estimate error and reconstruct to a higher order solution?
    Data Type: boolean
    Default Value: false
    ```
* `use_exact_te`
    ```
    Description: Are you using exact truncation error?
    Data Type: boolean
    Default Value: false
    ```
* `linear_ete`
    ```
    Description: Are you using the linear ETE?
    Data Type: boolean
    Default Value: true
    ```
* `n_recursive_correct`
    ```
    Description: How many times do you want to recursively use the ETE?
    Data Type: integer
    Default Value: 0
    ```

# parallel
* `reorder_bc`
    ```
    Description: Whether reordering the bcs when the ROOT processor reads in all bcs in the beginning?
    Data Type: boolean
    Default Value: true
    ```

# adjoint_setup
* `adjoint`
    ```
    Description: Do you want adjoint solve?
    Data Type: boolean
    Default Value: false
    ```
* `adj_cfl`
    ```
    Description: CFL number for adjoint solve.
    Data Type: real
    Default Value: 50
    ```
* `adj_iterations`
    ```
    Description: Max number of iterations for adjoint solve.
    Data Type: integer
    Default Value: 1000

# quadrature
* `quad_order`
    ```
    Description: Order of polynomial to integrate exactly
    Data Type: integer
    Default Value: 1
    ```
* `quad_rule`
    ```
    Description: Quadrature rule to use
    Data Type: string
    Default Value: 'gauss'
    Possible Values: 'gauss'
    ```
* `gauss_quad_polynomial`
    ```
    Description: Basis function to use for definition of quadrature
    Data Type: string
    Default Value: 'legendre'
    Possible Values: 'legendre'
    ```

# reconstruction
* `stencil_type`
    ```
    Description: Which type of stencil to use
    Data Type: string
    Default Value: 'kexact'
    Possible Values: 'kexact'
    ```
* `reconstruct_order`
    ```
    Description: Order of polynomial to use
    Data Type: integer
    Default Value: 1
    ```
* `ls_factor`
    ```
    Description: ???
    Data Type: real
    Default Value: 1.0
    ```
* `constraint_factor`
    ```
    Description: Factor to apply to constraint rows
    Data Type: real
    Default Value: 1000.0
    ```
* `scale_reconstruct_lhs`
    ```
    Description: ???
    Data Type: boolean
    Default Value: false
    ```