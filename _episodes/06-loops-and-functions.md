---
title: Data Workflows and Automation
teaching: 30
exercises: 15
questions:
    - "Can I automate operations in Python?"
    - "What are functions and why should I use them?"
objectives:
    - "Describe why for loops are used in Python."
    - "Employ for loops to automate data analysis."
    - "Write unique filenames in Python."
    - "Build reusable code in Python."
    - "Write functions using conditional statements (if, then, else)."
keypoints:
    - "Loops help automate repetitive tasks over sets of items."
    - "Loops combined with functions provide a way to process data more efficiently than we could by hand."
    - "Conditional statements enable execution of different operations on different data."
    - "Functions enable code reuse."
---

So far, we've used Python and the pandas library to explore and manipulate
individual datasets by hand, much like we would do in a spreadsheet. The beauty
of using a programming language like Python, though, comes from the ability to
automate data processing through the use of loops and functions.

## For loops

Loops allow us to repeat a workflow (or series of actions) a given number of
times or while some condition is true. We would use a loop to automatically
process data that's stored in multiple files (daily values with one file per
year, for example). Loops lighten our work load by performing repeated tasks
without our direct involvement and make it less likely that we'll introduce
errors by making mistakes while processing each file by hand.

Let's write a simple for loop that simulates what a kid might see during a
visit to the zoo:

~~~
animals = ['lion', 'tiger', 'crocodile', 'vulture', 'hippo']
print(animals)
~~~
{: .language-python}

~~~
['lion', 'tiger', 'crocodile', 'vulture', 'hippo']
~~~
{: .output}

~~~
for creature in animals:
    print(creature)
~~~
{: .language-python}

~~~
lion
tiger
crocodile
vulture
hippo
~~~
{: .output}

The line defining the loop must start with `for` and end with a colon, and the
body of the loop must be indented.

In this example, `creature` is the loop variable that takes the value of the next
entry in `animals` every time the loop goes around. We can call the loop variable
anything we like. After the loop finishes, the loop variable will still exist
and will have the value of the last entry in the collection:

~~~
animals = ['lion', 'tiger', 'crocodile', 'vulture', 'hippo']
for creature in animals:
    pass
~~~
{: .language-python}

~~~
~~~
{: .output}

~~~
print('The loop variable is now: ' + creature)
~~~
{: .language-python}

~~~
The loop variable is now: hippo
~~~
{: .output}

We are not asking Python to print the value of the loop variable anymore, but
the for loop still runs and the value of `creature` changes on each pass through
the loop. The statement `pass` in the body of the loop means "do nothing".

> ## Challenge - Loops
>
> 1. What happens if we don't include the `pass` statement?
>
> 2. Rewrite the loop so that the animals are separated by commas, not new lines
> (Hint: You can concatenate strings using a plus sign. For example,
> `print(string1 + string2)` outputs 'string1string2').
{: .challenge}

## Automating data processing using For Loops

The file we've been using so far, `lobsters_data.csv` is
very large. We would like to separate the data for each year into a separate
file.

Let's start by making a new directory inside the folder `data` to store all of
these files using the module `os`:

~~~
import os

os.mkdir('data/yearly_files')
~~~
{: .language-python}

The command `os.mkdir` is equivalent to `mkdir` in the shell. Just so we are
sure, we can check that the new directory was created within the `data` folder:
~~~
os.listdir('data')
~~~
{: .language-python}
~~~
['lobsters_2012.csv',
 'lobsters_2013.csv',
 'lobsters_data.csv',
 'position_cols.csv',
 'time_cols.csv',
 'yearly_files']
~~~
{: .output}

The command `os.listdir` is equivalent to `ls` in the shell.

In previous lessons, we saw how to use the library pandas to load the lobster
data into memory as a DataFrame, how to select a subset of the data using some
criteria, and how to write the DataFrame into a CSV file. Let's write a script
that performs those three steps in sequence for the year 2012:

~~~
import pandas as pd

# Load the data into a DataFrame
lobsters_df = pd.read_csv('data/lobsters_data.csv')

# Select only data for the year 2012
lobs2012 = lobsters_df[lobsters_df.year == 2012]

