---
layout: default
title: Minilab 3
description: Varying the Accretion Rate
---

[link to the google spreadsheet for lab 3](https://docs.google.com/spreadsheets/d/1__UPg_5JfiBkJpZTleyaSwW_faxHzmo_X7Us2RTfLOM/edit#gid=2060915946)

# (More) Realistic(ish) Mass Transfer onto a WD accretor

This is the background info about cool stars and why and physics and stuff... 

# Lab Instructions
For this lab, we will use run_star_extras to interpolate the Mdot from Lab 1 and ultimately measure a more-realistic thickness of the Helium shell (as compared to our values from Minilab 2)
<br>


### Task 0. Project Setup 
Make a clean copy of your directory from Lab 2 then copy over the binary_history file generated in Lab 1. 


### Task 1. Writing the interpolation Code
In order to meaningfully alter run_star_extras, we need to copy in the available standard subroutines. To begin, open run_star_extras and replace the include statement with contents of <code>$MESA_DIR/star/job/standard_run_star_extras.inc</code>. Check that the code compiles, to ensure that this copy was clean. 

Recall the control flow in MESA, that is, which routines get called at which points during a MESA run. For this lab, which routine should the interpolation script go in? Keep in mind, the goal is to interpolate the varying mdot from Lab 1 and use that as the mdot accreting onto our target star. 

Find that routine in <code>run_star_extras</code>. 

<hint><details>
<summary> Hint (click here) </summary><p>
We need to modify extras_start_step. 
</p></details></hint>
<br>

Now open the donor history file and take a look around. How many columns are before the mdot (log_abs_mdot)? How many rows before our target data begins? 

Our interpolation function will be opening the file to read by row, so it will be able to skip the header. However, in order to save out our Mdot data, we need a place to hold the model_number, num_zones, star_age, log_dt, star_mass, and log_xmstar that come before it. Although we only need the mdot, star_age, and log_dt, take note of the data type(integer or reals) for all seven (7) of these columns.   

Navigate back to run_star_extras and begin adding in some additional variables: integer i, integer placeholder_i, real placeholder_r, real log_Mdot_interp, real 2-dimensional star_age, real 2-dimensional log_dt, and real 2-dimensional log_Mdot. These declarations should be done above the statement <code>ierr=0</code>, but order does not matter. Remember to format the declarations correctly as <code>type :: name</code>. 

<hint><details>
<summary> Hint (click here) </summary><p>
The types used are <code>integer</code>, <code>real(dp)</code>, and <code>real(dp), dimension(2)</code>
</p></details></hint>
<br>

Now, we need to tell Fortran to open the history.data file. Use your profound access to all of human knowledge to find this function (ie. Google it). Set <code>unit=33</code>, <code>file = '< local path to binary_history.data >'</code>, and <code>action='read'</code>. Remember to write this beneath <code>extras_start_step = 0</code>. 

<hint><details>
<summary> Hint (click here) </summary><p>
<code>
!! open file
         open(unit=33, file='history.data', action='read')
</code>
</p></details></hint>

<hint><details>
<summary> Hint (click here) </summary><p>
<code>
The unit number is largely arbitrary, 33 is chosen for consistency, not necessity. 
</code>
</p></details></hint>
<br>

Let's make sure to skip through that header that we saw earlier. Remember how many rows were in the header? Create a do loop that reads the file (henceforth '33'), but does nothing for those first header rows. 
<hint><details>
<summary> Hint (click here) </summary><p>
Our Do loop will have the form:
<code>
do i = 1, < upper bound ><br />
   func()<br />
end do
</code>
</p></details></hint>

<hint><details>
<summary> Hint (click here) </summary><p>
The func() is <code>read(33, *)</code> where * is an automatic format assignment
</p></details></hint>
<br>

Now that we made it through that pesky header, it's time to grab that mdot! Start by setting a format for the file contents. This is also for consistency, not necessity, as format can be automatically assigned with <code>*</code>.
```
! Set Format for file contents
101 format(2(i40, 1x),5(1pes40.16e3, 1x)) ! i40 = integer with 40 spaces, 1pes40.16e3 = float
```
<hint><details>
<summary> Bonus (click here) </summary><p>
What do i40 and 1pes40.16e3 mean? What format are we setting here?
</p></details></hint>
<br>

Once we have an established format we can read in the history contents. Start a do loop and read in the seven values from the history file. Save the star age, log dt, and log mdot from the history file to the second column of our fortran arrays. Remember to use the right placeholder!

<hint><details>
<summary> Hint (click here) </summary><p>
<code>read(file, format) outputs</code>
</p></details></hint>

<hint><details>
<summary> Hint (click here) </summary><p>
To access the second column of the fortran array use <code>< var >(2)</code>
</p></details></hint>

<hint><details>
<summary> Hint (click here) </summary><p>
<code>read(33,101) placeholder_i, placeholder_i, star_age(2), log_dt(2), placeholder_r, placeholder_r, log_Mdot(2)</code>
</p></details></hint>
<br>

Recall there was no mention to add a condition for this do loop as was used earlier to skip the header. Instead, this 'do' will function as a traditional While loop until we give the command to exit. Our goal is, again, to get the Mdot at a particular age of the star. This age will increase each iteration of the loop, as the next line in the history file is read. Write an if to exit the loop when the current star age is reached, else save the star_age(2), log_dt(2), and log_mdot(2) values to the other column of their arrays. 

<hint><details>
<summary> Hint (click here) </summary><p>
The format for if/else in Fortran is:
if (condition) then
else
endif
</p></details></hint>

<hint><details>
<summary> Hint (click here) </summary><p>
Exit the do loop with <code>exit</code>
</p></details></hint>

<hint><details>
<summary> Hint (click here) </summary><p>
The condition is <code>star_age(2) > s% star_age</code>
</p></details></hint>
<br>


End the do loop, close the file, and add the following lines to limit the next timestep. This ensures that the next step taken is the lesser of the current expected timestep and the timestep used in the donor evolution. 
```
!! LIMIT DT
s% dt_next = min( s% dt_next, exp10(log_dt(1)) * secyer )
```
<hint><details>
<summary> Hint (click here) </summary><p>
<code>end do </code>
<code>close(33) </code>
</p></details></hint>
<br>

Before we interpolate, how can we check that at least two history rows have been read? 

Upon variable declaration, their values are considered NaN (Not a Number) until assigned a value. Following the IEEE standard, NaN =/ NaN in Fortran, simplifying this check. If two rows have been read, then <code>log_Mdot(1)==log_Mdot(1)</code> will evaluate True. Otherwise the unset NaN's will not equal each other, evaluating False. Add the following if statement:
```
!! Check that at least two history rows have been read (the variables will be NAN if not)
if (log_Mdot(1)==log_Mdot(1)) then
    !! interpolate m_dot (linear)       

    !! set the m_dot value 

endif
```

Finally, write the linear interpolation for m_dot and set the value for the star. Once complete, clean/mk to check for any errors. 

<hint><details>
<summary> Hint (click here) </summary><p>
The form of the linear interpolation is
Y = Y_0 + ((Y_1 - Y_0)/(X_1 - X_0)) * (X - X_0)
,where Y is the Mdot and X is the star age.
</p></details></hint>

<hint><details>
<summary> Hint (click here) </summary><p>
The variable names to complete the interpolation are:
Y = log_Mdot_interp
X = s% star_age
Y(0) = log_Mdot(1)
Y(1) = log_Mdot(2)
X(0) = star_age(1)
X(1) = star_age(2)
</p></details></hint>

<hint><details>
<summary> Hint (click here) </summary><p>
The variable for m_dot is <code>s% mass_change</code>
</p></details></hint>

<hint><details>
<summary> Hint (click here) </summary><p>
<code>s% mass_change = exp10(log_Mdot_interp)</code>
</p></details></hint>
<br>


### Task 2. Measuring the accretor at Helium flash
Run the model. Was it successful? If not, note the reason for the error. 

You should have received a Fortran runtime error pointing back to our run_star_extras modifications from Task 1. Recall that we are attempting to trace through a history file based on the current age of the star. At the same time, however, MESA is loading a saved model, treating it as the current star, and running through the entire history file before a step can occur. Let's make some modifications to <code>inlist_project</code> to remedy this. Set the initial age and model numbers to 0, then delete (or comment out) the accretion rate. 

Run the model (don't forget to clean and make). During the model's evolution, you should see a "lump" form that grows and ignites. Where is this "lump"?

Once the model has completed, open the history.data log. Use the data to find the rotation rate at the helium flash. Assume the accretor rotates as a solid body. Record this rotation rate, Helium shell thickness, and the time to Helium flash in [the google spreadsheet](https://docs.google.com/spreadsheets/d/1__UPg_5JfiBkJpZTleyaSwW_faxHzmo_X7Us2RTfLOM/edit#gid=1651867869). Compare the Helium shell thickness and time to Helium flash with the results from Lab 2. 

<hint><details>
<summary> Hint (click here) </summary><p>
Jdot = sqrt(GMR) * Mdot
</p></details></hint>

<hint><details>
<summary> Hint (click here) </summary><p>
Delta_J = Jdot * s% dt
</p></details></hint>

<hint><details>
<summary> Hint (click here) </summary><p>
rotation rate = Delta_J / I
</p></details></hint>

<hint><details>
<summary> Hint (click here) </summary><p>
The moment of inertia, I, of a solid sphere is (2/5)MR^2
</p></details></hint>
<br>


### Bonus Task. Exploring timescales


# BONUS. Evolutionary Timescale
<task><details><summary>Plot the density vs temperature of the constant case</summary></details></task>
<task><details><summary>Plot the density vs temperature of the varying case</summary></details></task>
<task><details><summary>Compare to Bauer Fig 8</summary></details></task>
* Expand out


* * *



[Back to main page](./)