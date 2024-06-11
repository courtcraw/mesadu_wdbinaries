---
layout: default
title: Lab 3 Solutions
---

## Task 0. Copy Files
Links:

Github Repo -> https://github.com/courtcraw/mesadu_wdbinaries

MESA Down Under Sheet -> https://docs.google.com/spreadsheets/d/1__UPg_5JfiBkJpZTleyaSwW_faxHzmo_X7Us2RTfLOM/edit#gid=1651867869

<br>

## Task 1. Writing the interpolation Code
In <code>run_star_extras.f90</code>
```fortran
integer function extras_start_step(id)
         integer, intent(in) :: id
         integer :: ierr
         type (star_info), pointer :: s

 
         !!!!!!!!!!!!!!!!!!!!
         !!!!!!!!!!!!!!!!!!!!
         ! Add Variables
         integer :: i
         integer :: placeholder_i
         real(dp) :: placeholder_r
         real(dp), dimension(2) :: star_age, log_dt, log_Mdot
         real(dp) :: log_Mdot_interp        
         !!!!!!!!!!!!!!!!!!!!
         !!!!!!!!!!!!!!!!!!!!


         ierr = 0
         call star_ptr(id, s, ierr)
         if (ierr /= 0) return
         extras_start_step = 0


         !!!!!!!!!!!!!!!!!!!!
         !!!!!!!!!!!!!!!!!!!!
         !! open file
         open(unit=33, file='history.data', action='read')


         !! skip through header (first six rows)
         do i = 1,6
            read(33,*)
         end do


         !! Grab m_dot's
         ! Set Format for file contents
         101 format(2(i40, 1x),5(1pes40.16e3, 1x)) ! i40 = integer with 40 spaces, 1pes40.16e3 = float

         ! Read in history contents
         do 
            read(33,101) placeholder_i, placeholder_i, star_age(2), log_dt(2), placeholder_r, placeholder_r, log_Mdot(2)

            ! Check that current star_age isn't reached
            if (star_age(2) > s% star_age) then
               exit
            else
               star_age(1) = star_age(2)
               log_dt(1) = log_dt(2)
               log_Mdot(1) = log_Mdot(2)
             
            endif

         end do
         close(33)         
         
         !! LIMIT DT
         s% dt_next = min( s% dt_next, exp10(log_dt(1)) * secyer )
         
         !! Check that at least two history rows have been read (the variables will be NAN if not)
         if (log_Mdot(1)==log_Mdot(1)) then
            !! interpolate m_dot (linear)
            log_Mdot_interp = (log_Mdot(1) - log_Mdot(2))/(star_age(1) - star_age(2)) * (s% star_age - star_age(1)) + log_Mdot(1)         

            !! set the m_dot value 
            s% mass_change = exp10(log_Mdot_interp)
         endif
         !!!!!!!!!!!!!!!!!!!!
         !!!!!!!!!!!!!!!!!!!!

      end function extras_start_step
```
<br>

## Task 2. Run the model
In <code>inlist_project</code>
```
&controls
  ! see star/defaults/controls.defaults
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
     !mass_change = 3d-8 ! replace with your accretion rate


/ ! end of controls namelist
```
<br>

## Task 3. Find Helium shell thickness and time to Helium flash
None :)

<br>

## Bonus. Calculating rotation rate of the accretor at Helium flash
None :)

<br>

* Note that the Full Solution in the Github Repo is for the case of a 0.15 M HeWD donor and 1 M Donor. 