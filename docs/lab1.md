---
layout: default
title: Lab 1
description: Using the Binary Module - Evolving a donor star
---

# Solving for Mass Transfer in a DWD binary system

This can serve as a bit of an introduction to what the lab is about

# Lab Instructions

For this lab we will be running a generic binary system with one of the stars as a point mass (which one? elaborate here). Each task will have a solution given [here](./lab1_solns.md). Feel free to visit the solutions as needed, but we encourage you to work through with the hints first. 


### Task 0. Download Files
Download the Lab 1 working directory from the [github repo](https://github.com/courtcraw/mesadu_wdbinaries) and claim a binary in the [MESA Down Under Google Spreadsheet](https://docs.google.com/spreadsheets/d/1__UPg_5JfiBkJpZTleyaSwW_faxHzmo_X7Us2RTfLOM/edit#gid=1356579440). Then, download the relevant donor model (HeStar or HeWD) and accretor model (cowd) for your binary from the [github repo](https://github.com/courtcraw/mesadu_wdbinaries) and save it in your Lab 1 working directory. Note, the donor model files are formatted as '< type >_< mass >M[_Sc< entropy >].mod' and accretor models are formatted as 'cowd_< mass >M_Tc2e7.mod'. 

Here, we specify the entropy of the Helium White Dwarfs because ... ""

<br>

### Task 1. Project Setup
The inlists (and some variables) in a binary directory are organized by number, 1 and 2. For the purposes of this lab, 1 will refer to the donor star, while 2 will refer to the accretor. 

To begin, open <code>inlist_project</code>. Set the binary masses and period to the values chosen in Task 0 using <code>m1</code>, <code>m2</code>, and <code>initial_period_in_days</code>. 

Next, let's set some orbital angular momentum controls. In our case, we want to include gravitational wave radiation and contributions from mass loss, while ignoring the effects of magnetic braking. Take a look at the MESA documentation to find the corresponding orbital jdot flags and set them accordingly.

<hint><details>
<summary> Hint (click here) </summary><p>
Search "orbital jdot controls" in the MESA Documentation, specifically under binary_controls. The common format for the three flags is 'do_jdot_X'.
</p></details></hint>
<br>

Finally, we will need to modify the timestep controls for the run. These 'f_' parameters provide the ability to set an upper limit on each timestep based on a particular quantity (ie. envelope mass, binary separation, orbital angular momentum, etc). Let's set an upper limit to based on the orbital angular momentum and your number of threads. Look at the MESA documentation and set the the timestep control for change in orbital angular momentum. If you are using two (2) threads, then set this value to 2d-3. Otherwise, if using more than two (2) threads, then set this value to 5d-4. 

<hint><details>
<summary> Hint (click here) </summary><p>
If you aren't sure what that value is run <code>echo $OMP_NUM_THREADS</code>. 
</p></details></hint>
<br>

Don't forget to save the inlist!
<br>

### Task 2. Setting up the donor
Now we need to set up our donor star. This work will all be done in the donor inlist (inlist1). 

In 'load', make the following changes to load from the saved model file. Remember to add in the local path to your donor model:
```
load_saved_model = .true.
load_model_filename = ! copy filepath to donor model here
```

We will be using a new reaction network called 'co_burn'. To do this, set <code>change_initial_net</code> to True and new_net_name to 'co_burn.net'

Now, we can set the initial model number, age, and dt for the run by adding:
```
set_initial_model_number = .true.
initial_model_number = 0
set_initial_age = .true.
initial_age = 0
set_initial_dt = .true.
years_for_initial_dt = 1d3
```

Next, turn on the pgstar flag to display the on-screen plots.
<hint><details>
<summary> Hint (click here) </summary><p>
pgstar_flag = .true.
</p></details></hint>
<br>

Now, we want to stop the model once the donor loses a given mass. Using the information in the Google sheet, find the target final mass of the donor model. 
<hint><details>
<summary> Hint (click here) </summary><p>
(mass of donor - max loss)
</p></details></hint>
<br>

Set the stop flag for the model to this target final mass:
```
star_mass_min_limit = ! target minimum mass
```

Next, we need to change some solver settings to speed things up. First, Set eps_mdot_leak_frac_factor and eps_mdot_factor to 0d0. Then, set the maximum jump limit, max_resid_jump_limit, to 1d20.
<hint><details>
<summary> Hint (click here) </summary><p>
<code>
eps_mdot_leak_frac_factor = 0d0
eps_mdot_factor = 0d0
max_resid_jump_limit = 1d20
</code>
</p></details></hint>
<br>

Next, we need something interesting to look at during our runs (we turned on that pgstar flag for a reason!). Our first plot is a temperature/density profile. Turn the TRho profile on and set the min/max values of the window.
<hint><details>
<summary> Hint (click here) </summary><p>
Set TRho_profile_win_flag = .true.
</p></details></hint>
<hint><details>
<summary> Hint (click here) </summary><p>
<code>
TRho_Profile_xmin = -8.1
TRho_Profile_xmax = 7.2
TRho_Profile_ymin = 2.6
TRho_Profile_ymax = 8.5
</code>
You can choose your own values as well. Experiment!
</p></details></hint>
<br>

We can make this first plot a little more informative by showing the Equation of State regions. Set show_TRho_Profile_eos_regions to True. 

The second plot will show us the period of the first star (our lovely donor). Start by adding another panel to the pgstar plot:
```
History_Panels1_win_flag = .true.
History_Panels1_num_panels = 2
```

Plot the binary period on the x-axis and the m_dot on the y-axis by setting 
```
History_Panels1_xaxis_name = 'period_minutes'
History_Panels1_yaxis_name(1) = 'lg_mstar_dot_1'
```

Now, let's add some formatting values to make this useful:
```
History_Panels1_yaxis_reversed(1) = .false.
History_Panels1_ymin(1) = -13d0
History_Panels1_ymax(1) = -6d0
History_Panels1_dymin(1) = -1
History_Panels1_other_yaxis_name(1) = ''
```

Don't forget to save the inlist!


### Task 3 - Setting up the Accretor
Open the accretor inlist. As in Task 2, we want to load in an initial saved model, except the accretors will use the CoWD< >.mod files. In "load", turn on load_saved_model and set load_model_filename to your CoWD model file. 
<hint><details>
<summary> Hint (click here) </summary><p>
The accretor inlist is inlist2
</p></details></hint>
<hint><details>
<summary> Hint (click here) </summary><p>
```
load_saved_model = .true.
load_model_filename = ! copy filepath to accretor model here
```
</p></details></hint>
<br>

Unlike the donor, we unfortunately don't actually care about what this stand-in accretor looks like at the end (we will evolve that later), so set save_model_when_terminate to False. 
<hint><details>
<summary> Hint (click here) </summary><p>
<code>save_model_when_terminate = .false.</code> under "! DO NOT save a model at the end of the run"
</p></details></hint>
<br>

Just as with the donor, we will be using a new reaction network, so repeat the steps from Task 2 here (using co_burn.net).
<hint><details>
<summary> Hint (click here) </summary><p>
<code>
change_initial_net = .true.
new_net_name = 'co_burn.net'
</code>
</p></details></hint>
<br>

Next, Turn on the pgstar flag and plot the temperature/density profile. 
<hint><details>
<summary> Hint (click here) </summary><p>
<code> pgstar_flag = .true. </code> under "! display on-screen plots"
<code> TRho_profile_win_flag = .true. </code>
</p></details></hint>
<br>

Turn on the flags to show the TRho Profile legend and the numerical info about the star. 
<hint><details>
<summary> Hint (click here) </summary><p>
<code> show_TRho_Profile_legend = .true. </code> 
<code> Show_TRho_Profile_text_info = .true. </code>
</p></details></hint>
<br>

Now, let's set the window size using the aspect ratio (height/width):
```
TRho_Profile_win_width = 8
TRho_Profile_win_aspect_ratio = 0.75
```

Finally, turn on the show abundances flag:
```
Abundance_win_flag = .true.
Abundance_xmin = 0.99d0
```

Don't forget to save your inlist!


### Task 4 - Adding history columns
In order for this exercise to be a useful shortcut, we need to save out additional data in our history columns for later use. To do this, add the following values to your history columns:
```
period_minutes
binary_separation
star_1_mass
star_2_mass
lg_mstar_dot_1
lg_mstar_dot_2
J_orb
Jdot
donor_index
```
<hint><details>
<summary> Hint (click here) </summary><p>
Uncomment the variables in binary_history_columns
</p></details></hint>

<hint><details>
<summary> Hint (click here) </summary><p>
Each of them is marked by a '!!!!!'
</p></details></hint>

<hint><details>
<summary> Hint (click here) </summary><p>
Delete the '!' infront of the variables
</p></details></hint>
<br>

Double check that each of the above values is uncommented! (And don't forget to save)


### Task 5 - Run the model
It is finally time! Run the model and watch the magic of computers! The runs should take approximately 8 minutes. If the run appears desparately stuck, let us know. Keep in mind that run time will be dependent on which donor model is being used and how many threads are available.
<hint><details>
<summary> Hint (click here) </summary><p>
./rn
</p></details></hint>

<hint><details>
<summary> Hint (click here) </summary><p>
Don't forget to ./mk
</p></details></hint>
<br>


### Task 5 - Investigate
Once the model has completed look at the plots. INSERT DISCUSSION OF WHAT IS SEEN HERE. 


Feel free to repeat with another donor, are there any differences?

Current timing: 15 minutes to make inlist changes (Tryston)

* * *



* Courtney formatting note: I kind of prefer to only have the hints in the hidden boxes personally, so that students could see their tasks at a glance if they wish. I'll leave this task one here as an example for now.
<task><details>
<summary>Task 1</summary><p>
Choose a separation and set of component masses from the google spreadsheet (place link here). Implement these into your inlist, and make sure that you call the correct donor star from the list of options.
</p></details></task>
* he-wd masses 0.1-0.2 ish solar masses
* co-wd base model is 1 Msol, 0.8 - 1 solar mass generally (for those that are interesting)

Task 2: ensure that Mdot is included in your <code>history_columns.list</code> output (do we include any other things in the output here?)

Task 3: create a pgstar output that will plot the Mdot as a function of time for visualization purposes (any other pgstar output we'd like them to include?)

Task 4: add in the stopping condition (what is our stopping condition?)

Task 5: any other inlist adjustments? (change history file name ?)

Run the model. 

Do they need to put any values into a google spreadsheet for this one or is that only going to come up in lab 3?

Now for some interpretation, what type of donors give different values of Mdot? (are we kind of exploring stability here?)

Bonus tasks?


* being taken over by Tryston

* * *

# TA notes

## Run binary as point mass - goal is to get Mdot as a function of time
* Start from an inlist that we give them for a generic binary, then changing the parameters: component masses and separation
* Run speedup options - could be given or asked to add in
* Ask them to save to history file the Mdot and time (visualize this in pgstar - write their own pgstar with history data)
* Claudia task: run a few and play around
* Main concern is runtime for some parameters, solver may struggle
* Science goal: connect types of donors to Mdot and Mdot to thickness of Helium shell



* claudia timed this on 6 cores with default and it ran in 7 ish minutes
* could lower the time resolution - esp with those for the lower number of threads to do a crowdsourced convergence study - how do we compare these between the different types
* google collab with scripts on it to plot history files on the fly
* vary the separation and mass ratios and the total mass - Sunny will generate donor models for this
* there will be roughly 13 groups, 39 attendees


* he-wd masses 0.1-0.2 ish solar masses
* co-wd base model is 1 Msol, 0.8 - 1 solar mass generally (for those that are interesting)





[Back to main page](./)