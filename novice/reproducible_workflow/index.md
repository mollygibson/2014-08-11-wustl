---
layout: lesson
root: ../..
title: Creating a Reproducible Workflow
---

Objectives
---------
- Learn about reproducible research.
- Organize a research project to be reproducible.
- Write a python module to analyze data and visualize it.
- Use the best-practices we've 

Introduction
------------

We're now in the home stretch of the workshop - congratulations! Up to this
point, we've talked about how to make your code efficient (good programming
practice), accurate (testing), and maintainable (version control). Now we're
going to bring it all back to science by discussing reproducibility.

What does it mean for science to be reproducible? In the words of Buckheit and
Donoho (2005):

**"An article about a computational result is advertising, not
scholarship. The actual scholarship is the full software environment code and
data, that produced the result."**

Victoria Stodden has written extensively about the idea of
reproducibility in scientific software - you may want to look up [some of her
papers](http://www.stanford.edu/~vcs/Papers.html) for reference.

For our purposes, we can summarize the goal of reproducibility in two related 
ways, one technical and one colloquial.

In a technical sense, your goal is to __have a complete chain of custody (ie, 
provenance) from your raw data to your finished results and figures__. That is, 
you should _always_ be able to figure out precisely what data and what code 
were used to generate what result - there should be no "missing links". If you 
have ever had the experience of coming across a great figure that you made 
months ago and having no idea how in the world you made it, then you understand 
why provenance is important. Or, worse, if you've ever been unable to recreate 
the results that you once showed on a poster or (gasp) published in a 
paper...

In a colloquial sense, I should be able to sneak into your lab late at night, 
delete everything except for your raw data and your code, and __you should be 
able to run a single command to regenerate EVERYTHING, including all of your 
results, tables, and figures in their final, polished form__. Think of this as 
the "push button" workflow. This is your ultimate organizational goal as a 
computational scientist. Importantly, note that this rules out the idea of 
manual intervention at any step along the way - no tinkering with figure axes 
in a pop-up window, no deleting columns from tables, no copying data from one 
folder to another, etc. All of that needs to be fully automated.

As an added bonus, if you couple this with a version control system that tracks 
changes over time to your raw data and your code, you will be able to instantly 
recreate your results from any stage in your research (the lab presentation 
version, the dissertation version, the manuscript version, the Nobel Prize 
committee version, etc.). Wouldn't that be nice?

To illustrate these ideas, we'll set up a small but realistic research project 
that follows a reproducible workflow. Just like you would in your own research 
projects, we'll go through the following key steps:

1.	Create a clear and useful directory structure for our project.
2.	Set up (and use) Git to track our changes.
3.	Add the raw data to our project.
4.	Write the core "scientific code" to perform the analysis, including tests.
5.     Create notebooks to generate the results for our specific project.

One final note - the workflow that we're following here is just a suggestion.
Organizing code and data is an art, and a room of 100 scientists will give you
101 opinions about how to do it best. Consider the below a useful place to get
started, and don't hesitate to tinker and branch out as you get a better feel
for this process. You also might want to review [William Noble's
paper](http://dx.doi.org/10.1371/journal.pcbi.1000424) on this topic for more
ideas.

Set up the project directory
-------------------------

Let's create a project (a reasonably self-contained set of code, data, and 
results to answer a discrete scientific question) that will compare
inflammation levels. We begin by creating a directory 
called `inflammation` in a convenient place on our hard drive. You might 
want to create a main directory called `Projects` or `Research` in your home 
folder or in your Documents folder to hold the directories for all of your 
individual research projects.

Now, within the `inflammation` directory, create four subdirectories:

    .
    |-- data
    |-- notebooks
    |-- src

The `data` directory will hold all of the raw data associated with the project, 
which in this case will be a bunch of csv files containing data on inflammation
levels in several patients. The `notebooks` folder, will contain the results of
our analysis, while the `src` directory will contain the code we use to create
these notebooks.

In a more complex project, each of these directories may have additional 
subdirectories to help keep things organized.

For bonus points, do this all from the command line.

## 1. Initialize a Git repository
-------------------------

Since we want to use version control to track the development of our project, 
we'll start off right away by initializing an empty Git repository within this 
directory. To do this, open a Terminal window, navigate to the main 
`inflammation` directory, and run the command `git init`.

As you add things to the project directory, and modify old things, you'll want 
to frequently commit your changes as we discussed in the Git tutorial.

## 2. Add raw data
-----------------

Often, we start a project with a particular data file, or set of data files. In 
this case, we have the files `inflammation-*.csv`, which contains the records 
that we want to analyze. Download these files [here](inflammation.zip) and 
place it in the `data` subdirectory.

Now we reach an interesting question - should your `data` directory be placed 
under version control (ie, should you `git add` and `git commit` these files)? 
Although you might automatically think that this is necessary, in principle our 
raw data should never change - that is, there's only one version (the original 
version!), and it will never be updated. As a result, it's not necessarily 
useful to place this file under version control for the purpose of tracking 
changes to it.

