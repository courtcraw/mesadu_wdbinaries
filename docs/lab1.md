---
layout: default
title: Lab 1
description: Using the Binary Module - Evolving a donor star
---

# Solving for Mass Transfer in a DWD binary system

Double white dwarf binaries can display a rich variety of outcomes, depending on their mass ratio (e.g.,[Marsh+2004](https://ui.adsabs.harvard.edu/abs/2004MNRAS.350..113M/abstract)). Some will undergo unstable mass transfer and merge/explode, forming either a type Ia supernova or a merger product like an R Coronae Borealis star. 
However, for this lab we will look into the case of stable mass transfer. 

AM CVn binaries are ultracompact binaries with orbital periods between 5 and 69 minutes, where a white dwarf stably accretes from a helium-rich companion (e.g., [Ramsay+2018](https://ui.adsabs.harvard.edu/abs/2018A%26A...620A.141R/abstract)). 
Their orbital evolution is driven by angular momentum loss due to the emission of gravitational waves, detectable by future space-based gravitational wave detectors like LISA (e.g.,[Kupfer+2024](https://ui.adsabs.harvard.edu/abs/2024ApJ...963..100K/abstract)). 
There are three formation channels for AM CVn binaries. The donor can either be a helium white dwarf formed from a common envelope event, a non-degenerate helium-burning star, or an evolved Cataclysmic Variable formed from stable mass transfer. Common to all these scenarios is that the donor eventually becomes a fully to semi-degenerate object. The donor responds to mass loss by expanding, and since it is filling its Roche lobe, the orbit also expands. 
The component masses and the donor's mass-radius relation set the rate of orbital angular momentum loss, and in turn the mass transfer rate. 
In this lab, we will look into the first two scenarios involving a helium white dwarf or a helium star, and in particular the mass transfer rate for different types of donors. 


In the helium star channel (e.g.,[Yungelson2008](https://ui.adsabs.harvard.edu/abs/2008AstL...34..620Y/abstract),[Sarkar+2023](https://ui.adsabs.harvard.edu/abs/2023MNRAS.519.2567S/abstract)), the donor undergoes core helium burning at first. Gravitational waves bring the donor into contact, and the nuclear burning in the donor eventually quenches due to mass loss. In this lab, we will use a few different masses and initial orbital periods at zero-age core helium burning. 

A donor in the helium white dwarf channel (e.g.,[Deloye+2007](https://ui.adsabs.harvard.edu/abs/2007MNRAS.381..525D/abstract),[Wong&Bildsten2021](https://ui.adsabs.harvard.edu/abs/2021ApJ...923..125W/abstract)) is not necessarilly fully degenerate (zero-temperature). Depending on its post-common envelope orbital period, it could remain semi-degenerate due to a short cooling time. We will explore a range of central specific entropies at contact. A higher entropy value (less degenerate) yields a larger radius. 


* * * 

# Lab Instructions

For this lab, we will be running a generic binary system with the accretor as a point mass, assuming fully conservative mass transfer. Each task will have a solution given [here](./lab1_solns.md). Feel free to visit the solutions as needed, but we encourage you to work through with the hints first. 


## Task 0. Download Files
Download the Lab 1 working directory from the [github repo](https://github.com/courtcraw/mesadu_wdbinaries) and claim a binary in the [MESA Down Under Google Spreadsheet](https://docs.google.com/spreadsheets/d/1__UPg_5JfiBkJpZTleyaSwW_faxHzmo_X7Us2RTfLOM/edit#gid=1356579440). Then, download the relevant donor model (HeStar or HeWD) and accretor model (cowd) for your binary from the <code>initial_donor_models</code> folder in the [github repo](https://github.com/courtcraw/mesadu_wdbinaries) and save it in your Lab 1 working directory. Note, the donor model files are formatted as '< type >_< mass >M[_Sc< entropy >].mod' and accretor models are formatted as 'cowd_< mass >M_Tc2e7.mod'. 

<div class="filetext-title"> The Lab 1 starting directory should contain these files </div> 
<div class="filetext"><p>
inlist <br />
inlist1 <br />
inlist2 <br />
inlist_project <br />
binary_history_columns.list <br />
< type >_< mass >M[_Sc< entropy >].mod <br />
cowd_< >M_Tc2e7.mod <br />
mesa_49_and_fe56.net <br />
src/ <br />
make/ <br />
mk <br />
clean <br />
re <br />
rn <br />
</p></div>

<br>

## Task 1. Project Setup
The inlists (and some variables) in a binary directory are organized by number, 1 and 2. For the purposes of this lab, 1 will refer to the donor star, while 2 will refer to the accretor. 

To begin, open <code>inlist_project</code>. Set the binary masses and period to the values chosen in Task 0 using <code>m1</code>, <code>m2</code>, and <code>initial_period_in_days</code>. 

Next, let's set some orbital angular momentum controls. In our case, we want to include gravitational wave radiation only, while ignoring the effects of magnetic braking and mass loss (we assume fully conservative mass transfer). Take a look at the MESA documentation to find the corresponding orbital jdot flags and set them accordingly.

<hint><details>
<summary> Hint (click here) </summary><p>
Search "orbital jdot controls" in the MESA Documentation, specifically under binary_controls. The common format for the three flags is <code>'do_jdot_X'</code>.
</p></details></hint>
<br>

Finally, we will need to modify the timestep controls for the run. These 'f_' parameters provide the ability to set an upper limit on each timestep based on a particular quantity (ie. envelope mass, binary separation, orbital angular momentum, etc). Let's set an upper limit to based on the orbital angular momentum and your number of threads. Look at the MESA documentation and set the the timestep control for change in orbital angular momentum. If you are using two (2) threads, then set this value to 2d-3. Otherwise, if using more than two (2) threads, then set this value to 5d-4. 

<hint><details>
<summary> Hint (click here) </summary><p>
If you aren't sure how many threads you are using, run <code>echo $OMP_NUM_THREADS</code> in the terminal. 
</p></details></hint>
<br>

Don't forget to save the inlist!

<br>

## Task 2. Setting up the donor
We now need to set up our donor star. This work will all be done in the donor inlist, <code>inlist1</code>. Start by editing <code>&star_job</code> to load in the saved donor model file from earlier, change the initial reaction network to 'co_burn', and turn on pgstar. Visit the MESA documentation for these variables. 

<hint><details>
<summary> Hint (click here) </summary><p>
Search for <code>load_saved_model</code>, <code>load_model_filename</code>, and <code>change_initial_net</code>
</p></details></hint>
<br>

Now, we can set the initial model number, age, and dt for the run by adding:
```
set_initial_model_number = .true.
initial_model_number = 0
set_initial_age = .true.
initial_age = 0
set_initial_dt = .true.
years_for_initial_dt = 1d3
```

We want to stop the model once the donor loses a given mass. Using the information in the [Google sheet](https://docs.google.com/spreadsheets/d/1__UPg_5JfiBkJpZTleyaSwW_faxHzmo_X7Us2RTfLOM/edit#gid=1356579440), find the target final mass of the donor model and set the <code>star_mass_min_limit</code> variable to that value. 
<hint><details>
<summary> Hint (click here) </summary><p>
star_mass_min_limit = (mass of donor - max loss)
</p></details></hint>
<br>

Let's change some solver settings to speed things up. First, Set <code>eps_mdot_leak_frac_factor</code> and <code>eps_mdot_factor</code> to 0d0. Then, set the maximum jump limit, <code>max_resid_jump_limit</code>, to 1d20.

We need something interesting to look at during our runs (we turned on that pgstar flag for a reason!). Our first plot is a temperature/density profile. Turn the TRho profile on and set the min/max values of the window. We recommend x = [-8.1, 7.2] and y = [2.6, 8.5], but feel free to experiment. Now, make this plot a little more informative by showing the Equation of State regions.

Our second plot will show us the period of the first star (our lovely donor) against mass loss. We will add another panel to the pgstar plot using 'History_Panels1'. Look through <code>binary_history_columns.list</code> to find the axis names to plot the period in minutes and the mdot of the first star (as a log). Include these variables as <code>History_Panels1_xaxis_name</code> and <code>History_Panels1_yaxis_name(1)</code>, respectively, in the code below. 
```
History_Panels1_win_flag = .true.
History_Panels1_num_panels = 2

History_Panels1_xaxis_name =  ! Period in minutes
History_Panels1_yaxis_name(1) =  ! log10 of donor star abs(mdot)

History_Panels1_yaxis_reversed(1) = .false.
History_Panels1_ymin(1) = -13d0
History_Panels1_ymax(1) = -6d0
History_Panels1_dymin(1) = -1
History_Panels1_other_yaxis_name(1) = ''
```

Don't forget to save the inlist!

<br>

## Task 3. Setting up the Accretor
Do we need to do anything for the accretor? Look at the variables in <code>inlist_project</code> for an answer.

<hint><details>
<summary> Hint (click here) </summary><p>
We don't need to set up the accretor, because <code>evolve_both_stars</code> is set to False! This means, the binary module only cares about the primary star, the donor, as described in <code>inlist1</code>. As a matter of fact, MESA won't even read <code>inlist2</code>. 
</p></details></hint>
<br>


## Task 4. Adding history columns
In order for this exercise to be a useful shortcut, we need to save out additional data in our history columns for later use. To do this, uncomment the following values in your <code>binary_history_columns.list</code>:

* <code>period_minutes</code>
* <code>binary_separation</code>
* <code>star_1_mass</code>
* <code>star_2_mass</code>
* <code>lg_mstar_dot_1</code>
* <code>lg_mstar_dot_2</code>
* <code>J_orb</code>
* <code>Jdot</code>
* <code>donor_index</code>

<hint><details>
<summary> Hint (click here) </summary><p>
Each of them is marked by a '!!!!!'
</p></details></hint>
<br>

Double check that each of the above values is uncommented! (And don't forget to save)

<br>

## Task 5. Run the model
It is finally time! Run the model and watch the magic of computers! The runs should take approximately 8 minutes. If the run appears desparately stuck, let us know. Keep in mind that run time will be dependent on which donor model is being used and how many threads are available.

<hint><details>
<summary> Hint (click here) </summary><p>
Don't forget to ./clean then ./mk
</p></details></hint>
<br>

Once the model starts, you'll notice some colors on our Temperature-Density plot. Let's add a little bit more information on the fly! Keep the run going and open <code>inlist1</code>. Add the following under <code>pgstar</code> and save the file:
```
! add legend explaining colors
show_TRho_Profile_legend = .true.
```

The plot should update automatically to describe the colors we are seeing without having to restart the run! Thanks pgstar!

Now, watch the behavior of the Mdot vs period plot. Are there any features you notice? How do they relate to changes in the luminosity?

The run should terminate with the code <code>star_mass_min_limit</code>, indicating our stopping condition was successful. 

Feel free to repeat with another donor. Are there any behavior differences?


* * *



[Back to main page](./)