# Write the new DataFrame to a CSV file
lobs2012.to_csv('data/yearly_files/lobs2012.csv')
~~~
{: .language-python}

To create yearly data files, we could repeat the last two commands over and
over, once for each year of data. Repeating code is neither elegant nor
practical, and is very likely to introduce errors into your code. We want to
turn what we've just written into a loop that repeats the last two commands for
every year in the dataset.

Let's start by writing a loop that prints the names of the files we want
to create - the dataset we are using covers 2012 through 2021, and we'll create
a separate file for each of those years. Listing the filenames is a good way to
confirm that the loop is behaving as we expect.

We have seen that we can loop over a list of items, so we need a list of years
to loop over. We can get the years in our DataFrame with:

~~~
lobsters_df['year']
~~~
{: .language-python}
~~~
0        2012
1        2012
2        2012
3        2012
4        2012
          ...
10698    2021
10699    2021
10700    2021
10701    2021
10702    2021
~~~
{: .output}

but we want only unique years, which we can get using the `unique` method
which we have already seen.

~~~
lobsters_df['year'].unique()
~~~
{: .language-python}
~~~
array([2012, 2013, 2014, 2015, 2016, 2017, 2018, 2019, 2020, 2021],
      dtype=int64)
~~~
{: .output}

Putting this into our for loop we get

~~~
for year in lobsters_df['year'].unique():
   filename='data/yearly_files/lobs' + str(year) + '.csv'
   print(filename)
~~~
{: .language-python}
~~~
data/yearly_files/lobs2012.csv
data/yearly_files/lobs2013.csv
data/yearly_files/lobs2014.csv
data/yearly_files/lobs2015.csv
data/yearly_files/lobs2016.csv
data/yearly_files/lobs2017.csv
data/yearly_files/lobs2018.csv
data/yearly_files/lobs2019.csv
data/yearly_files/lobs2020.csv
data/yearly_files/lobs2021.csv
~~~
{: .output}

We can now add the rest of the steps we need to create separate text files:

~~~
# Load the data into a DataFrame
lobsters_df = pd.read_csv('data/lobsters_data.csv')

for year in lobsters_df['year'].unique():

    # Select data for the year
    lobs_year = lobsters_df[lobsters_df.year == year]

    # Write the new DataFrame to a CSV file
    filename = 'data/yearly_files/lobs' + str(year) + '.csv'
    lobs_year.to_csv(filename)
~~~
{: .language-python}

Look inside the `yearly_files` directory and check a couple of the files you
just created to confirm that everything worked as expected.

## Writing Unique File Names

Notice that the code above created a unique filename for each year.

~~~
filename = 'data/yearly_files/lobs' + str(year) + '.csv'
~~~
{: .language-python}

Let's break down the parts of this name:

* The first part is some text that specifies the directory to store our
  data file in (data/yearly_files/) and the first part of the file name
  (lobs for lobsters): `'data/yearly_files/lobs'`
* We can concatenate this with the value of a variable, in this case `year` by
  using the plus `+` sign and the variable we want to add to the file name: `+
  str(year)`
* Then we add the file extension as another text string: `+ '.csv'`

Notice that we use single quotes to add text strings. The variable is not
surrounded by quotes. This code produces the string
`data/yearly_files/lobs2012.csv` which contains the path to the new filename
AND the file name itself.

> ## Challenge - Modifying loops
>
> 1. Some of the surveys you saved are missing data (they have null values that
> show up as NaN - Not A Number - in the DataFrames and do not show up in the text
> files). Modify the for loop so that the entries with null values are not
> included in the yearly files.
>
> 2. Let's say you only want to look at data from a given multiple of years. How would you modify your loop in order to generate a data file for only every 2 years, starting from 2012?
>
> 3. Instead of splitting out the data by years, a colleague wants to do analyses each species separately. How would you write a unique CSV file for each species?
{: .challenge}

## Building reusable and modular code with functions

Suppose that separating large data files into individual yearly files is a task
that we frequently have to perform. We could write a **for loop** like the one above
every time we needed to do it but that would be time consuming and error prone.
A more elegant solution would be to create a reusable tool that performs this
task with minimum input from the user. To do this, we are going to turn the code
we've already written into a function.

