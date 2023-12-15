# Record Linkage

## Context

Record linkage attempts to join data sources that have similar fuzzy duplicate values
(when values look similar but are not 100% identical), so that we end up with a final
DataFrame with no duplicates by using string similarity.

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
