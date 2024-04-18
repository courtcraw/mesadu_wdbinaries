---
layout: default
title: Lab 2 Solutions
---


### Task 1: Generate your inlist

in <code>star_job</code>:

```
 ! load 
     create_pre_main_sequence_model = .false.
     load_saved_model = .true.
     load_model_filename = 'cowd_1.000M_Tc2e7.mod' ! edit with your filename

 ! new net
     change_initial_net = .true.
     new_net_name = 'co_burn.net'

  ! initial time step
     set_initial_dt = .true.
     years_for_initial_dt = 1d-1

  ! display on-screen plots
     pgstar_flag = .true.
```

in <code>controls</code>:

```
  ! when to stop
     power_he_burn_upper_limit = 1d4

  ! mixing
     use_ledoux_criterion = .true.

  ! timesteps
     delta_lgL_He_limit = 0.02d0 !0.025d0

  ! mesh
     mesh_delta_coeff = 1.2d0

  ! accretion
     xa_function_species(1) = 'he4'
     xa_function_weight(1) = 0 !30
     xa_function_param(1) = 1d-2

  ! solver
     energy_eqn_option = 'eps_grav'
     max_resid_jump_limit = 1d20 !1d6
     make_gradr_sticky_in_solver_iters = .true.
     report_solver_progress = .true.

  ! mdot
     mass_change = 3d-8 ! replace with your accretion rate

```



### Task 3: Create a new reaction network