Functions are reusable, self-contained pieces of code that are called with a
single command. They can be designed to accept arguments as input and return
values, but they don't need to do either. Variables declared inside functions
only exist while the function is running and if a variable within the function
(a local variable) has the same name as a variable somewhere else in the code,
the local variable hides but doesn't overwrite the other.

Every method used in Python (for example, `print`) is a function, and the
libraries we import (say, `pandas`) are a collection of functions. We will only
use functions that are housed within the same code that uses them, but
we can also write functions that can be used by different programs.

Functions are declared following this general structure:

~~~
def this_is_the_function_name(input_argument1, input_argument2):

    # The body of the function is indented
    # This function prints the two arguments to screen
    print('The function arguments are:', input_argument1, input_argument2, '(this is done inside the function!)')

    # And returns their product
    return input_argument1 * input_argument2
~~~
{: .language-python}

The function declaration starts with the word `def`, followed by the function
name and any arguments in parenthesis, and ends in a colon. The body of the
function is indented just like loops are. If the function returns something when
it is called, it includes a return statement at the end.

This is how we call the function:

~~~
product_of_inputs = this_is_the_function_name(2, 5)
~~~
{: .language-python}

~~~
The function arguments are: 2 5 (this is done inside the function!)
~~~
{: .output}

~~~
print('Their product is:', product_of_inputs, '(this is done outside the function!)')
~~~
{: .language-python}

~~~
Their product is: 10 (this is done outside the function!)
~~~
{: .output}

> ## Challenge - Functions
>
> 1. Change the values of the arguments in the function and check its output
> 2. Try calling the function by giving it the wrong number of arguments (not 2)
>   or not assigning the function call to a variable (no `product_of_inputs =`)
> 3. Declare a variable inside the function and test to see where it exists (Hint:
>   can you print it from outside the function?)
> 4. Explore what happens when a variable both inside and outside the function
>   have the same name. What happens to the global variable when you change the
>   value of the local variable?
{: .challenge}

We can now turn our code for saving yearly data files into a function. There are
many different "chunks" of this code that we can turn into functions, and we can
even create functions that call other functions inside them. Let's first write a
function that separates data for just one year and saves that data to a file:

~~~
def one_year_csv_writer(this_year, all_data):
    """
    Writes a csv file for data from a given year.

    this_year -- year for which data is extracted
    all_data -- DataFrame with multi-year data
    """

    # Select data for the year
    lobs_year = all_data[all_data.year == this_year]

    # Write the new DataFrame to a csv file
    filename = 'data/yearly_files/function_file'+ str(this_year) + '.csv'
    lobs_year.to_csv(filename)
~~~
{: .language-python}

The text between the two sets of triple double quotes is called a docstring and
contains the documentation for the function. It does nothing when the function
is running and is therefore not necessary, but it is good practice to include
docstrings as a reminder of what the code does. Docstrings in functions also
become part of their 'official' documentation:

~~~
one_year_csv_writer?
~~~
{: .language-python}

~~~
one_year_csv_writer(2012, lobsters_df)
~~~
{: .language-python}

We changed the root of the name of the CSV file so we can distinguish it from
the one we wrote before. Check the `yearly_files` directory for the file. Did it
do what you expect?

What we really want to do, though, is create files for multiple years without
having to request them one by one. Let's write another function that replaces
the entire `for` loop by looping through a sequence of years and repeatedly
calling the function we just wrote, `one_year_csv_writer`:


~~~
def yearly_data_csv_writer(start_year, end_year, all_data):
    """
    Writes separate CSV files for each year of data.

    start_year -- the first year of data we want
    end_year -- the last year of data we want
    all_data -- DataFrame with multi-year data
    """

    # "end_year" is the last year of data we want to pull, so we loop to end_year+1
    for year in range(start_year, end_year+1):
        one_year_csv_writer(year, all_data)
~~~
{: .language-python}

Because people will naturally expect that the end year for the files is the last
year with data, the for loop inside the function ends at `end_year + 1`. By
writing the entire loop into a function, we've made a reusable tool for whenever
we need to break a large data file into yearly files. Because we can specify the
first and last year for which we want files, we can even use this function to
create files for a subset of the years available. This is how we call this
function:

