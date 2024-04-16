---
layout: default
title: Minilab 3
description: Varying the Accretion Rate
---

# (More) Realistic(ish) Mass Transfer onto a WD accretor

This is the background info about cool stars and why and physics and stuff... 

# Objectives

For this lab, we will use run_star_extras to interpolate the Mdot from Lab 1 and ultimately measure a more-realistic thickness of the Helium shell (as compared to our values from Minilab 2)<br>

* Tryston Note: These tasks can be consolidated/broken up. This is mostly a first pass
# 0: Mission Prep
<task><details><summary> 0.0. Copy over scratch file (baseline for lab)</summary></details></task>
<task><details><summary> 0.1. Copy over history file from Lab 1</summary></details></task>

<br> 

* Tryston Note: Present these as mostly a "if you want to try this you can, here's a hint, otherwise copy from hidden box below"
# 1. Writing the interpolation Code
<task><details><summary> 1.1. Open run_star_extras</summary></details></task>
Insert text here leading them to where the interpolation script go
<task><details><summary> 1.2. Add Variables</summary></details></task>
Remember the format for variables (((is this covered in a prior day??)))
<task><details><summary> 1.3. Open file</summary></details></task>
give the general fortran code for opening file
<task><details><summary> 1.4. Skip through header</summary></details></task>
Ask how many rows they should skip
BONUS: Set format for file contents 
<task><details><summary> 1.5. Fill out "DO" loop</summary></details></task>
Note: "DONT FORGET TO CLOSE THE FILE"
Give them the limit_dt in the file already
<task><details><summary> 1.6. Check history rows have been read (give them the if)</summary></details></task>
Brief talk on why and the NaN comparison in Fortran
<task><details><summary> 1.7. Write interpolation</summary></details></task>
Give them general equation, they find variables
<task><details><summary> 1.8. Set m_dot </summary></details></task>
They find variable name
Note: "exp10 the interpolation result"
<task><details><summary> 1.9. Have them write out m_dot in the terminal? (maybe not, could be a check?)</summary></details></task>
<task><details><summary> 1.10. MAKE/RUN/STOP</summary></details></task>
does stuff look right?

<br>

* Tryston Note: Parts 2 and 3 below can be combined into a bigger section, I wasn't sure if we planned on just running once and manipulating everything after, since the runs are ~7 mins. 
* Tryston Note2: Not sure what units/steps we are asking from them for the rotation rate
# 2. Measuring Rotation of the accretor at --point?-- (not sure if this should also be helium flash, would depend on time)
<task><details><summary> 2.1. Add in stopping condition at --point?-- </summary></details></task>
Insert text here leading them to where the stopping condiiton goes
<task><details><summary> 2.2. Add history column for star mass_change (mass added at any time step)</summary></details></task>
Insert text here leading them to go
Insert hint here for variable name
<task><details><summary> 2.3. MAKE/RUN</summary></details></task>
does stuff look right?
<task><details><summary> 2.4. Get last star mass_change</summary></details></task>
<task><details><summary> 2.5. find Jdot</summary></details></task>
<task><details><summary> 2.6. Assume solid body, find $$\omega$$ in rad/s</summary></details></task>
<task><details><summary> 2.7. Convert to Hz? (maybe not)</summary></details></task>
<task><details><summary> 2.8. Add to Crowdsource doc</summary></details></task>

<br>

# Part 3. Measuring Helium shell thickness at Helium Flash
<task><details><summary> 3.1. Replace stopping condition with Helium flash</summary></details></task>
Insert text here leading them to where the stopping condiiton goes
<task><details><summary> 3.2. Add history column for star_Mdot (total mass added during run)</summary></details></task>
<task><details><summary> 3.3. MAKE/RUN</summary></details></task>
does stuff look right?
<task><details><summary> 3.4. Get last star mass_change</summary></details></task>
<task><details><summary> 3.5. Get last star_Mdot (total mass)</summary></details></task>
<task><details><summary> 3.6. How long did it take to reach Helium flash?</summary></details></task>
Update Crowdsource document

<br>

# BONUS. Include NCO reaction network and repeat
<task><details><summary>Add in NCO and... repeat</summary></details></task>
* Expand out

<br>

# BONUS. Evolutionary Timescale
<task><details><summary>Plot the density vs temperature of the constant case</summary></details></task>
<task><details><summary>Plot the density vs temperature of the varying case</summary></details></task>
<task><details><summary>Compare to Bauer Fig 8</summary></details></task>
* Expand out


* * *

# TA notes

## april 16, 2024
* Jdot = sqrt(G*M*R) * Mdot
* Delta_J = Jdot * s% dt
* rotation rate = delta_J/moment of inertia


## this will actually be the varying Mdot procedure
* measure the rotation rate of the accretor, assuming it rotates as a solid body, angular momentum goes as GMR then measure Jdot (rate of ang mom increase), then assuming a solid body, estimate the rotation rate (it should spin up with mass accretion, this is also true in the constant case)
* give them the interpolation script most likely? - could ask them to put it in - might have them write certain parts and then give them other parts
* runs to helium flash in about 7 minutes it seems - got mad about hydro stuff and adjusted corrections
* use whichever history file from lab 1 that they used - need a second spreadsheet for that
* still want to look at the thickness of the helium burning shell - could use an average Mdot (mass accreted - could store this in the history file columns - might even be a default value vs time taken)
* the default one is called star_Mdot - can simply call that


## Mdot over time accretion from part 1 of lab onto donor
* Given a WD model, and then they’ll write (or maybe we’ll give) extras_start_step for mass accretion - probably give interpolation code in fortran to read the history file
* Tryston task: write interpolation code
* Evolve to helium flash - maybe (if possible)
* Goal is to record the total helium mass at helium flash as a function of Mdot (crowdsourced) - see Evan Bauer paper
* More realistic look at what stars actually do before helium flash


[Back to main page](./)