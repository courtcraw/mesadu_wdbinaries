---
layout: default
title: Lab 1 Solutions
---


## Task 0. Download Files
<hint><details>
<summary> Links </summary><p>
Github Repo -> https://github.com/courtcraw/mesadu_wdbinaries

MESA Down Under Sheet -> https://docs.google.com/spreadsheets/d/1__UPg_5JfiBkJpZTleyaSwW_faxHzmo_X7Us2RTfLOM/edit#gid=1356579440
</p></details></hint>
<br>


## Task 1: Project Setup
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

## Task 2. Setting up the donor
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

<hint><details>
<summary> Full Solution </summary><p>
In <code>Inlist1</code>
<code>
&star_job

  ! load
    !!!!!
    load_saved_model = .true.
    load_model_filename = 'HeWD_0.150M_Sc2.0.mod'
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

&eos
  ! eos options
  ! see eos/defaults/eos.defaults

  logRho_min_for_any_Skye = 1.9 !2.3
  logRho_min_for_all_Skye = 2.0 !2.4
  logT_min_for_any_Skye = 6.5 !7.2
  logT_min_for_all_Skye = 6.6 !7.4

/ ! end of eos namelist


&kap
  ! kap options
  ! see kap/defaults/kap.defaults
  use_Type2_opacities = .true.
  Zbase = 0.02

/ ! end of kap namelist


&controls
  ! starting specifications

  ! when to stop
    !!!!!
    star_mass_min_limit = 0.10d0
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

## Task 3 - Setting up the Accretor
<hint><details>
<summary> &star_job </summary><p>
<code>
&star_job
  ! see star/defaults/star_job.defaults

  ! load 
    create_pre_main_sequence_model = .false.
    !!!!!
    load_saved_model = .true.
    load_model_filename = 'cowd_1.000M_Tc2e7.mod'
    !!!!!    

  ! DO NOT save a model at the end of the run
    !!!!!
    save_model_when_terminate = .false.
    !!!!!

  ! new net
    !!!!!
    change_initial_net = .true.
    new_net_name = 'co_burn.net'
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
<summary> &pgstar </summary><p>
<code>
&pgstar
  ! see star/defaults/pgstar.defaults

  ! MESA uses PGPLOT for live plotting and gives the user a tremendous
  ! amount of control of the presentation of the information.

  ! show temperature/density profile - this plots the internal structure at single timestep
    !!!!!
    TRho_profile_win_flag = .true.
    !!!!!

  ! add legend explaining colors
    !!!!!
    show_TRho_Profile_legend = .true.
    !!!!!

  ! display numerical info about the star
    !!!!!
    show_TRho_Profile_text_info = .true.
    !!!!!

  ! set window size (aspect_ratio = height/width)
    !!!!!
    TRho_Profile_win_width = 8
    TRho_Profile_win_aspect_ratio = 0.75
    !!!!!

  ! show Abundances
    !!!!!
    Abundance_win_flag = .true.
    Abundance_xmin = 0.99d0
    !!!!!

/ ! end of pgstar namelist
</code>
</p></details></hint>
<br>

<hint><details>
<summary> Full solution </summary><p>
In <code>inlist2</code>:
<code>
&star_job
  ! see star/defaults/star_job.defaults

  ! load 
    create_pre_main_sequence_model = .false.
    !!!!!
    load_saved_model = .true.
    load_model_filename = 'cowd_1.000M_Tc2e7.mod'
    !!!!!    

  ! DO NOT save a model at the end of the run
    !!!!!
    save_model_when_terminate = .false.
    !!!!!

  ! new net
    !!!!!
    change_initial_net = .true.
    new_net_name = 'co_burn.net'
    !!!!!

  ! display on-screen plots
    !!!!!
    pgstar_flag = .true.
    !!!!!

/ ! end of star_job namelist


&eos
  ! eos options
  ! see eos/defaults/eos.defaults

/ ! end of eos namelist


&kap
  ! kap options
  ! see kap/defaults/kap.defaults
  use_Type2_opacities = .true.
  Zbase = 0.02

/ ! end of kap namelist