~~~
# Load the data into a DataFrame
lobsters_df = pd.read_csv('data/lobsters_data.csv')

# Create CSV files
yearly_data_csv_writer(2012, 2021, lobsters_df)
~~~
{: .language-python}

BEWARE! If you are using Jupyter Notebooks and you modify a function, you MUST
re-run that cell in order for the changed function to be available to the rest
of the code. Nothing will visibly happen when you do this, though, because
defining a function without *calling* it doesn't produce an output. Any
cells that use the now-changed functions will also have to be re-run for their
output to change.

> ## Challenge: More functions
>
> 1. Add two arguments to the functions we wrote that take the path of the
>    directory where the files will be written and the root of the file name.
>    Create a new set of files with a different name in a different directory.
> 2. How could you use the function `yearly_data_csv_writer` to create a CSV file
>    for only one year? (Hint: think about the syntax for `range`)
> 3. Make the functions return a list of the files they have written. There are
>    many ways you can do this (and you should try them all!): either of the
>    functions can print to screen, either can use a return statement to give back
>    numbers or strings to their function call, or you can use some combination of
>    the two. You could also try using the `os` library to list the contents of
>    directories.
> 4. Explore what happens when variables are declared inside each of the functions
>    versus in the main (non-indented) body of your code. What is the scope of the
>    variables (where are they visible)? What happens when they have the same name
>   but are given different values?
{: .challenge}

The functions we wrote demand that we give them a value for every argument.
Ideally, we would like these functions to be as flexible and independent as
possible. Let's modify the function `yearly_data_csv_writer` so that the
`start_year` and `end_year` default to the full range of the data if they are
not supplied by the user. Arguments can be given default values with an equal
sign in the function declaration. Any arguments in the function without default
values (here, `all_data`) is a required argument and MUST come before the
argument with default values (which are optional in the function call).

~~~
def yearly_data_arg_test(all_data, start_year=2012, end_year=2021):
    """
    Modified from yearly_data_csv_writer to test default argument values!

    start_year -- the first year of data we want (default 1977)
    end_year -- the last year of data we want (default 2002)
    all_data -- DataFrame with multi-year data
    """

    return start_year, end_year


start, end = yearly_data_arg_test(lobsters_df, 2015, 2020)
print('Both optional arguments:\t', start, end)

start, end = yearly_data_arg_test(lobsters_df)
print('Default values:\t\t\t', start, end)
~~~
{: .language-python}

~~~
Both optional arguments:   2015 2020
Default values:   2012 2021
~~~
{: .output}

The "\t" in the `print` statements are tabs, used to make the text align and be
easier to read.

But what if our dataset doesn't start in 2012 and end in 2021? We can modify the
function so that it looks for the start and end years in the dataset if those
dates are not provided:

~~~
def yearly_data_arg_test(all_data, start_year=None, end_year=None):
    """
    Modified from yearly_data_csv_writer to test default argument values!

    all_data -- DataFrame with multi-year data
    start_year -- the first year of data we want, Check all_data! (default None)
    end_year -- the last year of data we want; Check all_data! (default None)
    """

    if start_year is None:
        start_year = min(all_data.year)
    if end_year is None:
        end_year = max(all_data.year)

    return start_year, end_year


start, end = yearly_data_arg_test(lobsters_df, 2015, 2020)
print('Both optional arguments:\t', start, end)

start, end = yearly_data_arg_test(lobsters_df)
print('Default values:\t\t\t', start, end)
~~~
{: .language-python}
~~~
Both optional arguments:	 2015 2020
Default values:   2012 2021
~~~
{: .output}

The default values of the `start_year` and `end_year` arguments in the function
`yearly_data_arg_test` are now `None`. This is a built-in constant in Python
that indicates the absence of a value - essentially, that the variable exists in
the namespace of the function (the directory of variable names) but that it
doesn't correspond to any existing object.

