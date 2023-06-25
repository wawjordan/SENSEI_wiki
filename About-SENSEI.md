## Contents

- [Discretization](#discretization)
- [Indexing Documentation](#indexing-documentation)

## Discretization
- Grids:
    - 2D / 3D
    - Structured
    - Single-Block / Multi-Block
- Governing Equations:
    - Euler / Navier-Stokes Equations
- Assumptions:
    - Perfect Gas, Negligible Body Forces
- Inviscid Fluxes:
    - Roe
    - Van Leer
    - Steger-Warming
    - Steger-Warming + Buning SP Fix
    - AUSM
    - AUSM+
- Slope Limiters:
    - OSPRE
    - Van Albada
    - Van Leer
    - minmod
    - superbee
    - Michalak
- Viscous Fluxes:
    - Green-Gauss Gradients

## Indexing Documentation

Since SENSEI is a cell-centered, rather than node-centered, finite volume code, there
is a need to create several sets of indices.

The first is the node indices, that go from 1 to imax, jmax, or kmax.
The second is the cell indices, which go from 1 to i_cells, j_cells, or k_cells.
This looks something like:

GC -1 | GC 0 |*****cell 1*****|*****cell 2*****| ..... |*****cell i_cells-1*****|*****cell i_cells*****| GC i_cells+1 | GC i_cells +2

    node 0  node 1           node 2           node 3  node i_cells-1           node icells          node imax      node imax+1

Where, GC stands for ghost cell. In SENSEI, two layers of ghost cells are used. Node 0 and node imax+1 are created through extrapolation so they are not from the grid (.grd) file.

The viscous flux requires a special sub grid, where control volumes are centered on each cell face. This is because the Green-Gauss gradients are applied to calculate the gradients on each cell face.
In SENSEI, the volumes are indexed by a direction and which cell/node they correspond to. This means that a sub cell centered on a xi face that goes between cells (1,3,4) and (2,3,4) would be indexed to (2,3,4) since it is centered at node 2, the node between cells (1,:,:) and (2,:,:).

This sub cell has six faces, two at cell centers and four centered on cell edges. For our example, the xi faces of (1,3,4) and (2,3,4), these are the two cell centered faces and have cell centered indices.

## References
All the references related to SENSEI can be found at https://drive.google.com/drive/u/0/folders/15fUBQCm8ThA9SyxBWPA-vrx7HuTzIOxV. Please e-mail weich97@vt.edu to get an access to the drive.

[1] Joseph M Derlaga, Tyrone Phillips, and Christopher J Roy. Sensei computational fluid dynamics code: a case study in modern fortran software development. In 21st AIAA Computational Fluid Dynamics Conference, 2013.

[2] Charles W Jackson, William C Tyson, and Christopher J Roy. Turbulence model implementation and verification in the sensei cfd code. In AIAA Scitech 2019 Forum, 2019.

[3] Weicheng Xue, Hongyu Wang, and Christopher J Roy. Code verification for 3d turbulence modeling in parallel sensei accelerated with mpi. In AIAA Scitech 2020 Forum, 2020.

[4] Weicheng Xue, Honyu Wang, and Christopher J Roy. Code Verification for 2D Unsteady Flows in SENSEI. In AIAA Scitech 2021 Forum, 2021.

[5] Hongyu Wang, Weicheng Xue, and Christopher J Roy. Error Transport Equation Implementation in the SENSEI CFD Code. In AIAA Scitech 2021 Forum, 2020.

[6] Hongyu Wang, Weicheng Xue, and Christopher J Roy. Discretization Error Estimation Using the Unsteady Error Transport Equations. In AIAA Scitech 2021 Forum, 2021.

[7] Weicheng Xue, Charles W Jackson, and Christopher J Roy. Multi-cpu/gpu parallelization, optimization and machine learning based autotuning of structured grid cfd codes. In 2018 AIAA Aerospace Sciences Meeting, page 0362, 2018.

[8] Weicheng Xue and Christopher J Roy. Heterogeneous computing of cfd applications on cpu-gpu platforms using openacc directives. In AIAA Scitech 2020 Forum, page 1046, 2020.

[9] Weicheng Xue, Charles W Jackson, and Christopher J Roy. An Improved Framework of GPU Computing for CFD Applications on Structured Grids using OpenACC. arXiv:2012.02925