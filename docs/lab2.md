---
layout: default
title: Lab 2
description: This is for Lab 2
---


# Lab Instructions

For this lab we will be running constant mass accretion onto the accretor in single star MESA. Our goal is to get to the He flash in these kinds of systems.

Task 0: download the WD initial model (what mass?) from the github repo.

Task 1: add in a constant mass accretion to the inlist (chosen from the spreadsheet of options)

Task 2: add stopping condition (ignition is L_nuc > 10^4-3 Lsol or when convection in the he burning shell)

Task 3: Run to the helium flash and record the he thickness and time of detonation

Task 4: adjust the reaction network to add in the NCO reactions

Task 5: Run the same model with the new reaction network and record he thickness and time of detonation

Interpretation: how did the reaction change those two things?

Bonus task: run through helium flash over lunch time if you wish.






* * *

# TA notes

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