&controls
  ! see star/defaults/controls.defaults

  ! starting specifications

  ! when to stop

  ! wind

  ! atmosphere

  ! rotation

  ! element diffusion

  ! mlt

  ! mixing

     use_ledoux_criterion = .true.

  ! timesteps

     delta_lgL_He_limit = 0.02d0 !0.025d0

  ! mesh

     mesh_delta_coeff = 1.5d0

     xa_function_species(1) = 'he4'
     xa_function_weight(1) = 0 !30
     xa_function_param(1) = 1d-2

     mesh_dlog_cno_dlogP_extra = 1d0 !0.25d0

  ! solver
  
     energy_eqn_option = 'eps_grav'

     max_resid_jump_limit = 1d20 !1d6

     make_gradr_sticky_in_solver_iters = .true.

     report_solver_progress = .true.
     use_gold_tolerances = .false.

  ! mdot

    ! mass_change = 3d-8

  ! output

/ ! end of controls namelist

&pgstar
  ! see star/defaults/pgstar.defaults

  ! MESA uses PGPLOT for live plotting and gives the user a tremendous
  ! amount of control of the presentation of the information.

  ! show temperature/density profile - this plots the internal structure at single timestep
    !!!!!
    TRho_profile_win_flag = .true.
    !!!!!

  ! add legend explaining colors
    !!!!!
    show_TRho_Profile_legend = .true.
    !!!!!

  ! display numerical info about the star
    !!!!!
    show_TRho_Profile_text_info = .true.
    !!!!!

  ! set window size (aspect_ratio = height/width)
    !!!!!
    TRho_Profile_win_width = 8
    TRho_Profile_win_aspect_ratio = 0.75
    !!!!!

  ! show Abundances
    !!!!!
    Abundance_win_flag = .true.
    Abundance_xmin = 0.99d0
    !!!!!

/ ! end of pgstar namelist
</code>
</p></details></hint>
<br>

