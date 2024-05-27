---
layout: default
title: Lab 2 Solutions
---


## Task 1: Generate your inlist

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

  ! initial age
     set_initial_age = .true.
     initial_age = 0

  ! display on-screen plots
     pgstar_flag = .true.

  ! Optional!
     pause_before_terminate = .true.
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

  ! mdot
     mass_change = 3d-8 ! replace with your accretion rate

```

### bonus task: extra output
There are many ways you could calculate this, but the following is the simplest:

in your <code>src/run_star_extras.f90</code>:

```fortran
      integer function how_many_extra_history_columns(id)
        integer, intent(in) :: id
        integer :: ierr
        type (star_info), pointer :: s
        ierr = 0
        call star_ptr(id, s, ierr)
        if (ierr /= 0) return
        how_many_extra_history_columns = 1
      end function how_many_extra_history_columns


      subroutine data_for_extra_history_columns(id, n, names, vals, ierr)
        integer, intent(in) :: id, n
        character (len=maxlen_history_column_name) :: names(n)
        real(dp) :: vals(n)
        integer, intent(out) :: ierr
        type (star_info), pointer :: s
        ierr = 0
        call star_ptr(id, s, ierr)
        if (ierr /= 0) return

        ! note: do NOT add the extras names to history_columns.list                            
        ! the history_columns.list is only for the built-in history column options.            
        ! it must not include the new column names you are adding here.                        

        names(1) = 'he_shell_mass'
        vals(1) = s% star_mass - s% co_core_mass

      end subroutine data_for_extra_history_columns
```



## Task 4: Create a new reaction network

<code>nco.net</code>:

```
         include 'basic.net'
         include 'add_co_burn'

      add_isos_and_reactions(
        c14
        o18
        )
```



