# Record Linkage

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
use, 
