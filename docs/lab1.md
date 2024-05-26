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
Download the Lab 1 working directory from the [GitHub repository](https://github.com/courtcraw/mesadu_wdbinaries) and claim a binary in the [MESA Down Under Google Spreadsheet](https://docs.google.com/spreadsheets/d/1__UPg_5JfiBkJpZTleyaSwW_faxHzmo_X7Us2RTfLOM/edit#gid=1356579440). Then, download the relevant donor model (HeStar or HeWD) and accretor model (cowd) for your binary from the <code>initial_donor_models</code> folder in the [GitHub repo](https://github.com/courtcraw/mesadu_wdbinaries/tree/main/initial_donor_models) and save it in your Lab 1 working directory. Note, the donor model files are formatted as '< type >_< mass >M[_Sc< entropy >].mod' and accretor models are formatted as 'cowd_< mass >M_Tc2e7.mod'. As for the accretor models, contained in the folder <code>initial_accretors_models</code>, you don’t have to do anything for now.

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

Next, let's set some orbital angular momentum controls. In our case, we want to include gravitational wave radiation only, while ignoring the effects of magnetic braking and mass loss (we assume fully conservative mass transfer). Take a look at the [MESA documentation](https://docs.mesastar.org/en/latest/modules.html) to find the corresponding orbital jdot flags and set them accordingly.

<hint><details>
<summary> Hint (click here) </summary><p>
Search "orbital jdot controls" in the [MESA documentation](https://docs.mesastar.org/en/latest/modules.html), specifically under binary_controls. The common format for the three flags is <code>'do_jdot_X'</code>.
</p></details></hint>
<br>

Finally, we will need to modify the timestep controls for the run. These 'f' parameters provide the ability to set an upper limit on each timestep based on a particular quantity (ie. envelope mass, binary separation, orbital angular momentum, etc). Let's set an upper limit on the orbital angular momentum based on your number of threads. Look at the [MESA documentation](https://docs.mesastar.org/en/latest/modules.html) and set the the timestep control for change in orbital angular momentum. If you are using two (2) threads, then set this value to 2d-3. Otherwise, if using more than two (2) threads, then set this value to 5d-4. Note, there are two "kinds" of limits that can be set. We wish to set the soft version here, as opposed to the hard limit (denoted by the suffix "_hard"). 

<hint><details>
<summary> Hint (click here) </summary><p>
The section on timestep controls based on relative changes is [here](https://docs.mesastar.org/en/latest/reference/binary_controls.html#fj).
</p></details></hint>
<br>

<hint><details>
<summary> Hint (click here) </summary><p>
If you aren't sure how many threads you are using, run <code>echo $OMP_NUM_THREADS</code> in the terminal. 
</p></details></hint>
<br>

Don't forget to save the inlist! [Solutions](./lab1_solns.md)

<br>

## Task 2. Setting up the donor
We now need to set up our donor star. This work will all be done in the donor inlist, <code>inlist1</code>. Start by editing <code>&star_job</code> to load in the saved donor model file from earlier, change the initial reaction network to 'co_burn', and turn on pgstar. Visit the [MESA documentation](https://docs.mesastar.org/en/latest/modules.html) for these variables. 

<hint><details>
<summary> Hint (click here) </summary><p>
Search for <code>load_saved_model</code>, <code>load_model_filename</code>, <code>change_initial_net</code>, and <code>pgstar_flag</code>
</p></details></hint>
<br>

<hint><details>
<summary> Hint (click here) </summary><p>
We only need to set <code>change_initial_net</code> here, not <code>change_net</code>. 
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

We want to stop the model once the donor loses a given mass. Using the information in the [Google sheet](https://docs.google.com/spreadsheets/d/1__UPg_5JfiBkJpZTleyaSwW_faxHzmo_X7Us2RTfLOM/edit#gid=1356579440), find the target final mass of the donor model and set the <code>star_mass_min_limit</code> variable in <code>&controls</code> to that value. 
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

Don't forget to save the inlist! [Solutions](./lab1_solns.md)

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

Double check that each of the above values is uncommented! (And don't forget to save) [Solutions](./lab1_solns.md)

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

### BONUS. Calculating Timescales
The general behavior of timescales can provide us with insights into the behavior of a system. As a bonus exercise, we will add the global thermal timescale and the mass transfer timescale as history columns. Both of these timescales can be derived from the variables floating around in MESA, generally following:
```
Global thermal timescale = integral ( cp * T * dm ) / L
Mass transfer timescale ~ M/Mdot
```

In order to add these values to history columns, we cannot simply add the values to <code>history_columns.list</code>, instead we need to modify <code>run_star_extras</code>. 

Open <code>run_star_extras</code> and replace the include statement with the contents of <code>$MESA_DIR/star/job/standard_run_star_extras.inc</code>. Then, find the subroutine and function that would allow us to add data to additional history columns. Take a look through the [MESA Documentation](https://docs.mesastar.org/en/latest/using_mesa/extending_mesa.html) if you are not sure.

<hint><details>
<summary> Hint (click here) </summary><p>
We will be making our changes in the subroutine <code>data_for_extra_history_columns</code> 
</p></details></hint>
<br>

In this subroutine, declare an additional integer, k. Remember the format for variable initializations is <code>type :: name</code>. 

Now, let's add in the thermal timescale in years. Establish a name and initial value for this column, then write a Do loop for the integral.  
```
! thermal timescale (yr)
names(1) = 't_th'
vals(1) = 0d0
do k = 1, () ! Add end condition for Do loop
    vals(1) = () ! Add calculation step in loop
end do 
vals(1) = ! Finish calculation outside of integral
```
<hint><details>
<summary> Hint (click here) </summary><p>
Look through <code>$MESA_DIR/star_data</code> for the available data in the star pointer, <code>s%</code> 
</p></details></hint>

<hint><details>
<summary> Hint (click here) </summary><p>
The variables you'll need to use are: <br />
<code>s% nz</code><br />
<code>s% cp(k)</code><br />
<code>s% T(k)</code><br />
<code>s% dm(k)</code><br />
<code>s% L_surf</code><br />
</p></details></hint>

<hint><details>
<summary> Hint (click here) </summary><p>
The calculation step outside the integral is <code>vals(1) = vals(1) / (s% L_surf * Lsun) / secyer</code>
</p></details></hint>
<br>

Next, we can add the timescale for mass change in years. As with the thermal timescale, we will need to establish a name for this column, <code>t_Mdot</code> and an initial value for this column, <code>1d99</code>. Add a write step that shows prints the result of the thermal timescale calculation to the terminal. Only set the column to the absolute value of this calculation, if |Mdot|is over 1d-20. Otherwise, leave the column at <code>1d99</code>. Remember to watch the units!
```
! timescale for mass change (yr)
names(2) = 't_Mdot'
vals(2) = 1d99
write(*,*) ! Add the calculation here
if () then
   vals(2) = ()
end if
```
<hint><details>
<summary> Hint (click here) </summary><p>
The variables you'll need to use are: <br />
<code>s% mstar</code><br />
<code>s% mstar_dot</code><br />
</p></details></hint>


<hint><details>
<summary> Hint (click here) </summary><p>
<code>write(*,*) s% mstar/s% mstar_dot / secyer</code>
</p></details></hint>


<hint><details>
<summary> Hint (click here) </summary><p>
<code>if (abs(s% mstar_dot/Msun*secyer) > 1d-20) then</code>
</p></details></hint>

<hint><details>
<summary> Hint (click here) </summary><p>
<code>vals(2) = abs( s% mstar/ s% mstar_dot / secyer )</code>
</p></details></hint>
<br>

In order to prep MESA for these extra history columns, update the relevant value in <code>how_many_extra_binary_history_columns</code>. 

Finally, add some useful pgstar plots of these values to the donor inlist:
```
History_Panels1_yaxis_name(2) = 't_th'
History_Panels1_yaxis_reversed(2) = .false.
History_Panels1_ymin(2) = 5d0
History_Panels1_ymax(2) = 12d0
History_Panels1_yaxis_log(2) = .true.
History_Panels1_dymin(2) = -1

History_Panels1_other_yaxis_name(2) = 't_Mdot'
History_Panels1_other_yaxis_reversed(2) = .false.
History_Panels1_other_ymin(2) = 5d0
History_Panels1_other_ymax(2) = 12d0
History_Panels1_other_yaxis_log(2) = .true.
History_Panels1_other_dymin(2) = -1
```

Rerun the model. Do you notice any features in the new timescale plots? What do these values tell us about the behavior of the donor?

* * *



[Back to main page](./)