> ## Challenge - Variables
>
> 1. What type of object corresponds to a variable declared as `None`? (Hint:
> create a variable set to `None` and use the function `type()`)
>
> 2. Compare the behavior of the function `yearly_data_arg_test` when the
> arguments have `None` as a default and when they do not have default values.
>
> 3. What happens if you only include a value for `start_year` in the function
> call? Can you write the function call with only a value for `end_year`? (Hint:
> think about how the function must be assigning values to each of the arguments -
> this is related to the need to put the arguments without default values before
> those with default values in the function definition!)
{: .challenge}

## If Statements

The body of the test function now has two conditionals (if statements) that
check the values of `start_year` and `end_year`. If statements execute a segment
of code when some condition is met. They commonly look something like this:

~~~
a = 5

if a<0:  # Meets first condition?

    # if a IS less than zero
    print('a is a negative number')

elif a>0:  # Did not meet first condition. meets second condition?

    # if a ISN'T less than zero and IS more than zero
    print('a is a positive number')

else:  # Met neither condition

    # if a ISN'T less than zero and ISN'T more than zero
    print('a must be zero!')
~~~
{: .language-python}

Which would return:

~~~
a is a positive number
~~~
{: .output}

Change the value of `a` to see how this function works. The statement `elif`
means "else if", and all of the conditional statements must end in a colon.

The if statements in the function `yearly_data_arg_test` check whether there is an
object associated with the variable names `start_year` and `end_year`. If those
variables are `None`, the if statements return the boolean `True` and execute whatever
is in their body. On the other hand, if the variable names are associated with
some value (they got a number in the function call), the if statements return `False`
and do not execute. The opposite conditional statements, which would return
`True` if the variables were associated with objects (if they had received value
in the function call), would be `if start_year` and `if end_year`.

As we've written it so far, the function `yearly_data_arg_test` associates
values in the function call with arguments in the function definition just based
on their order. If the function gets only two values in the function call, the
first one will be associated with `all_data` and the second with `start_year`,
regardless of what we intended them to be. We can get around this problem by
calling the function using keyword arguments, where each of the arguments in the
function definition is associated with a keyword and the function call passes
values to the function using these keywords:

~~~
start, end = yearly_data_arg_test(lobsters_df)
print('Default values:\t\t\t', start, end)

start, end = yearly_data_arg_test(lobsters_df, 2015, 2020)
print('No keywords:\t\t\t', start, end)

start, end = yearly_data_arg_test(lobsters_df, start_year=2015, end_year=2020)
print('Both keywords, in order:\t', start, end)

start, end = yearly_data_arg_test(lobsters_df, end_year=2020, start_year=2015)
print('Both keywords, flipped:\t\t', start, end)

start, end = yearly_data_arg_test(lobsters_df, start_year=2015)
print('One keyword, default end:\t', start, end)

start, end = yearly_data_arg_test(lobsters_df, end_year=2020)
print('One keyword, default start:\t', start, end)

~~~
{: .language-python}
~~~
Default values:   2012 2021
No keywords:   2015 2020
Both keywords, in order:   2015 2020
Both keywords, flipped:   2015 2020
One keyword, default end:   2015 2021
One keyword, default start:   2012 2020
~~~
{: .output}

> ## Challenge - Modifying functions
>
> 1. Rewrite the `one_year_csv_writer` and `yearly_data_csv_writer` functions to
> have keyword arguments with default values
>
> 2. Modify the functions so that they don't create yearly files if there is no
> data for a given year and display an alert to the user (Hint: use conditional
> statements to do this. For an extra challenge, use `try`
> statements!)
>
> 3. The code below checks to see whether a directory exists and creates one if it
> doesn't. Add some code to your function that writes out the CSV files, to check
> for a directory to write to.
>
> ~~~
>if 'dir_name_here' in os.listdir('.'):
>    print('Processed directory exists')
>else:
>    os.mkdir('dir_name_here')
>    print('Processed directory created')
> ~~~
> {: .language-python }
>
> 4. The code that you have written so far to loop through the years is good,
> however it is not necessarily reproducible with different datasets.
> For instance, what happens to the code if we have additional years of data
> in our CSV files? Using the tools that you learned in the previous activities,
> make a list of all years represented in the data. Then create a loop to process
> your data, that begins at the earliest year and ends at the latest year using
> that list.
>
> HINT: you can create a loop with a list as follows: `for years in year_list:`
{: .challenge}

{% include links.md %}
