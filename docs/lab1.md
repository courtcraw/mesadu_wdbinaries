---
layout: default
title: Lab 1
description: Using the Binary Module - Evolving a donor star
---
<a href="[link_url]" target="_blank">[Link_text]</a>
<a href="https://ui.adsabs.harvard.edu/abs/2023MNRAS.519.2567S/abstract" target="_blank">Sarkar+2023</a>
# Solving for Mass Transfer in a DWD binary system

Double white dwarf binaries can display a rich variety of outcomes, depending on their mass ratio (e.g.,<a href="https://ui.adsabs.harvard.edu/abs/2004MNRAS.350..113M/abstract" target="_blank">Marsh+2004</a>). Some will undergo unstable mass transfer and merge/explode, forming either a type Ia supernova or a merger product like an R Coronae Borealis star. 
However, for this lab we will look into the case of stable mass transfer. 

AM CVn binaries are ultracompact binaries with orbital periods between 5 and 69 minutes, where a white dwarf stably accretes from a helium-rich companion (e.g., <a href="https://ui.adsabs.harvard.edu/abs/2018A%26A...620A.141R/abstract" target="_blank">Ramsay+2018</a>). 
Their orbital evolution is driven by angular momentum loss due to the emission of gravitational waves, detectable by future space-based gravitational wave detectors like LISA (e.g.,<a href="https://ui.adsabs.harvard.edu/abs/2024ApJ...963..100K/abstract" target="_blank">Kupfer+2024</a>). 
There are three formation channels for AM CVn binaries. The donor can either be a helium white dwarf formed from a common envelope event, a non-degenerate helium-burning star, or an evolved Cataclysmic Variable formed from stable mass transfer. Common to all these scenarios is that the donor eventually becomes a fully to semi-degenerate object. The donor responds to mass loss by expanding, and since it is filling its Roche lobe, the orbit also expands. 
The component masses and the donor's mass-radius relation set the rate of orbital angular momentum loss, and in turn the mass transfer rate. 
In this lab, we will look into the first two scenarios involving a helium white dwarf or a helium star, and in particular the mass transfer rate for different types of donors. 


In the helium star channel (e.g.,<a href="https://ui.adsabs.harvard.edu/abs/2008AstL...34..620Y/abstract" target="_blank">Yungelson2008</a>,<a href="https://ui.adsabs.harvard.edu/abs/2023MNRAS.519.2567S/abstract" target="_blank">Sarkar+2023</a>), the donor undergoes core helium burning at first. Gravitational waves bring the donor into contact, and the nuclear burning in the donor eventually quenches due to mass loss. In this lab, we will use a few different masses and initial orbital periods at zero-age core helium burning. 

