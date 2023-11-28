# Membership Constraints

## Categorical Data

### Context

Categorical data represents variables that represent a predefined finite set of categories.
Examples are marriage status (unmarried vs married) and loan statuses (defaulted, paid, etc.).
They are often coded as numbers for machine learning models to recognise them.

Since categorical data represents variables that represent predefined finite sets of categories,
they cannot have values that go beyond these predefined categories. (ie. they can **only**
take up these predefined values)

### Problems

We can have inconsistencies in categorical data for various reasons. Here are some potential
reasons:

- Data Entry Errors (Free text vs Dropdown)
- Parsing Errors

### Treatment

There are several approaches we can take, with increasingly specific solutions for different
types of inconsistencies.

1. Dropping data with incorrect categories
2. Remap incorrect categories to correct ones
3. Inferring categories based on data

#### Dropping Data - An Example
