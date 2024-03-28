---
layout: default
title: Lab 2
description: This is for Lab 2
---


# This is the markdown file for Lab 2

Here is the text for lab 2 - this lab takes place just before lunch


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




## Mdot over time accretion from part 1 of lab onto donor
* Given a WD model, and then they’ll write (or maybe we’ll give) extras_start_step for mass accretion - probably give interpolation code in fortran to read the history file
* Tryston task: write interpolation code
* Evolve to helium flash - maybe (if possible)
* Goal is to record the total helium mass at helium flash as a function of Mdot (crowdsourced) - see Evan Bauer paper
* More realistic look at what stars actually do before helium flash








[Back to main page](./)