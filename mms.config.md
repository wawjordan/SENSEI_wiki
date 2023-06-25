An example mms.config file is given in `SENSEI_Test_Cases`.
This file can contain the settings for multiple different manufactured solutions (MS) and even several different versions (instances) of the same MS.
Each MS is a section and then there can be as many sections as desired (one for each type of MS).
Each section will have the following format.

# Header (2 lines)
The first line of the header is the name of the BC.
The allowable names are given below.
The second line is 3 integers: number of MS instances (nMS), number of equations (neq), number of inputs per equation (ninputs).
The second two numbers must match the number expected by each type of MS (given below).
Then there will be nMS blocks in the rest of the section.

# Blocks
The first line of the block is mms_number to choose which instance of the MS you want.
Then the following neq lines will be lists of inputs for each of the equations.
And each line should have ninputs entries for the real(dp) inputs for the MS.
The contents of these lines are given below.
All angles are input in degrees.
For a more in depth discussion of each of the MS look in the header of each module.

# MS Types
## Analytic Solutions

### Simple Sinusoidal
### Crossterm Sinusoidal
    Name: crossterm_sinusoidal
    Inputs: a1-a20  length (this line is repeated 5 times, once for each prim variable)
    Default Values: Supersonic case (see file)

## Flowlike Solutions

### Supersonic Vortex Flow
    Name: svf
    Inputs: radius  rho  p  mach (on inner surface)
    Default Values: 2.0  1.0  12780.0  2.0

### Expansion Fan
    Name: euler2d_expansionfan
    Inputs: p  T  M (inflow) turn angle
    Default Values: 101325.0  273.16  1.2  12.0

### Shock
    Name: euler2d_shock
    Inputs: p  T  M (inflow) turn angle
    Default Values: 100000.0 300.0 2.0 10.0

### Diamond Airfoil
    Name: euler2d_diamond
    Inputs: p  T  M (inflow) wedge_half_angle
    Default Values: 100000.0 300.0 1.75 15.0

### Potential Cylinder
    Name: cylinder
    Inputs: rhoinf  pinf  vinf  alpha
    Default Values: From sensei.nml flow init variables

### Mapped Airfoil
    Name: joukowsky or karman-trefftz
    Inputs: rhoinf  pinf  vinf  alpha  mux  muy  te_angle
    Default Values: From sensei.nml flow init variables
                    mu = (-.01, 0.0) te_angle = 0.1 rad