A donor in the helium white dwarf channel (e.g.,[Deloye+2007](https://ui.adsabs.harvard.edu/abs/2007MNRAS.381..525D/abstract),[Wong&Bildsten2021](https://ui.adsabs.harvard.edu/abs/2021ApJ...923..125W/abstract)) is not necessarilly fully degenerate (zero-temperature). Depending on its post-common envelope orbital period, it could remain semi-degenerate due to a short cooling time. We will explore a range of central specific entropies at contact. A higher entropy value (less degenerate) yields a larger radius. 


* * * 

# Lab Instructions

For this lab, we will be running a generic binary system with the accretor as a point mass, assuming fully conservative mass transfer. Each task will have a solution given [here](./lab1_solns.md). Feel free to visit the solutions as needed, but we encourage you to work through with the hints first. 

### Some helpful links

[link to the Google spreadsheet of options](https://docs.google.com/spreadsheets/d/1__UPg_5JfiBkJpZTleyaSwW_faxHzmo_X7Us2RTfLOM/edit#gid=1356579440)

[link to the GitHub repo (general link)](https://github.com/courtcraw/mesadu_wdbinaries)

[link to the MESA documentation](https://docs.mesastar.org/en/release-r24.03.1/)

[Lab 1 solutions if needed](./lab1_solns.md)

## Task 0. Download Files

* Download the Lab 1 working directory from the [GitHub repository (direct link)](https://github.com/courtcraw/mesadu_wdbinaries/blob/main/Lab1_StartPoint/Lab1_StartPoint.zip) and claim a binary in the [MESA Down Under Google Spreadsheet](https://docs.google.com/spreadsheets/d/1__UPg_5JfiBkJpZTleyaSwW_faxHzmo_X7Us2RTfLOM/edit#gid=1356579440). Note: Click "Download Raw File" in the top right corner of the github link here. The download is NOT automatic. 

* Then, download the relevant donor model (HeStar or HeWD) for your binary from the <code>initial_donor_models</code> folder in the [GitHub repository (direct link)](https://github.com/courtcraw/mesadu_wdbinaries/tree/main/initial_donor_models) and save it in your Lab 1 working directory. Note, the donor model files are formatted as `< type >_< mass >M[_Sc< entropy >].mod` and accretor models are formatted as `cowd_< mass >M_Tc2e7.mod`. As we are modelling the accretor as a point mass, we do not actually need to download an accretor model yet!

* At this point, you should be roughly aware of the MESA run speed for your computer. If your computer tends to run slow, we recommend choosing a HeWD as your donor as those run faster than HeStars.

<div class="filetext-title"> The Lab 1 starting directory should contain these files </div> 
<div class="filetext"><p>
inlist <br />
inlist1 <br />
inlist2 <br />
inlist_project <br />
binary_history_columns.list <br />
< type >_< mass >M[_Sc< entropy >].mod <br />
mesa_49_and_fe56.net <br />
src/ <br />
make/ <br />
mk <br />
clean <br />
re <br />
rn <br />
</p></div>

<br>

## Task 1. Editing inlist_project
The inlists in this directory are organized by number, 1 being the donor and 2 being the accretor, along with an <code>inlist_project</code> that contains parameters for both stars. 

Begin by editing <code>inlist_project</code> in the <code>binary_controls</code> section (all relevant areas are notated by a !!!!! in the inlist):

* Set the binary masses and period to the values chosen in Task 0 using <code>m1</code>, <code>m2</code>, and <code>initial_period_in_days</code>. 

* Next, let's set some orbital angular momentum controls (search "orbital jdot controls" under <code>binary_controls</code> in the <a href="[https://docs.mesastar.org/en/release-r24.03.1/]" target="_blank">[MESA Documentation]</a>). In our case, we want to include gravitational wave radiation only, while ignoring the effects of magnetic braking and mass loss (we assume fully conservative mass transfer). Take a look at the [MESA documentation](https://docs.mesastar.org/en/release-r24.03.1/modules.html) to find the corresponding orbital jdot flags and set them accordingly.

<hint><details>
<summary> Hint (click here) </summary><p>
The common format for the three flags is <code>'do_jdot_X'</code>.
</p></details></hint>
<br>

* Finally, we will need to modify the timestep controls for the run. These `f_` parameters provide the ability to set an upper limit on each timestep based on a particular quantity (ie. envelope mass, binary separation, orbital angular momentum, etc). Let's set an upper limit based on the orbital angular momentum and your number of threads. Look at the <a href="[https://docs.mesastar.org/en/release-r24.03.1/]" target="_blank">[MESA Documentation]</a> and set the the timestep control for change in orbital angular momentum. If you are using two (2) threads OR if you are evolving a Helium Star donor, then set this value to <code>2d-3</code>. Otherwise, if using more than two (2) threads, then set this value to <code>7d-4</code>. Note that each option will have two parameters, e.g. <code>fj</code> and <code>fj_hard</code>. We will only alter the first ones, not <code>fj_hard</code>, etc.

<hint><details>
<summary> Hint (click here) </summary><p>
The section on timestep controls based on relative changes is <a href="https://docs.mesastar.org/en/latest/reference/binary_controls.html#fj" target="_blank">here</a>.
</p></details></hint>
<br>

<hint><details>
<summary> Hint (click here) </summary><p>
You are looking to set the parameter <code>fj</code>. If you aren't sure how many threads you are using, run <code>echo $OMP_NUM_THREADS</code> in the terminal. 
</p></details></hint>
<br>

Don't forget to save the inlist! [Solutions](./lab1_solns.md)

<br>

## Task 2. Setting up the donor
We now need to set up our donor star. This work will all be done in the donor inlist, <code>inlist1</code>. 

* Start by editing <code>&star_job</code> to load in the saved donor model file from earlier, change the initial reaction network to `co_burn.net`, and turn on pgstar. Visit the [MESA documentation](https://docs.mesastar.org/en/release-r24.03.1/reference.html) for these variables. 

<hint><details>
<summary> Hint (click here) </summary><p>
Search for <code>load_saved_model</code>, <code>load_model_filename</code>, <code>change_initial_net</code>, <code>new_net_name</code> and <code>pgstar_flag</code>
</p></details></hint>
<br>

<hint><details>
<summary> Hint (click here) </summary><p>
We only need to set <code>change_initial_net</code> here, not <code>change_net</code>. 
</p></details></hint>
<br>


* Now, we can set the initial model number, age, and dt for the run by adding:
```
set_initial_model_number = .true.
initial_model_number = 0
set_initial_age = .true.
initial_age = 0
set_initial_dt = .true.
years_for_initial_dt = 1d3
```

* Now, we can move to <code>&controls</code>. We want to stop the model once the donor loses a given mass. Using the information in the [Google sheet](https://docs.google.com/spreadsheets/d/1__UPg_5JfiBkJpZTleyaSwW_faxHzmo_X7Us2RTfLOM/edit#gid=1356579440), set the <code>star_mass_min_limit</code>.

* Now let's add some solver settings to speed things up. <code>eps_mdot</code> variables modify how MESA treats the energetics of material lost or gained, while relaxing <code>max_resid_jump_limit</code> allows for greater residuals before solver chooses to quit. 
```
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
```

* If you are running a HeStar donor (rather than a HeWD donor), then you will need to adjust 2 additional parameters in your <code>&controls</code> inlist. First, you will comment out the line <code>max_abar_for_burning = -1</code>. This will turn nuclear reactions back on for this model. Second, add the following lines underneath <code>! mlt </code> to turn on mlt++, which will provide numerical speedup for your models.

```
     okay_to_reduce_gradT_excess = .true.
     gradT_excess_lambda1 = -1
```

* Now lets move to <code>&pgstar</code>. We need something interesting to look at during our runs (we turned on that pgstar flag for a reason!). Our first plot is a temperature/density profile. Turn the TRho profile on and set the min/max values of the window. We recommend x = [-8.1, 7.2] and y = [2.6, 8.5], but feel free to experiment. Now, make this plot a little more informative by showing the Equation of State regions.

```
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
```

* Our second plot will show us the period of the first star (our lovely donor) against mass loss. We will add another panel to the pgstar output using `History_Panels1`. Look through <code>binary_history_columns.list</code> to find the axis names to plot the period in minutes and the mdot of the first star (as a log). Include these variables as <code>History_Panels1_xaxis_name</code> and <code>History_Panels1_yaxis_name(1)</code>, respectively, in the code below. 

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
Given that we are evolving the accetor as a point mass, is there any remaining setup we need to do for that star?

<hint><details>
<summary> Hint (click here) </summary><p>
We don't need to set up the accretor, because <code>evolve_both_stars</code> is set to False! This means, the binary module only cares about the primary star, the donor, as described in <code>inlist1</code>. As a matter of fact, MESA won't even read <code>inlist2</code>. 
</p></details></hint>
<br>


## Task 4. Adding history columns

In order for this exercise to be useful for lab 3, we need to save out additional data in our history columns for later use. To do this, uncomment the following values in your <code>binary_history_columns.list</code>:

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
It is finally time! Clean, Make, and Run the model and watch the magic of computers! The runs should take approximately 8 minutes. If the run appears desparately stuck, let us know. Keep in mind that run time will be dependent on which donor model is being used and how many threads are available. If the run is taking too long, you can stop it and use the solution file for Lab 3.

* Once the model starts, you'll notice some colors on our Temperature-Density plot. Let's add a little bit more information on the fly! Keep the run going and open <code>inlist1</code> in a new terminal tab. Add the following under <code>pgstar</code> and save the file:
```
! add legend explaining colors
show_TRho_Profile_legend = .true.
```
The plot should update automatically to describe the colors we are seeing without having to restart the run! Thanks pgstar!

* You can also add some text information on the fly to view the donor's current mass with the following lines as well:
```
show_TRho_Profile_text_info = .true.
```

* Now lets focus on the history panels. The upper panel, which shows the mass transfer rate versus the period in minutes, may appear blank for awhile as the binary inspirals. In general, your model will evolve leftwards in both of these panels, as the stars inspiral and decrease their orbital period. You can watch the luminosity and temperature of your donor star change during the inspiral.

* Once mass transfer begins, you should see the mass transfer rate change in the upper panel. What happens to the luminosity and temperature of the donor as the mass transfer begins? How does the mass transfer rate change over time/orbital period?

The run should terminate with the code <code>star_mass_min_limit</code>, indicating our stopping condition was successful. 

Now compare with your neighbor's model. Are there any behavior differences?

### BONUS. Calculating Timescales

If you choose to do this bonus task, please begin by making a backup of your completed <code>LOGS1</code> directory, just in case you run out of time and don't get to complete this second run!

The general behavior of timescales can provide us with insights into the behavior of a system. As a bonus exercise, we will add the global thermal timescale and the mass transfer timescale as history columns. Both of these timescales can be derived from the variables floating around in MESA, generally following:

```
Global thermal timescale = integral ( cp * T * dm ) / L
Mass transfer timescale ~ M/Mdot
```

In order to add these values to history columns, we cannot simply add the values to <code>history_columns.list</code>, instead we need to modify <code>run_star_extras</code>. 

Open <code>run_star_extras</code> and replace the include statement with the contents of <code>$MESA_DIR/star/job/standard_run_star_extras.inc</code>. Then, find the subroutine and function that would allow us to add data to additional history columns. Take a look through the [MESA Documentation](https://docs.mesastar.org/en/release-r24.03.1/using_mesa/extending_mesa.html) if you are not sure.

<hint><details>
<summary> Hint (click here) </summary><p>
We will be making our changes in the subroutine <code>data_for_extra_history_columns</code> 
</p></details></hint>
<br>

In this subroutine, declare an additional integer, k. Remember the format for variable initializations is <code>type :: name</code>. 

Now, let's add in the thermal timescale in years. Establish a name and initial value for this column, then write a Do loop for the integral.  
```fortran
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

Next, we can add the timescale for mass change in years. As with the thermal timescale, we will need to establish a name for this column, <code>t_Mdot</code> and an initial value for this column, <code>1d99</code>. Add a write step that prints the result of the thermal timescale calculation to the terminal. Only set the column to the absolute value of this calculation, if |Mdot| is over 1d-20. Otherwise, leave the column at <code>1d99</code>. Remember to watch the units!
```fortran
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

# Solutions

If you need any solutions for Lab 1: you can find them [here](./lab1_solns.md)


[Back to main page](./)