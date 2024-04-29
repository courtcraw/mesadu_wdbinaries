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
In <code>inlist1</code>
<summary> Solution </summary><p>
<code>


</code>
</p></details></hint>


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


