


# Markdown and HTML stuff stolen from May

In the following `MESA` lab exercises, you will see different text boxes and colours appearing at different times. <br>

Text boxes with black background will provide either some terminal commands or terminal output from your `MESA` and `GYRE` runs. (Courtney note this DOES NOT WORK in our github pages theme!!)

<div class="terminal-title"> Terminal commands or output </div> 
<div class="terminal"><p>
A terminal command or output will appear here
</p></div>


The second type of text box has a white background, and lists example contents of different files that you need to use/modify or `fortran` coding examples.

<div class="filetext-title"> File name or type of fortran commands </div> 
<div class="filetext"><p>
Text inside file or example fortran coding snippets go here.
</p></div>


Aside from the two types of text boxes, there are three different types of coloured text that you will need to pay attention to. <a href="https://docs.mesastar.org/en/release-r23.05.1/" target="_blank"> Text of this colour can be clicked</a> and will either take you to a website or give you downloadable content that you need for the exercise, including solutions to Minilab 1, Minilab 2, and the Maxilab. The remaining two types of text colours correspond to specific tasks that you have to complete and some useful hints along the way, click on them to expand the task or hint.

<task><details>
<summary>Task 0</summary><p>
This is an example of how a specific task will show up in the following <code>MESA</code> labs.
</p></details></task>


<hint><details>
<summary> Hint </summary><p>
This is an example of how hints to different tasks will show up in the text.
</p></details></hint>




# Markdown tutorial stuff below here

Text can be **bold**, _italic_, or ~~strikethrough~~.

[Link to another page](./another-page.html).

There should be whitespace between paragraphs.

There should be whitespace between paragraphs. We recommend including a README, or a file with information about your project.

# Header 1

This is a normal paragraph following a header. GitHub is a code hosting platform for version control and collaboration. It lets you and others work together on projects from anywhere.

## Header 2

> This is a blockquote following a header.
>
> When something is important enough, you do it even if the odds are not in your favor.

### Header 3

```js
// Javascript code with syntax highlighting.
var fun = function lang(l) {
  dateformat.i18n = require('./lang/' + l)
  return true;
}
```

```ruby
# Ruby code with syntax highlighting
GitHubPages::Dependencies.gems.each do |gem, version|
  s.add_dependency(gem, "= #{version}")
end
```

#### Header 4

*   This is an unordered list following a header.
*   This is an unordered list following a header.
*   This is an unordered list following a header.

##### Header 5

1.  This is an ordered list following a header.
2.  This is an ordered list following a header.
3.  This is an ordered list following a header.

###### Header 6

| head1        | head two          | three |
|:-------------|:------------------|:------|
| ok           | good swedish fish | nice  |
| out of stock | good and plenty   | nice  |
| ok           | good `oreos`      | hmm   |
| ok           | good `zoute` drop | yumm  |

### There's a horizontal rule below this.

* * *

### Here is an unordered list:

*   Item foo
*   Item bar
*   Item baz
*   Item zip

### And an ordered list:

1.  Item one
1.  Item two
1.  Item three
1.  Item four

### And a nested list:

- level 1 item
  - level 2 item
  - level 2 item
    - level 3 item
    - level 3 item
- level 1 item
  - level 2 item
  - level 2 item
  - level 2 item
- level 1 item
  - level 2 item
  - level 2 item
- level 1 item

### Small image

![Octocat](https://github.githubassets.com/images/icons/emoji/octocat.png)
![alt_text](./img/file_path.png)


### Large image

![Branching](https://guides.github.com/activities/hello-world/branching.png)


### Definition lists can be used with HTML syntax.

<dl>
<dt>Name</dt>
<dd>Godzilla</dd>
<dt>Born</dt>
<dd>1952</dd>
<dt>Birthplace</dt>
<dd>Japan</dd>
<dt>Color</dt>
<dd>Green</dd>
</dl>

```
Long, single-line code blocks should not wrap. They should horizontally scroll if they are too long. This line should be long enough to demonstrate this.
```

```
The final element.
```



* * *

# TA notes

Bonus task: run through helium flash over lunch time if you wish. - see the heating timescale getting short (some cases where the heating timescale is shorter than the dynamical timescale or the sound speed, there is a time where the heating timescale gets long)

* Could add str_ptr mixing_type (0 to 7, 1=convection), to find the mass of the convective zone rather than just using 10^3 or 4 in our case (this is a different estimate than the pure subtraction)

* Court note: not done yet but closer than before. running with updated rates and network runs at 3 min runtime on 4 cores

adjust the reaction network to add in the reaction chain $$^{14}N(e^-,\nu)^{14}C(\alpha,\gamma)^{18}O$$ which we will call the NCO reaction.
* [link to the networks docs](https://docs.mesastar.org/en/latest/net/nets.html)

* Courtney note: okay I've been playing with this and I think all we need is to add c14 and o18 to the list of isos, and then I assume the reaction will automatically be added. Evan Bauer's paper has a more complicated patch, but I would assume that got fixed after the version update (they used r8118 lol)
* there is now a test_suite case called custom_rates that implements the updated reaction rate used in the Bauer paper if we wanted to do that?? 
* I need to figure out if or how you would make the reaction chain into its own group for outputting the energy generated by just this reaction chain
<code>add_isos(
c14
o18)</code>

* outputting the energy generated by this reaction should be fairly simple to do using profile_defaults, but then what do we do with that? do we care?
* have them add in the reaction rates
* at 10^-8 rates you should check to see the NCO is actually working by looking for o18 in the star abundance profile

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