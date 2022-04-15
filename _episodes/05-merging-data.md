---
title: Combining DataFrames with Pandas
teaching: 20
exercises: 25
questions:
  - "Can I work with data from multiple sources?"
  - "How can I combine data from different data sets?"
objectives:
    - "Combine data from multiple files into a single DataFrame using merge and concat."
    - "Combine two DataFrames using a unique ID found in both DataFrames."
    - "Employ `to_csv` to export a DataFrame in CSV format."
    - "Join DataFrames using common fields (join keys)."
keypoints:
    - "Pandas' `merge` and `concat` can be used to combine subsets of a DataFrame, or even data from different files."
    - "`join` function combines DataFrames based on index or column."
    - "Joining two DataFrames can be done in multiple ways (left, right, and inner) depending on what data must be in the final DataFrame."
    - "`to_csv` can be used to write out DataFrames in CSV format."
---
79
In many "real world" situations, the data that we want to use come in multiple
files. We often need to combine these files into a single DataFrame to analyze
the data. The pandas package provides [various methods for combining
DataFrames](https://pandas.pydata.org/pandas-docs/stable/user_guide/merging.html) including
`merge` and `concat`.

To work through the examples below, we first need to load the time and
position files into pandas DataFrames. In iPython:

~~~
import pandas as pd

position_cols = pd.read_csv('data/position_cols.csv', keep_default_na=False, na_values=[""])

position_cols = position_cols[['record_id', 'site', 'transect', 'replicate']]

position_cols

    record_id  site  transect replicate
0           1  IVEE         1         A
1           2  IVEE         5         B
2           3  IVEE         5         B
3           4  IVEE         5         B
4           5  IVEE         5         C
...       ...   ...       ...       ...
45         46  IVEE         3         C
46         47  IVEE         3         C
47         48  IVEE         4         A
48         49  NAPL         6         D
49         50  NAPL         8         A
50         51  NAPL         7         A

[50 rows x 4 columns]

time_cols = pd.read_csv('data/time_cols.csv', keep_default_na=False, na_values=[""])

time_cols = time_cols[['record_id', 'year', 'month', 'date']]

time_cols

    record_id  year  month        date
0           1  2012      8  2012-08-20
1           2  2012      8  2012-08-20
2           3  2012      8  2012-08-20
3           4  2012      8  2012-08-20
4           5  2012      8  2012-08-20
5           6  2012      8  2012-08-20
...       ...   ...    ...         ...
45         46  2012      8  2012-08-20
46         47  2012      8  2012-08-20
47         48  2012      8  2012-08-20
48         49  2012      8  2012-08-22
49         50  2012      8  2012-08-22
50         51  2012      8  2012-08-22

[50 rows x 4 columns]
~~~
{: .language-python}

Take note that the `read_csv` method we used can take some additional options which
we didn't use previously. Many functions in Python have a set of options that
can be set by the user if needed. In this case, we have told pandas to assign
empty values in our CSV to NaN `keep_default_na=False, na_values=[""]`.
[More about all of the read_csv options here.](https://pandas.pydata.org/pandas-docs/stable/reference/api/pandas.read_csv.html#pandas.read_csv)

# Concatenating DataFrames

We can use the `concat` function in pandas to append either columns or rows from
one DataFrame to another.  Let's grab two subsets of our data to see how this
works.

~~~
# Read in first 10 lines
lobster_sub = lobster_df.head(10)
# Grab the last 10 rows
lobster_sub_last10 = lobster_df.tail(10)
# Reset the index values to the second dataframe appends properly
lobster_sub_last10 = lobster_sub_last10.reset_index(drop=True)
# drop=True option avoids adding new index column with old index values
~~~
{: .language-python}

When we concatenate DataFrames, we need to specify the axis. `axis=0` tells
pandas to stack the second DataFrame UNDER the first one. It will automatically
detect whether the column names are the same and will stack accordingly.
`axis=1` will stack the columns in the second DataFrame to the RIGHT of the
first DataFrame. To stack the data vertically, we need to make sure we have the
same columns and associated column format in both datasets. When we stack
horizontally, we want to make sure what we are doing makes sense (i.e. the data are
related in some way).

~~~
# Stack the DataFrames on top of each other
vertical_stack = pd.concat([lobster_sub, lobster_sub_last10], axis=0)

# Place the DataFrames side by side
horizontal_stack = pd.concat([lobster_sub, lobster_sub_last10], axis=1)
~~~
{: .language-python}

### Row Index Values and Concat
Have a look at the `vertical_stack` dataframe? Notice anything unusual?
The row indexes for the two data frames `lobster_sub` and `lobster_sub_last10`
have been repeated. We can reindex the new dataframe using the `reset_index()` method.

## Writing Out Data to CSV

We can use the `to_csv` command to do export a DataFrame in CSV format. Note that the code
below will by default save the data into the current working directory. We can
save it to a different folder by adding the foldername and a slash to the file
`vertical_stack.to_csv('foldername/out.csv')`. We use the 'index=False' so that
pandas doesn't include the index number for each line.

~~~
# Write DataFrame to CSV
vertical_stack.to_csv('data/out.csv', index=False)
~~~
{: .language-python}

Check out your working directory to make sure the CSV wrote out properly, and
that you can open it! If you want, try to bring it back into Python to make sure
it imports properly.

~~~
# For kicks read our output back into Python and make sure all looks good
new_output = pd.read_csv('data/out.csv', keep_default_na=False, na_values=[""])
~~~
{: .language-python}

> ## Challenge - Combine Data
>
> In the data folder, there are two lobster data files: `lobsters_2012.csv` and
> `lobsters_2013.csv`. Read the data into Python and combine the files to make one
> new data frame. Create a plot of average plot size by year grouped by site.
> Export your results as a CSV and make sure it reads back into Python properly.
{: .challenge}

# Joining DataFrames

When we concatenated our DataFrames we simply added them to each other -
stacking them either vertically or side by side. Another way to combine
DataFrames is to use columns in each dataset that contain common values (a
common unique id). Combining DataFrames using a common field is called
"joining". The columns containing the common values are called "join key(s)".
Joining DataFrames in this way is often useful when one DataFrame is a "lookup
table" containing additional data that we want to include in the other.

NOTE: This process of joining tables is similar to what we do with tables in an
SQL database.

Storing data in this way has many benefits including:

1. It ensures consistency in the spelling of different attributes (site) given each species is only     
   entered once.
2. It also makes it easy for us to make changes to a particular attribute information once,
   without having to find each instance of it in the larger data.
3. It optimizes the size of our data.


## Joining Two DataFrames

To better understand joins, let's import a subset of our data to work with.
~~~
position_cols = pd.read_csv('data/position_cols.csv', keep_default_na=False, na_values=[""])
position_cols = position_cols[['record_id', 'site', 'transect', 'replicate']]

time_cols = pd.read_csv('data/time_cols.csv', keep_default_na=False, na_values=[""])
time_cols = time_cols[['record_id', 'year', 'month', 'date']]
~~~
{: .language-python}

## Identifying join keys

To identify appropriate join keys we first need to know which field(s) are
shared between the files (DataFrames). We might inspect both DataFrames to
identify these columns. If we are lucky, both DataFrames will have columns with
the same name that also contain the same data. If we are less lucky, we need to
identify a (differently-named) column in each DataFrame that contains the same
information.

~~~
>>> position_cols.columns

Index(['record_id', 'site', 'transect', 'replicate'], dtype='object')

>>> time_cols.columns

Index(['record_id', 'year', 'month', 'date'], dtype='object')
~~~
{: .language-python}

In our example, the join key is the column containing the two-letter species
identifier, which is called `record_id`.

Now that we know the fields with the common record ID attributes in each
DataFrame, we are almost ready to join our data. However, since there are
[different types of joins][join-types], we
also need to decide which type of join makes sense for our analysis.

## Inner joins

The most common type of join is called an _inner join_. An inner join combines
two DataFrames based on a join key and returns a new DataFrame that contains
**only** those rows that have matching values in *both* of the original
DataFrames.

Inner joins yield a DataFrame that contains only rows where the value being
joined exists in BOTH tables. An example of an inner join, adapted from [Jeff Atwood's blogpost about SQL joins][join-types] is below:

![Inner join -- courtesy of codinghorror.com](../fig/inner-join.png)

The pandas function for performing joins is called `merge` and an Inner join is
the default option:

~~~
merged_inner = pd.merge(left=position_cols, right=time_cols, left_on='record_id', right_on='record_id')
# In this case `record_id` is the only column name in  both dataframes, so if we skipped `left_on`
# And `right_on` arguments we would still get the same result

# What's the size of the output data?
merged_inner.shape
merged_inner
~~~
{: .language-python}

~~~
record_id  site  transect replicate  year  month        date
0           1  IVEE         1         A  2012      8  2012-08-20
1           2  IVEE         5         B  2012      8  2012-08-20
2           3  IVEE         5         B  2012      8  2012-08-20
3           4  IVEE         5         B  2012      8  2012-08-20
4           5  IVEE         5         C  2012      8  2012-08-20
5           6  IVEE         5         D  2012      8  2012-08-20
6           7  IVEE         6         A  2012      8  2012-08-20
7           8  IVEE         6         B  2012      8  2012-08-20
8           9  IVEE         6         B  2012      8  2012-08-20
9          10  IVEE         6         C  2012      8  2012-08-20
10         11  IVEE         6         D  2012      8  2012-08-20
...
40         41  IVEE         4         D  2012      8  2012-08-20
41         42  IVEE         4         B  2012      8  2012-08-20
42         43  IVEE         4         C  2012      8  2012-08-20
43         44  IVEE         3         D  2012      8  2012-08-20
44         45  IVEE         3         C  2012      8  2012-08-20
45         46  IVEE         3         C  2012      8  2012-08-20
46         47  IVEE         3         C  2012      8  2012-08-20
47         48  IVEE         4         A  2012      8  2012-08-20
48         49  NAPL         6         D  2012      8  2012-08-22
49         50  NAPL         8         A  2012      8  2012-08-22
50         51  NAPL         7         A  2012      8  2012-08-22
~~~
{: .output}

The result of an inner join of `position_cols` and `time_cols` is a new DataFrame
that contains the combined set of columns from `position_cols` and `time_cols`. It
*only* contains rows that have record IDs that are the same in
both` DataFrames. In other words, if a row in
`position_cols` has a value of `record_id` that does *not* appear in the `record_id`
column of `time_cols`, it will not be included in the DataFrame returned by an
inner join.  Similarly, if a row in `time_cols` has a value of `record_id`
that does *not* appear in the `record_id` column of `position_cols`, that row will not
be included in the DataFrame returned by an inner join.

The two DataFrames that we want to join are passed to the `merge` function using
the `left` and `right` argument. The `left_on='species'` argument tells `merge`
to use the `record_id` column as the join key from `position_cols` (the `left`
DataFrame). Similarly , the `right_on='record_id'` argument tells `merge` to
use the `record_id` column as the join key from `position_cols` (the `right`
DataFrame). For inner joins, the order of the `left` and `right` arguments does
not matter.

The result `merged_inner` DataFrame contains all of the columns from `position_cols`
 as well as all the columns from `time_cols`.

## Left joins

Like an inner join, a left join uses join keys to combine two DataFrames. Unlike
an inner join, a left join will return *all* of the rows from the `left`
DataFrame, even those rows whose join key(s) do not have values in the `right`
DataFrame.  Rows in the `left` DataFrame that are missing values for the join
key(s) in the `right` DataFrame will simply have null (i.e., NaN or None) values
for those columns in the resulting joined DataFrame.

Note: a left join will still discard rows from the `right` DataFrame that do not
have values for the join key(s) in the `left` DataFrame.

![Left Join](../fig/left-join.png)

A left join is performed in pandas by calling the same `merge` function used for
inner join, but using the `how='left'` argument:

~~~
merged_left = pd.merge(left=position_cols, right=time_cols, how='left', left_on='record_id', right_on='record_id')
merged_left
~~~
{: .language-python}
~~~
record_id  site  transect replicate  year  month        date
0           1  IVEE         1         A  2012      8  2012-08-20
1           2  IVEE         5         B  2012      8  2012-08-20
2           3  IVEE         5         B  2012      8  2012-08-20
3           4  IVEE         5         B  2012      8  2012-08-20
4           5  IVEE         5         C  2012      8  2012-08-20
5           6  IVEE         5         D  2012      8  2012-08-20
6           7  IVEE         6         A  2012      8  2012-08-20
7           8  IVEE         6         B  2012      8  2012-08-20
8           9  IVEE         6         B  2012      8  2012-08-20
9          10  IVEE         6         C  2012      8  2012-08-20
10         11  IVEE         6         D  2012      8  2012-08-20
...
40         41  IVEE         4         D  2012      8  2012-08-20
41         42  IVEE         4         B  2012      8  2012-08-20
42         43  IVEE         4         C  2012      8  2012-08-20
43         44  IVEE         3         D  2012      8  2012-08-20
44         45  IVEE         3         C  2012      8  2012-08-20
45         46  IVEE         3         C  2012      8  2012-08-20
46         47  IVEE         3         C  2012      8  2012-08-20
47         48  IVEE         4         A  2012      8  2012-08-20
48         49  NAPL         6         D  2012      8  2012-08-22
49         50  NAPL         8         A  2012      8  2012-08-22
50         51  NAPL         7         A  2012      8  2012-08-22
~~~
{: .output}

## Other join types

The pandas `merge` function supports two other join types:

* Right (outer) join: Invoked by passing `how='right'` as an argument. Similar
  to a left join, except *all* rows from the `right` DataFrame are kept, while
  rows from the `left` DataFrame without matching join key(s) values are
  discarded.
* Full (outer) join: Invoked by passing `how='outer'` as an argument. This join
  type returns the all pairwise combinations of rows from both DataFrames; i.e.,
  the result DataFrame will `NaN` where data is missing in one of the dataframes. This join type is
  very rarely used.

# Final Challenge

> ## Challenge - Merging Comparisons
> Please implement left, right, and inner joins on dummy_df1 and dummy_df2. Try to answer the
> questions below by observing the resulting three dataframes.
>
> ~~~
> df_1 = {'site_rep': ['IVEE_A', 'NAPL_A', 'NAPL_B', 'AQUE_A', 'AQUE_B',
>                     'AQUE_C', 'CARP_A', 'CARP_B', 'MOHK_A', 'MOHK_B'],
>        'value': np.random.normal(0, 1, size= 10),
>        'diver': ['foo', float("nan"), 'foo', 'baz', 'bar',
>                  'baz', 'foo', float("nan"), 'baz', 'bar']}
>
> dummy_df1 = pd.DataFrame(df_1)
>
> df_2 = {'site_rep': ['IVEE_A', 'NAPL_A', 'NAPL_B', 'AQUE_A', 'AQUE_B',
>                     float("nan"), 'CARP_A', 'CARP_B', 'MOHK_A', 'MOHK_B'],
>        'transect': [1, 5, 6, 9, 7, 8, 4, 2, 3, float("nan")],
>        'protected': [True, False, False, True, False,
>                 True, True, True, False, False]}
> dummy_df2 = pd.DataFrame(df_2)
> ~~~
> {: .language-python}
>
> 1. Are all the dataframes the same dimensions?
> 2. What join do you think is best for these dataframes?
> 3. Another parameter that is available with the merge function is sort (e.g. sort = True, sort
> = False), what change do you observe 'sort = True' making?
{: .challenge}

[join-types]: http://blog.codinghorror.com/a-visual-explanation-of-sql-joins/

{% include links.md %}
