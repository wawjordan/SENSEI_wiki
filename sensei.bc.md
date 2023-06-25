Example BC files are in the test cases and they are loosely related to gridgen boundary condition files.

# Header Portion (2 lines)
1. The first line is a gridgen label (not currently used).
    - [For the multi-mesh branch: 1=standard mesh system, 2=multi-mesh system].
2. The second line is the number of blocks (this must match the number of blocks in the grid).

Then each block will have a section for its BCs this section will be repeated for each block in order.

# Block Section
1. The first line is the number of nodes in each direction (for 2D there are 2 numbers).
2. The second line is label (not currently used).
    - [For the multimesh_turb branch: 'mean'=mean_flow, 'turb'=turb_flow, can use both if you want (use at your own risk)].
    - [For the multimesh branch: list of integers, first number is the number of equations solved on this mesh, then a list of the equations solved on the mesh].
3. The third line is the number of boundary conditions (nBC) specified for this block.

Then the next nBC lines specify the boundary conditions.


# BC Line
Each BC line starts with a boundary name first, followed by 2*dimension numbers, which are the start and end indices for the nodes that the BC applies to.
For example for a 3D case the first 6 numbers are ```xi_start  xi_end  eta_start  eta_end  zeta_start  zeta_end```
If 2D then there would only be the first 4 terms.

After these entries there is the BC number which corresponds to the BC type.
Then depending on the BC type there may be additional numbers that correspond to the parameters of each BC described below.

# BC Types
BC numbering can be found in `src/boundary_conditions/boundary_conditions.f90`

To get alpha and beta project velocity into x,z plane (call projection vector b).
Alpha is angle from b to V in degrees (angle of attack).
Beta is angle from x axis to b in degrees (sideslip angle).

## Wall [200-299]

* 201 = slip wall

		No additional inputs

* 202 = adiabatic wall

		No additional inputs

## Symmetry [300-399]

* 301 = symmetry

		No additional inputs

## Farfield [400-499]

* 401 = farfield

		Mach  Pressure  Temperature  Alpha  Beta

* 402 = farfield1

		Mach  Reynolds #  Temperature  Alpha  Beta

## Inflow [500-599]

* 501 = supersonic inflow

		Mach  Pressure  Temperature  Alpha  Beta

* 502 = subsonic inflow

		Velocity  Density  Alpha  Beta

* 503 = pressure inflow

		Total_Pressure  Total_Temperature  Alpha Beta

## Outflow [600-699]

* 601 = supersonic outflow

		No additional inputs

* 602 = subsonic outflow

		Pressure

## Interior [0,-10]

* -1 = connected

		xi_start  xi_end  eta_start  eta_end  [zeta_start  zeta_end] block#

This boundary condition works when boundary is physically touching (like c- or o-grids or multi-block), or when the boundary is periodic (SENSEI will detect it itself so the user do not need to worry about it). The order of the start and end indices does not matter and they should all be positive (unlike gridgen's BC). The code will check all of the corners and match them up appropriately.

* -2 = pressure jump [not working currently]

		xi_start  xi_end  eta_start  eta_end  [zeta_start  zeta_end]  block#  pressure_jump

`pressure_jump` is positive when pressure increases in the same direction as the outward facing normal and negative otherwise.

There are different numbering used inside the code for the interior BCs but these should not be input in the BC file.


## Mms [-200]

* -200 = mms

		No additional inputs