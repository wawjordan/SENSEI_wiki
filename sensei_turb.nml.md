This namelist will only be read if the flow equation is set to RANS.

# turbulence_model
* `turb_model`
    ```
    Description: Turbulence model to use.
    Data Type: string
    Default Value: 'none'
    Possible Values: 'none'
                     'sa'
                     'komega'
    ```
* `turb_flux_scheme`
    ```
    Description: Flux scheme to use for turbulence equations.
    Data Type: string
    Default Value: 'roe'
    Possible Values: 'vanleer'
                     'roe'
                     'roe_pike'
    ```
* `turb_dimensional_inputs`
    ```
    Description: Are you using the dimensional(true) inputs or non-dimenional(F) inputs?
    Data Type: boolean
    Default Value: F
    ```
* `first_order_turb`
    ```
    Description: Are you using the first-order (true) or second-order (F) for the turbulence equations?
    Data Type: boolean
    Default Value: F
    ```

# sa (only read if 'sa')
* `nutilde_inflow`
    ```
    Description: Inflow (freestream) value of working variable.
    Data Type: real
    Default Value: 3.0
    ```

# k_omega (only read if 'k-omega')
* `tke_inflow`
    ```
    Description: Inflow (freestream) value of turbulent kinetic energy.
    Data Type: real
    Default Value: 9.0E-9
    ```
* `omega_inflow`
    ```
    Description: Inflow (freestream) value of omega.
    Data Type: real
    Default Value: 1.0E-6
    ```
* `use_vorticity_production`
    ```
    Description: Set it to true, then you are using the SST-V model, otherwise SST
    Data Type: boolean
    Default Value: F
    ```
* `turb_vel_sf`
    ```
    Description: The scaling factor for turbulence reference velocity
    Data Type: double 
    Default Value: 1.00
    ```

# turb_output
* `turb_save_freq`
    ```
    Description: Frequency to output turbulence file.
    Data Type: integer
    Default Value: -1
    Special Values: -1 - Only output at final iteration
    ```
* `print_production`
    ```
    Description: Print the production terms.
    Data Type: boolean
    Default Value: false
    ```
* `print_destruction`
    ```
    Description: Print the destruction terms.
    Data Type: boolean
    Default Value: false
    ```
* `print_diffusion`
    ```
    Description: Print the diffusion terms.
    Data Type: boolean
    Default Value: false
    ```
* `print_misc`
    ```
    Description: Print the miscellaneous terms (depends on the turbulence model).
    Data Type: boolean
    Default Value: false
    ```
* `print_reynolds_stress`
    ```
    Description: Print the Reynolds stress terms.
    Data Type: boolean
    Default Value: false
    ```