## Task 4 - Adding history columns
In <code>binary_history_columns.list</code>
<hint><details>
<summary> Full Solution </summary><p>
<code>
! the following lines of the log file contain info about 1 model per row
   
      model_number ! model number of donor star
      age ! age of donor star

      ! General binary information

      ! period_days ! orbital period in days
      !period_hr ! orbital period in hours
      period_minutes ! orbital period in minutes !!!!!
      !lg_separation ! log10 of orbital separation in rsun
      binary_separation ! orbital separation in rsun !!!!!
      !eccentricity ! orbital eccentricity
      v_orb_1 ! orbital velocity of first star (in km/s)
      v_orb_2 ! orbital velocity of first star (in km/s)

      ! Information related to radius and overflow
      !star_1_radius ! radius of the first star in rsun
      !star_2_radius ! radius of the second star in rsun
      rl_1 ! roche lobe radius of first star in rsun
      rl_2 ! roche lobe radius of second star in rsun
      !rl_overflow_1 ! roche lobe overflow of first star in rsun
      !rl_overflow_2 ! roche lobe overflow of second star in rsun
      rl_relative_overflow_1 ! roche lobe overflow of first star in units of rl_donor
      rl_relative_overflow_2 ! roche lobe overflow of second star in units of rl_donor
   
      ! Information related to eccentricity change
      
      !edot ! total eccentricity change
      !edot_tidal ! eccentricity change due to tidal interactions
      !edot_enhance ! eccentricity change due to eccentricity pumping
      !extra_edot ! user defined extra eccentricity change

      ! Information related to masses and mass transfer

      star_1_mass ! mass of first star in msun !!!!!
      !lg_star_1_mass ! log10 mass of first star in msun
      star_2_mass ! mass of second star in msun !!!!!
      !lg_star_2_mass ! log10 mass of second star in msun
      !sum_of_masses ! star_1_mass + star_2_mass
      lg_mtransfer_rate ! log10 of abs(mass transfer rate) in Msun/yr
                     ! this considers the amount of mass lost from the donor due to RLOF
                     ! not the actual mass that ends up accreted
      lg_mstar_dot_1 ! log10 of first star abs(mdot) in Msun/yr !!!!!
      lg_mstar_dot_2 ! log10 of second star abs(mdot) in Msun/yr !!!!!
      lg_system_mdot_1 ! log10 of abs(mdot) of mass lost from the system from
                        ! around star 1 due to inneficient mass transfer in Msun/yr
      lg_system_mdot_2 ! log10 of abs(mdot) of mass lost from the system from
                        ! around star 2 due to inneficient mass transfer in Msun/yr
      lg_wind_mdot_1 ! log10 of first star abs(mdot) due to winds in Msun/yr
      lg_wind_mdot_2 ! log10 of second star abs(mdot) due to winds in Msun/yr
      !star_1_div_star_2_mass ! star_1_mass/star_2_mass
      !delta_star_1_mass ! star_2_mass/initial_star_2_mass
      !delta_star_2_mass ! star_2_mass/initial_star_2_mass
      fixed_xfer_fraction ! fixed mass transfer fraction 1-alpha-beta-delta
      eff_xfer_fraction ! effective efficiency, -dot_M_a/dot_M_d
      !lg_mdot_edd ! log10 Eddington accretion rate for point mass accretor in units of Msun/secyer
      !mdot_edd_eta ! Efficiency of radiation from accretion to point source
      !lg_accretion_luminosity ! log10 Luminosity from accretion to point source (in units of Lsun)
      !bh_spin ! Spin parameter of BH accretor 
      !lg_mdot_system h1 ! you can use lg_mdot_system <isotope> to get the mass loss
                         ! rate from the system corresponding to a particular isotope.

      ! Information regarding angular momentum

      J_orb ! orbital angular momentum in g cm^2 s^-1 !!!!!
      !J_spin_1 ! spin angular momentum of first star
      !J_spin_2 ! spin angular momentum of second star
      !J_total ! orbital+spin angular momentum
      Jdot ! time derivative of orbital J !!!!!
      jdot_mb ! time derivative of J due to magnetic braking
      jdot_gr ! time derivative of J due to gravitational wave radiation
      jdot_ml ! time derivative of J due to mass loss
      jdot_ls ! time derivative of J due to L-S coupling
      jdot_missing_wind ! time derivative of J due to missing stellar AM
                        ! loss (see binary_controls.defaults)
      !extra_jdot ! time derivative of J due to user defined mechanism
      !accretion_mode ! Specifies whether accretion is ballistic (1) or via a
                      ! Keplerian disc (2). In case there is no angular momentum
                      ! accretion, its equal to zero.
      !acc_am_div_kep_am ! ratio of accreted specific angular momentum to
                         ! that of a Keplerian orbit at R_star. Used only when doing
                         ! rotation and do_j_accretion = .true.
      !lg_t_sync_1 ! log10 synchronization timescale for star 1 in years
      !lg_t_sync_2 ! log10 synchronization timescale for star 2 in years
      !P_rot_div_P_orb_1 ! rotational over orbital period for star 1
      !P_rot_div_P_orb_2 ! rotational over orbital period for star 2

      !Miscellaneous information

      !lg_F_irr ! irradiation flux on donor
      donor_index ! 1 or 2 depending on which star is taken as the donor !!!!!

      point_mass_index ! index of the star taken as point mass, zero if both are modelled

      !ignore_rlof_flag ! flag that indicates whether or not mass transfer from RLOF is ignored
      !model_twins_flag ! flag that indicates whether or not star 2 is modeled as twin of 1

      !CE_flag ! flag that indicates if a CE event is being modeled
      !CE_lambda1 ! lambda value for star 1 after CE ejection. Value is set to zero unless a CE 
      !        ! happens, and is updated when each CE phase finishes
      !CE_lambda2 ! same for star 2
      !CE_Ebind1 ! similar to CE_lambda1, but specifies the binding energy down to the mass
      !          ! coordinate of layers that we're ejected. Includes adjustements to Ebind from
      !          ! alpha_th and other options.
      !CE_Ebind2 ! similar to CE_lambda1, but specifies the binding energy down to the mass
      !          ! coordinate of layers that we're ejected. Includes adjustements to Ebind from
      !          ! alpha_th and other options.
      !CE_num1 ! number of times star 1 has initiated a CE phase
      !CE_num2 ! number of times star 2 has initiated a CE phase
</code>
</p></details></hint>




