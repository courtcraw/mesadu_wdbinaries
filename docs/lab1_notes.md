* * *
* * *
* * *
* * *

Current timing: 15 minutes to make inlist changes (Tryston)
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