A reasonable rule of thumb for getting started is that if the file is 
realatively small (ours is < 100k), go ahead and commit it to the Git 
repository, as you won't be wasting much hard disk space. Additionally, the 
file will then travel with your code, so if you push your repository to Github 
(for example) and one of your collaborators clones a copy, they'll have 
everything they need to generate your results.

However, if your file is realatively large AND is backed up elsewhere, you 
might want to avoid making a duplicate copy in the `.git` directory.

In either case, you'll want to ensure that every one of your data files has
some sort of metadata associated with it to describe where it came from, how it
got to you, the meaning of the columns, etc. There are many formats for
metadata that vary from simple to very complex. For your own private work, make
sure that, at a minimum, you create a `README.txt` file that describes your
data as best you can.

Copy and paste the text below into a `README.txt` file and place it in the data 
subdirectory. Remember that this is a bare-bones description - in your own 
work, you'll want to include as much information as you have.

	Data received via email on April 1, 2013 from Professor Smith. Includes
	records from patient surveys conducted by John Doe and Jane Doe from
	2011-2012. Method of collection, and additional descriptions are found
	in John Doe's dissertation, Chapter 3 Appendix, filed August 2012 at UC
	Berkeley.

At this point, your project directory should look like this:

	.
    |-- data
    |   |-- README.txt
    |   |--inflammation-01.csv
    |   |--inflammation-02.csv
     ... 
    |-- notebooks
    |-- src

Commit both the data and README files to your git repository.

## 3. Write code to perform analysis
-------------------------------

Now for the real work - writing the code and notebooks that perform our analysis. 

### Modules

To be able to reuse our code, we would like to write Python modules that
contain flexible, and tested functions. We'll keep it simple, for the sake of
demonstration.

