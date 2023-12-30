# Record Linkage

## Context

Record linkage attempts to join data sources that have similar fuzzy duplicate values
(when values look similar but are not 100% identical), so that we end up with a final
DataFrame with no duplicates by using string similarity.

Our flow would look something like this:

<img width="483" alt="image" src="https://github.com/Lumarstar/AISG_Foundations_In_AI/assets/63058663/1975e00e-5a9a-420d-aa5c-ea63e90a74df">

## Comparing Strings

### Minimum Edit Distance

Minimum edit distance is a systematic way to identify how close 2 strings are. For example,
let us compare "intention" and "execution".

```
  I N T E N T I O N
  E X E C U T I O N
```

The minimum edit distance between them is the *least possible amount of steps needed to
transition from one string to another*, in this case, from intention to execution, with the
following available operations:

- insertion
- deletion
- substitution
- transposition (changing characters' positions)

In our example, we first start by deleting 'I' from 'intention', and adding 'C' between 'E'
and 'N'.

```
  I N T E * N T I O N
  ▼       ▼
  * N T E C N T I O N
```

Our minimum edit distance so far is 2, since these are two operations.

We then substitute the first 'N' with E, 'T' with 'X', and 'N' with 'U', which completes our
transformation!

```
  I N T E * N T I O N
    ▼ ▼     ▼
  * E X E C U T I O N
```

It took us a total of 5 operations, thus the minimum edit distance is 5.

The lower the edit distance, the closer the two words are.

#### Minimum Edit Distance Algorithms

There is a variety of algorithms based on edit distance that differ on which operations they
use, how much weight is attributed to each operation, which type of strings they're suited
for and more, with a variety of packages to get each similarity. Possible packages include
`nltk`, `thefuzz`, and `textdistance` (and so on).

| Algorithm | Operations |
|---|---|
| Damerau-Levenshtein | insertion, substitution, deletion, transposition |
| Levenshtein | insertion, substitution, deletion |
| Hamming | substitution |
| Jaro distance | transposition |
| ... | ... |

*In this lecture, DataCamp opted to compare strings using the Levenshtein distance since it
is the most general form of string matching by using the `thefuzz` package.*

### Simple String Comparison (using `thefuzz`)

`thefuzz` is a package to perform string comparisons. We first import `fuzz` from `thefuzz`,
which allows us to compare between single strings.

```python
  from thefuzz import fuzz
```

We use the `fuzz`'s `WRatio` function to compute the similarity between reading and its typo,
inputting each string as an argument.

```python
  # compare "reeding" vs "reading"
  fuzz.WRatio('Reeding', 'Reading')
```

```console
  86
```

For any comparison function using `thefuzz`, our output is a score from 0 to 100, with 0
being not similar at all, and 100 being an exact match. *Do not confuse this with the
minimum edit distance score from earlier, where a lower minimum edit distance means a closer
match.*

#### Partial Strings and Different Orderings

The WRatio function is highly robust against partial string comparison with different
orderings. For example, here we compare the strings 'Houston Rockets' and 'Rockets'.

```python
  fuzz.WRatio('Houston Rockets', 'Rockets')
```

```console
  90
```

We still received a high similarity score!

The same can be said for the strings 'Houston Rockets vs Los Angeles Lakers' and 'Lakers vs
Rockets' - the team names are only partial and they are differently ordered.

```python
  fuzz.WRatio('Houston Rockets vs Los Angeles Lakers', 'Lakers vs Rockets')
```

```console
  86
```

#### Comparison with arrays

We can also compare a string with an array of strings by using the `extract()` function
from the `process` module from `thefuzz`. `extract()` takes in a string, an array of
strings, and the number of possible matches to return ranked from highest to lowest.

```python
  # import process
  from the fuzz import process

  # define string and array of possible matches
  string = 'Houston Rockets vs Los Angeles Lakers'
  choices = pd.Series(['Rockets vs Lakers', 'Lakers vs Rockets',
                        'Houston vs Los Angeles', 'Heat vs Bulls'])

  # use the extract() function
  process.extract(string, choices, limit=2)
```

The function returns a list of tuples with 3 elements - the first element being the
matching string being returned, the second one being its similarity score, and the third one
being its index in the array.

```console
  [('Rockets vs Lakers', 86, 0), ('Lakers vs Rockets', 86, 1)]
```

#### Collapsing categories with string similarity

Recall in "Text and Categorical Data Problems", we learnt that collapsing data into
categories is an essential aspect of working with categorical and text data. We used
`.replace()` method to manually replace categories in a column of a DataFrame! Making use
of string similarity, we can easily replace many inconsistent categories (at the same time)!

For example, say we have a DataFrame named `survey` containing results of a survey asking
residents from New York and California how likely they would move on a scale of 0 to 5.

```python
  print(survey['state'].unique())
```

```console
  id              state
  0          California
  1                Cali
  2          Calefornia
  3          Calefornie
  4          Californie
  5           Calfornia
  6          Calefernia
  7            New York
  8       New York City
 ...            ...
```

The `'state'` field was free text and contains *hundreds* of typos. Remapping them would
take a huge amount of time. Instead, we will use string similarity!

We also have a `categories` DataFrame containing the correct categories for each state.

```python
  categories
```

```console
     state
  0  California
  1  New York
```

We will collapse the incorrect categories with string matching.

First, we create a for loop iterating over each correctly typed state in `categories`. For
each state, we find its matches in the `'state'` column of `survey`, returning all possible
matches by setting the `limit` argument of `extract()` to the length of `survey`. 

We then iterate over each potential match, isolating the ones with a similarity score
higher than or equal to 80 with an if statement. Then for each of those returned strings,
we replace it with the correct state using the `.loc[]` method.

```python
  # for each correct category
  for state in categories['state']:
      # find potential matches in states with typoes
      # survey.shape[0] returns the length of survey
      matches = process.extract(state, survey['state'], limit=survey.shape[0])

      # check each potential match
      # all the rows are potential matches, that is why we need to filter out
      # similarity score >= 80
      for potential_match in matches:
          # if high similarity score
          if potential_match[1] >= 80:
              survey.loc[survey['state'] == potential_match[0], 'state'] = state
```

## Generating Pairs

### Motivation

Sometimes, we want to combine data from separate DataFrames that are complete duplicates of
each other but with different names. However, there may be data constraints, an absence of
a common unique identifier between the DataFrames, different names, etc. which means a
regular join or merge will not work in combining the two DataFrames.

This is where record linkage comes in.

### Record linkage

Record linkage is the act of linking data from different sources regarding the same entity.
Generally, we clean two or more DataFrames, generate pairs of potentially matching records,
score these pairs according to string similarity and other similarity metrics, and link
them.

All of these steps can be achieved with the `recordlinkage` package!

### Using `recordlinkage`

We will use an example to illustrate using this package.

We have two DataFrames, `census_A` and `census_B` containing data on individuals throughout
states in the United States of America. We want to merge them while avoiding duplication
using record linkage since they are collected manually and are prone to typos, and there
are no consistent IDs between them.

Both `census-A` and `census_B` have `rec_id`, `given_name`, `surname`, `date_of_birth`,
`suburb`, `state` and `address_1` as their columns.

#### Step 1 - Generating Pairs

We first want to *generate pairs* between both DataFrames. Ideally, we want to generate all
possible pairs between our DataFrames. But!! What if we had big DataFrames and ended up
having to generate millions, if not billions, of pairs? It would not prove scalable and
could seriously hamper development time.

This is where *blocking* comes in. Pairs are created based on a matching column. In our
example, that would be the `state` column, reducing the number of possible pairs.

To do this, we first start by importing `recordlinkage`. We then use the
`recordlinkage.Index()` function to create an indexing object. This essentially is an
object we can use to generate pairs from our DataFrames. To generate pairs blocked on
`state`, we use the `.block()` method, inputting `state` as the input. Once the indexer
object has been initialised, we generate our pairs using the `.index()` method, which takes
in two DataFrames.

```python
  # import recordlinkage
  import recordlinkage

  # create indexing object
  indexer = recordlinkage.Index()

  # generate pairs blocked on state
  indexer.block('state')
  pairs = indexer.index(census_A, census_B)
```

The resulting object is a pandas multiindex object containing pairs of row indices from both
DataFrames. (which is a fancy way to say it is an array containing possible pairs of indices
that makes it much easier to subset DataFrames on)

```python
  print(pairs)
```

```console
  MultiIndex(levels=[['rec-1007-org', 'rec-1016-org', 'rec-1054-org', 'rec-1066-org',
  'rec-1070-org', 'rec-1075-org', 'rec-1080-org', 'rec-110-org', 'rec-1146-org',
  'rec-1157-org', 'rec-1165-org', 'rec-1185-org', 'rec-1234-org', 'rec-1271-org', 'rec-1280-org', . . . .
  66, 14, 13, 18, 34, 39, 0, 16, 80, 50, 20, 69, 28, 25, 49, 77, 51, 85, 52, 63, 74, 61, 83, 91, 22, 26, 55, 84, 11, 81, 97, 56, 27, 48, 2, 64, 5, 17, 29, 60, 72, 47, 92, 12, 95, 15, 19, 57, 37, 70, 94]], names=['rec_id_1', 'rec_id_2'])
```

#### Step 2 - Compare the DataFrames

Since we have already generated our pairs, it is time to find potential matches. We first
start by creating a comparison object using the function `recordlinkage.compare()`. This
is similar to the indexing object we created while generating pairs, but this one is
responsible for assigning different comparison procedures for pairs.

Should we want exact matches between the pairs, we use the `.exact()` method of the Compare
object. It takes in the column name in question for each DataFrame and a `label` argument
which lets us set the column name in the resulting DataFrame.

Now, to compute string similarities (ie. not an exact match) between pairs of rows
for columns that have fuzzy values, we use the `.string()` method, which also takes in the
column names in question, the similarity cutoff point in the `threshold` argument which
takes in a value between 0 and 1, and a `label` argument.

Finally, to compute the matches, we use the `.compute()` function, which takes in the
possible pairs, and the two DataFrames in question. 

*Note that you need to always have the same order of DataFrames when inserting them as
arguments when generating pairs, comparing between columns and computing comparisons.*

```python
  # generate the pairs
  pairs = indexer.index(census_A, census_B)

  # create a Compare object
  compare_cl = recordlinkage.Compare()

  # find exact matches for pairs of date_of_birth and state
  compare_cl.exact('date_of_birth', 'date_of_birth', label='date_of_birth')
  compare_cl.exact('state', 'state', label='state')

  # find similar matches for pairs of surname and address_1 using string similarity
  compare_cl.string('surname', 'surname', threshold=0.85, label='surname')
  compare_cl.string('address_1', 'address_1', threshold=0.85, label='address_1')

  # find matches
  potential_matches = compare_cl.compute(pairs, census_A, census_B)
```

The output is a multiindex DataFrame, where the first index is the row index from the first
DataFrame (`census_A`), and the second index is a list of all row indices in `census_B`. The
columns are the columns being compared, with values being `1` for a match, and `0` for not a
match.

```python
  print(potential_matches)
```

```console
                                  date_of_birth state surname address_1
  rec_id_1     rec_id_2
  rec-1070-org rec-561-dup-0                  0     1     0.0       0.0
               rec-2642-dup-0                 0     1     0.0       0.0
               rec-608-dup-0                  0     1     0.0       0.0
  ...
  rec-1631-org rec-4070-dup-0                 0     1     0.0       0.0
               rec-4862-dup-0                 0     1     0.0       0.0
               rec-629-dup-0                  0     1     0.0       0.0
  ...
```

To find potential matches, we just filter for rows where the sum of row values is higher
than a certain threshold. In our context, that is higher or equal to 2.

```python
  potential_matches[potential_matches.sum(axis=1) >= 2]
```

```console
                              date_of_birth state surname address_1
rec_id_1     rec_id_2
rec-4878-org rec-4878-dup-0               1     1     1.0       0.0
rec-417-org  rec-2867-dup-0               0     1     0.0       1.0
rec-3964-org rec-394-dup-0                0     1     1.0       0.0
rec-1373-org rec-4051-dup-0               0     1     1.0       0.0
             rec-802-dup-0                0     1     1.0       0.0
rec-3540-org rec-470-dup-0                0     1     1.0       0.0
```

## Linking DataFrames

This is the last step in our workflow! We will be referring to the two DataFrames that we
used in the previous section, `census_A` and `census_B`.

We have already generated pairs between them, compared four of their columns (two for exact
matches and two for string similarity alongside a 0.85 threshold), and found potential
matches!

```python
  # import recordlinkage and generate full pairs
  import recordlinkage
  indexer = recordlinkage.Index()
  indexer.block('state')
  full_pairs = indexer.index(census_A, census_B)

  # comparison step
  compare_cl = recordlinkage.Compare()
  compare_cl.exact('date_of_birth', 'date_of_birth', label='date_of_birth')
  compare_cl.exact('state', 'state', label='state')
  compare_cl.string('surname', 'surname', threshold=0.85, label='surname')
  compare_cl.string('address_1', 'address_1', threshold=0.85, label='address_1')

  # find matches
  potential_matches = compare_cl.compute(full_pairs, census_A, census_B)
```

Our next step is to link the DataFrames.

First, let us look closely at our potential matches. It is a multiindex DataFrame, where we
have two index columns, `rec_id_1` and `rec_id_2`.

```console
                                  date_of_birth state surname address_1
  rec_id_1     rec_id_2
  rec-1070-org rec-561-dup-0                  0     1     0.0       0.0
               rec-2642-dup-0                 0     1     0.0       0.0
               rec-608-dup-0                  0     1     0.0       0.0
  ...
  rec-1631-org rec-4070-dup-0                 0     1     0.0       0.0
               rec-4862-dup-0                 0     1     0.0       0.0
               rec-629-dup-0                  0     1     0.0       0.0
  ...
```

Our first index column `rec_id_1` stores indices from `census_A`. The second index column
`rec_id_2` stores all possible indices from `census_B` for each row index of `census_A`.
The columns of our potential matches are the columns we chose to link both DataFrames on,
where the value is `1` for a match, and `0` otherwise.

The first step in linking DataFrames is to isolate the potentially matching pairs such that
we get pairs that we are (quite) sure are duplicates. We do this by subsetting the rows
where the row sum is above a certain number of columns.

```python
  # in our case, we take a row sum greater than or equal to 3
  matches = potential_matches[potential_matches.sum(axis=1) >= 3]
  print(matches)
```

The output is row indices between `census_A` and `census_B` that are most likely duplicates.

```console
  
```

Our next step is to extract one of the index columns and subset its associated DataFrame
to filter for duplicates.
