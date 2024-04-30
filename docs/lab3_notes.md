
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