We'll rely on some of the work we did earlier today. Let's start by copying and
pasting the code from our lesson about loops into a file that we will call
`analyze_inflammation.py`. We'll combine together a few of the things we did today, to
demonstrate the general ideas :

    import os
    import numpy as np
    from matplotlib import pyplot as plt

    def center(data, desired=0.0):
         """Return a new array containing the original data centered around the
         desired value (0 by default).

	 Example: center([1, 2, 3], 0) => [-1, 0, 1]"""

	 return (data -data.mean()) + desired
    
    def plot_data(filename):
    	data = np.loadtxt(fname=filename, delimiter=',')
    
        plt.figure(figsize=(10.0, 3.0))
    
        plt.subplot(1, 3, 1)
    	plt.ylabel('average')
    	plt.plot(data.mean(0))
    
        plt.subplot(1, 3, 2)
   	plt.ylabel('max')
    	plt.plot(data.max(0))
    
        plt.subplot(1, 3, 3)
    	plt.ylabel('min')
	plt.plot(data.min(0))
    
        plt.tight_layout()
    	plt.show()

	
    def analyze_all(data_path=(pattern):
        filenames = glob.glob(pattern)
        for f in filenames:
            print f
            plot_data(f)

    
Note that, by itself, the code in our `analyze_inflammation.py` file doesn't
actually generate any results for us - this file just contains the core
function definitions for the functions that we have written to perform our
analysis. Later on, we'll write notebooks that produce analysis results and
figures. It is generally good practice to keep your actual analysis code in a
separate module where it can easily be tested and reused later on. This
separation of concerns also supports a mental separation of roles - the module
is where you write and test the guts of your code (the stuff that "does
science") whereas the notebooks (or alternatively, a `runall` script, are just
the parts of the code that take this "science" and applies it to your
particular data set to generate your particular results (the stuff that spits
out "these results"). If you wanted to perform the same analysis on a different
data set or with a different set of input parameters, for example, you should
be able to accomplish that by modifying just the notebooks/`runall` file. We'll
look at that later.

Normally, you'd spend days/weeks/months working in the `src` directory, writing
code and tests, generating new ideas and new results, looking at the results,
writing new code and tests, generating new results, etc. This iterative cycle isn't unlike
writing a paper - you spew out a draft that's not too bad, then go back and
revise it, then spew out some new material, revise that, etc.

Different people have different favorite approaches and tools for this 
iterative cycle.

One strategy is to simultaneously work on three files at once - a module like 
`analye_inflammation.py`, a file to test the functions in your module like 
`test_analyze_inflammation.py`, and a set of notebooks or a script that call
various functions from  your module and runs them so that you can see whether
your code is doing what you want it to do. 

### Documentation

Documentation is an important part of creating a reproducible workflow - rather 
than simply reproducing results, you can think of documentation as helping you 
to reproduce the thought process that led you to design your code in a 
particular way. Being able to call up and recreate the thought process that led 
to your code is imporant both for "future you", who will eventually come back 
to this code in other projects, or at a minimum when writing the manuscript for 
this project, and for other users who might need to apply or maintain your code 
in the future.

Every programming language has its own guidelines and style for documentation - 
regardless of what language you're using, I'd suggest Googling something like 
"<my language name> style guide", and similar strings, to read about common 
conventions for that language. Below, we'll briefly discuss documentation in 
the context of Python modules, but a similar high-level conceptual framework 
will apply to any language.

In short, I would suggest mentally dividing your documentation into four basic 
levels, ranging from the smallest to the broadest scale.

#### 1. Line-level comments

At the smallest scale are line-by-line comments on your code, such as


    # We only analyze individuals with smaller than average inflammation:
    if data[indv, :].mean() < ave_inflammation:
        first_ten = data[indv, 0:10].mean()

Code comments such as these should generally be restricted to one line, 
although two or three lines would be OK for cases that require more 
explanation. Most importantly, comments should __describe what the code is 
intended to do and why__, not simply repeat literally what the code does. The 
above comment, for example, explains the purpose of the subsequent lines. In 
contrast, the comment below is basically useless, as it simply repeats what 
anyone reading the code could have already told you.

    # Set x to zero
    x = 0


A better option would be


    # Initialize running count of individuals
    x = 0


There's an art to determining how many comments are too many. It is good to be
rather verbose, especially when doing rather complicated stuff. It will happen
"future you" find the sections of my code that perform particular conceptual
steps.

Finally, make sure not to let your comments get out of date - when you update 
your code, you __must__ update the corresponding comments. An out of date or 
incorrect comment is worse than no comment at all.

At this point, open your `analyze_inflammation.py` file and add a few code comments.

#### 2. Function-level definitions

All programming languages have conventions surrounding how to document the
operations of a function.  In Python, a description of a function is known as a
docstring.  We've seen lots of function definitions throughout this bootcamp
and as we previously discussed, a nice feature of docstrings is that they
integrate nicely into the ways of getting help in IPython (recall the `?` help
function), as well as with other documentation tools such as Sphinx that we'll
mention later. I recommend using the [numpy docstring
standards](https://github.com/numpy/numpy/blob/master/doc/HOWTO_DOCUMENT.rst.txt#docstring-standard). This is the standard used by many scientific python
projects (including numpy/scipy/...), and you will get used to expecting this
format.

Let's make over the docstring that we have for the `center` function:

    def center(data, desired=0.0):
    	"""Return a new array containing the original data centered around the
    	desired value (0 by default).

	Parameters
	--------
	data : 1d array
	    The input data

	desired : float
	    The number around which to center the array 

	Returns
	------
	The original input array, recentered around `desired`.
	 
	Examples
	-------
	>>> center([1, 2, 3], 0)
	    [-1, 0, 1]

        Notes
        -----
	This operation does not change the variance of the input, just it's
        mean.

	"""

Note a few important features of this docstring. First, it's indented, just
like all of the other code within a function. Second, it starts immediately
after the line defining the function, and starts and ends with three quotation
marks (which, in Python, defines a multi-line string). Third, the first line of
the docstring is a single line describing the high level purpose and/or
function of the function. The rest of the docstring structure is not as
standardized, but the above example is a basic form that's common in scientific
Python packages.


If you haven't already, add a nice docstring to your function(s) in
`analyze_inflammation.py`, or fix up the ones that are already there. Imagine
that you will want to know how to use this function in 6 months, but you will
not to read the code.

#### 3. Module-level documentation

At a higher level, you should also provide some overarching documentation of 
each of your Python module files. This is usually a relatively short summary, 
compared to a function-level docstring, that states the purposes of the module 
and lists what the module contains.

    """
    Module containing functions to analyze data from inflammation
    
    Functions
    ---------
    center - center an array around a float
    ... 
    """

As with function docstrings in Python, a module docstring is set off by triple 
quotes and appears at the very top of the module.

If you haven't already, add a short module docstring to `analyze_inflammation.py`.

#### 4. Package-level and user documentation

At the highest level of documentation, we find information that is intended 
(mostly) to be read by users of your code to gain an overview of everything 
that your code does. We won't talk about this level in detail, as it only 
matches the other three levels in importance for larger projects that are 
shared widely and maintained on an ongoing basis (which may not apply to many 
of your research projects). If you are interested in learning more about this 
process for Python packages, I'd suggest having a look at 
[Sphinx](http://sphinx-doc.org/), which is the tool used to create 
documentation for Python itself as well as most of the main scientific 
packages. The websites documenting [core Python](http://docs.python.org/2/), 
[matplotlib](http://matplotlib.org/), and 
[numpy](http://docs.scipy.org/doc/numpy/) provide some useful examples of 
Sphinx in use as well as some general documentation styles that you might wish 
to review.

## 4. Test your code
-----------------

OK, back to our project. We now have the file `analyze_inflammation.py` in our `src` 
directory. Now lets create a `tests` directory under that, and create a new
file that will contain our unit tests:

    |-- data
    |   |-- README.txt
    |   |--inflammation-01.csv
    |   |--inflammation-02.csv
     ... 
    |-- notebooks
    |-- src
    |   |-- tests
    |   |   |-- test_analyze_inflammation.py


For the purpose of the demonstration, we will test the `center` function, but
you will want to write tests for all of your functions

It can be a bit awkward deciding where to place these test data files - you
could potentially put it in `data`, or here in the `tests` directory. You may
want to create a readme file for the test data sets you create, and keep track
of scripts you wrote to create it (you aren't going to do this by hand, are
you?) as well so that you can remember how it was created.

In test_analyze_inflammation, we use the following code:

    import sys
    sys.path.append('/Users/arokem/projects/inflammation/src/')
    import analyze_inflammation as ana

    import numpy as np

    def test_center():
         input = np.array([1, 2, 3])
	 output = ana.center(input)
	 assert(np.all(output == np.array([-1, 0, 1])))


Let's break this down. The first few lines are all about importing the module
that you have created. There is a way to get your code into the general system
`PYTHONPATH` (which is a set of file-system locations from which modules can be
imported), but we will not go into that for now. Instead, we use a local
solution that brings in your code into the name-space of the test function.

We will use `nosetests` from the command line to run the tests. This program
traverses the file-system from wherever it is called, looking for files called
`test_*.py`. It will then run any function within these files called
`test_*`. If no errors are raised during this run, it will tell you how many
functions it ran and tell you that everything is OK. On the other hand, if an
assertion is raised with an error, we 

Testing functions that create visualizations, or that require data files is
more challenging, but you should write these tests as well. It is a good idea
to create minimal data files, that contain just enough data to test the
functions that read this data, but no more and that contain contrived, very
simple data, for which you can easily cacluclate by hand what the answer should
be. 
	
## 5. Analyze your data and get a really reproducible paper 
----------------------------------------------------

Ideally, every claim in the paper, and every figure in the paper will be
supported by a full chain of custody. This will make the paper really
reproducible for anyone who reads it.

A recent example is a paper by Waskom et al. in the [Journal of
Neuroscience](http://www.jneurosci.org/content/34/32/10743). This paper has a
[github repository](https://github.com/WagnerLabPapers/Waskom_JNeurosci_2014)
that contains all of the code that was used to reach the conclusions in the
paper. For example,
[this](http://nbviewer.ipython.org/github/WagnerLabPapers/Waskom_JNeurosci_2014/blob/master/Searchlight_Analysis.ipynb)
is figure 5 in the paper.



