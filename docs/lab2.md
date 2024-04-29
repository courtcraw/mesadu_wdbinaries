---
layout: default
title: Lab 2
description: Constant Mass Accretion
---


# Lab Instructions

For this lab we will be running constant mass accretion onto the accretor in single star MESA. Our goal is to get to the He flash in these kinds of systems.

[link to the google spreadsheet of options](https://docs.google.com/spreadsheets/d/1__UPg_5JfiBkJpZTleyaSwW_faxHzmo_X7Us2RTfLOM/edit#gid=1651867869)

## Task 0: Download files
Choose your mass and accretion rate from the [google spreadsheet of options](https://docs.google.com/spreadsheets/d/1__UPg_5JfiBkJpZTleyaSwW_faxHzmo_X7Us2RTfLOM/edit#gid=1651867869), then download correct the WD initial model from the [github repo](https://github.com/courtcraw/mesadu_wdbinaries). (test some of the different masses)

## Task 1: Generate your inlist
Start by copying the <code>$MESA_DIR/star/work</code> directory. Make the following edits to your inlist

<code>star_job</code>:

* We will need to set the code to load in the accretor model that you just downloaded. 
* We will set the network to <code>co_burn.net</code> for this first part. 
* Set the initial timestep to zero (or 1d-1). 
* Turn on pgstar output

<hint><details>
<summary> Hint (click here) </summary><p>
Search for the following in the MESA Documentation: <code>load_saved_model</code>, <code>change_initial_net</code>, <code>set_initial_dt</code>, and <code>pgstar_flag</code>
</p></details></hint>
<br>

<code>controls</code>: 

* turn off the initial mass and initial z that come with the default <code>work</code> directory. 
* Change the stopping condition to be when L_He > 10^4 Lsol. 
* Turn on the Ledoux criterion. 
* Add in your constant mass accretion from the spreadsheet.
* Edit the mesh, adjust the timestep limit, tell the code what to accrete, and update the solver:
```
! mesh
     mesh_delta_coeff = 1.2d0
! timesteps
     delta_lgL_He_limit = 0.02d0 !0.025d0
! accretion
     xa_function_species(1) = 'he4'
     xa_function_weight(1) = 0 !30
     xa_function_param(1) = 1d-2
! solver
     energy_eqn_option = 'eps_grav'
     max_resid_jump_limit = 1d20 !1d6
     make_gradr_sticky_in_solver_iters = .true.
```

<hint><details>
<summary> Hint (click here) </summary><p>
Search for the following in the MESA Documentation: <code>power_he_burn_upper_limit</code>, <code>use_ledoux_criterion</code>, and <code>mass_change</code>
</p></details></hint>
<br>

If you need a solution: you can find it [here](./lab2_solns.md)

### Bonus Task: extra output

As a bonus task, you may choose to add a history column to your output that calculates the thickness of the helium shell as it is accreted. 

<hint><details>
<summary> Hint (click here) </summary><p>
The helium shell thickness can be calculated as <code>star_mass - co_core_mass</code> or <code>star_mass - initial_mass</code>
</p></details></hint>
<br>

## Task 2: make/clean/run
Upon completion, record your helium shell thickness and the time of helium ignition to [the google spreadsheet](https://docs.google.com/spreadsheets/d/1__UPg_5JfiBkJpZTleyaSwW_faxHzmo_X7Us2RTfLOM/edit#gid=1651867869).

<hint><details>
<summary> Hint (click here) </summary><p>
The helium shell thickness is <code>star_mass - co_core_mass</code>
</p></details></hint>
<br>

## Task 3: Create a new reaction network
For this portion, we will create our own reaction network that includes the "NCO" reaction chain ($$^{14}N(e^-,\nu)^{14}C(\alpha,\gamma)^{18}O$$) and adjust the reaction rate for $$^{14}C(\alpha,\gamma)^{18}O$$. 

[There is detailed network documentation on the MESA Docs](https://docs.mesastar.org/en/latest/net/nets.html)

Up until now we have been using the reaction network called <code>co_burn.net</code>. Navigate to <code>$MESA_DIR/data/net_data/nets</code> to view the source files for all of the available reaction networks. Start by opening <code>co_burn.net</code> and viewing its contents. You'll see that it contains very little information, and instead points to two other files. Open and examine those other files. You'll see that these other two files have much more information in them. Refer to the docs to see how the functions <code>add_isos()</code> and <code>add_reactions()</code> work.

If you'd like for your MESA run to print out its net information at start, you can add the following to the <code>star_job</code> section of your inlist:
```
show_net_species_info = .true. ! to list all of the isotopes in the net
show_net_reactions_info = .true. ! to output information on all net reactions
list_net_reactions = .true. ! to output a simple list of all the reactions in your net
```

Now lets generate our own reaction network. Begin by copying <code>co_burn.net</code> to your lab 2 working directory and giving it a new name, something like <code>nco.net</code>. Decide which isotopes you'll need to add in for the NCO reaction, either by examining <code>co_burn.net</code> and its constituent files or by outputting the net data to the terminal. Add those isotopes to the reaction network using <code>add_isos_and_reactions()</code>. This will automatically add all reactions associated with those isotopes as well. 

Now we'll update the reaction rate for $$^{14}C(\alpha,\gamma)^{18}O$$. Download the folder called <code>tables_hashimoto</code> from the [github repo](https://github.com/courtcraw/mesadu_wdbinaries). Add the following section to the <code>star_job</code> section of your inlist.

```
  ! adjusting nuclear reaction rates
    rate_tables_dir = 'tables_hashimoto'
    rate_cache_suffix = 'hashimoto'
    ion_coulomb_corrections = 'PCR2009' ! not sure if needed
    electron_coulomb_corrections = 'Itoh2002' ! not sure if needed
    use_3a_fl87 = .true. ! not sure if needed
```

* Court note: I don't know much practical details about this, but I know how to make it work?? Hope someone can expand on exactly how this works??

## Task 4: make/clean/run
Upon completion, record your helium shell thickness and the time of helium ignition to [the google spreadsheet](https://docs.google.com/spreadsheets/d/1__UPg_5JfiBkJpZTleyaSwW_faxHzmo_X7Us2RTfLOM/edit#gid=1651867869).

Interpretation: how did the reaction change those two things?

Bonus task: run through helium flash over lunch time if you wish. - see the heating timescale getting short (some cases where the heating timescale is shorter than the dynamical timescale or the sound speed, there is a time where the heating timescale gets long)

* Could add str_ptr mixing_type (0 to 7, 1=convection), to find the mass of the convective zone rather than just using 10^3 or 4 in our case (this is a different estimate than the pure subtraction)





* * *

# TA notes


* Court note: not done yet but closer than before. running with updated rates and network runs at 3 min runtime on 4 cores

adjust the reaction network to add in the reaction chain $$^{14}N(e^-,\nu)^{14}C(\alpha,\gamma)^{18}O$$ which we will call the NCO reaction.
* [link to the networks docs](https://docs.mesastar.org/en/latest/net/nets.html)

* Courtney note: okay I've been playing with this and I think all we need is to add c14 and o18 to the list of isos, and then I assume the reaction will automatically be added. Evan Bauer's paper has a more complicated patch, but I would assume that got fixed after the version update (they used r8118 lol)
* there is now a test_suite case called custom_rates that implements the updated reaction rate used in the Bauer paper if we wanted to do that?? 
* I need to figure out if or how you would make the reaction chain into its own group for outputting the energy generated by just this reaction chain
<code>add_isos(
c14
o18)</code>

* outputting the energy generated by this reaction should be fairly simple to do using profile_defaults, but then what do we do with that? do we care?
* have them add in the reaction rates
* at 10^-8 rates you should check to see the NCO is actually working by looking for o18 in the star abundance profile

## Potentially, run a constant mass accretion onto the accretor in single star
* Fairly easy task 
* Give them a WD model, and they add in a constant mass accretion which is just an inlist parameter
* Evolve to helium flash
* Goal is to record the total helium mass at helium flash as a function of Mdot (crowdsourced) - see Evan Bauer paper
* Science idea: strength of helium flash as a function of He thickness - whether there is He detonation is a function of the He shell thickness


## notes from newest meeting
* edit reaction network - adding N14 changes some stuff (adding a few reactions can change the thickness of the helium burning shell necessary for the flash) and would be the first one to do reaction networks in the workshop
* recreate fig 4 from Evan Bauer paper with crowd sourcing
* ask them to measure the reaction timescale and compare to the sound crossing time (aka is it super-sonic burning)
* ignition is L_nuc > 10^4-3 Lsol, or when there is a convection zone in the shell (this can be the stopping condition)
* bonus task is to just let it keep running through the whole helium flash
* second potential bonus task is to let it run through with time dependent convection prescriptions - could run this over lunchtime
* rates are between 10^-8 to 10^-7 (tends to decrease in runtime with higher mass transfer rates)









[Back to main page](./)