---
layout: default
title: Lab 1 Solutions
---


### Task 0. Download Files
<hint><details>
<summary> Links </summary><p>
Github Repo -> https://github.com/courtcraw/mesadu_wdbinaries

MESA Down Under Sheet -> https://docs.google.com/spreadsheets/d/1__UPg_5JfiBkJpZTleyaSwW_faxHzmo_X7Us2RTfLOM/edit#gid=1356579440
</p></details></hint>
<br>


### Task 1: Project Setup
<hint><details>
<summary> Binary Mass and Period </summary><p>
In <code> inlist_project</code>:
<code>
   ! Set the binary masses and period
     !!!!!      
     m1 = 0.15d0
     m2 = 1.0d0
     initial_period_in_days = 0.004d0
     !!!!!
</code>
</p></details></hint>
<br>

<hint><details>
<summary> jdot </summary><p>
In <code> inlist_project</code>:
<code>
   ! jdot 
     !!!!!
     do_jdot_gr = .true.
     do_jdot_ml = .true.
     do_jdot_mb = .false.
     !!!!!
</code>
</p></details></hint>
<br>

<hint><details>
<summary> timestep </summary><p>
In <code> inlist_project</code>:
<code>
   ! time step 
     !!!!!
     fm = 0.01d0
     fm_hard = -1d0
     fa = 0.01d0
     fa_hard = 0.02d0
     fr_hard = -1d0
     fj = 5d-4 ! or 2d-3
     fj_hard = 0.01d0
     !!!!!
</code>
</p></details></hint>
<br>

<hint><details>
<summary> Full Solution </summary><p>
In <code> inlist_project</code>:
<code>
&binary_job

   inlist_names(1) = 'inlist1' 
   inlist_names(2) = 'inlist2'

   evolve_both_stars = .false.

/ ! end of binary_job namelist

&binary_controls
   ! Set the binary masses and period
     !!!!!      
     m1 = 0.15d0
     m2 = 1.0d0
     initial_period_in_days = 0.004d0
     !!!!!

   ! transfer efficiency controls
     limit_retention_by_mdot_edd = .false.
     use_radiation_corrected_transfer_rate = .false.

   ! mdot controls
     mdot_scheme = 'Kolb' !'Ritter'
     max_tries_to_achieve = 50
     implicit_scheme_tolerance = 1d-2
     implicit_scheme_tiny_factor = 1d-3

     max_change_factor = 2d0
     min_change_factor = 1.01d0

     report_rlo_solver_progress = .true.

   ! jdot 
     !!!!!
     do_jdot_gr = .true.
     do_jdot_ml = .true.
     do_jdot_mb = .false.
     !!!!!

   ! time step 
     !!!!!
     fm = 0.01d0
     fm_hard = -1d0
     fa = 0.01d0
     fa_hard = 0.02d0
     fr_hard = -1d0
     fj = 5d-4 !2d-3 !5d-4
     fj_hard = 0.01d0
     !!!!!
         
/ ! end of binary_controls namelist
</code>
</p></details></hint>
<br>

### Task 2. Setting up the donor
<hint><details>
<summary> &star_job </summary><p>
<code>
&star_job

  ! load
    !!!!!
    load_saved_model = .true.
    load_model_filename = 'HeWD_0.150M_Sc2.0.mod' ! Replace with filepath
    !!!!!

  ! change net
    !!!!!
    change_initial_net = .true.
    new_net_name = 'co_burn.net'
    !!!!!

  ! set initial model number and age
    !!!!!
    set_initial_model_number = .true.
    initial_model_number = 0

    set_initial_age = .true.
    initial_age = 0
	
    set_initial_dt = .true.
    years_for_initial_dt = 1d3
    !!!!!

  ! display on-screen plots
    !!!!!
    pgstar_flag = .true.
    !!!!!

/ ! end of star_job namelist
</code>
</p></details></hint>
<br>


<hint><details>
<summary> &controls </summary><p>
<code>
&controls
  ! starting specifications

  ! when to stop
    !!!!!
    star_mass_min_limit = 0.10d0 ! Dependent on star
    !!!!!

  ! wind

  ! atmosphere

  ! turn off burning

    max_abar_for_burning = -1

  ! rotation

  ! element diffusion

  ! mlt

  ! mixing

  ! timesteps


  ! mesh

     mesh_delta_coeff = 2.0d0

  ! solver
  
     energy_eqn_option = 'eps_grav'

     ! set these two to zero avoid numerical problems
       !!!!!
       eps_mdot_leak_frac_factor = 0d0
       eps_mdot_factor = 0d0
       !!!!!
     
     ! assist the timesteps
       !!!!!
       max_resid_jump_limit = 1d20
       !!!!!

  ! output

    extra_terminal_output_file = 'log1' 
    log_directory = 'LOGS1'

    profile_interval = 50
    history_interval = 1
    terminal_interval = 1
    write_header_frequency = 10

/ ! end of controls namelist
</code>
</p></details></hint>
<br>

<hint><details>
<summary> &pgstar </summary><p>
<code>
&pgstar
  ! show temperature/density profile
    !!!!!
    TRho_Profile_win_flag = .true.
    TRho_Profile_xmin = -8.1
    TRho_Profile_xmax = 7.2
    TRho_Profile_ymin = 2.6
    TRho_Profile_ymax = 8.5
    !!!!!

  ! add eos regions
    !!!!!
    show_TRho_Profile_eos_regions = .true.
    !!!!!

  ! plot the period of the first star
    !!!!!
    History_Panels1_win_flag = .true.
    History_Panels1_num_panels = 2
    History_Panels1_xaxis_name = 'period_minutes'
    History_Panels1_yaxis_name(1) = 'lg_mstar_dot_1'
    History_Panels1_yaxis_reversed(1) = .false.
    History_Panels1_ymin(1) = -13d0
    History_Panels1_ymax(1) = -6d0
    History_Panels1_dymin(1) = -1
    History_Panels1_other_yaxis_name(1) = ''
    !!!!!
      
      
/ ! end of pgstar namelist
</code>
</p></details></hint>
<br>

### Task 3 - Setting up the Accretor
<hint><details>
In <code>inlist2</code>
<summary> Solution </summary><p>
<code>


</code>
</p></details></hint>


### Task 4 - Adding history columns
In <code>binary_history_columns.list</code>
<hint><details>
<summary> Solution </summary><p>
<code>


</code>
</p></details></hint>


### Task 5 - Run the model
<hint><details>
<summary> Solution </summary><p>
Uncomment the variables in binary_history_columns
</p></details></hint>


