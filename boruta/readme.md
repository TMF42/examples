# Boruta algorithm

In this package you'll find a script implementing feature selection using
a version of the
[Boruta algorithm](https://www.jstatsoft.org/article/view/v036i11/v36i11.pdf)
to detect important and unimportant fields in your dataset. The algorithm:

- Uploads a data souce from its remote location
- Creates the corresponding dataset
- Creates a new extended dataset. In the new dataset, each field has a
  corresponding shadow field which has the same type but contains a random
  sample of the values contained in the original one.
- Creates a random forest from the extended dataset
- Extracts the maximum of the importances for the shadow fields
- Uses this maximum plus (minus) a minim gain as threshold. Any of the original
  fields scoring less than the minimal threshold are considered unimportant
  and fields scoring more than the maximum threshold are considered important.
- Fields marked as unimportant are removed from the list of fields to be used
  as input fields for new datasets
- The procedure is repeated, and a new extended dataset is created with
  the remaining fields. The process stops when it reaches the user-given number
  of runs or when all the original fields in the dataset are marked as
  important or unimportant.

The **inputs** for the script are:

* `dataset-id`: (dataset-id) Dataset ID for the original data
* `objective`: (string) Name of the objective field
* `max-runs`: (integer) Maximum number of runs
* `min-gain`: (number) Minimum amount of gain (real in the [0, 1] inteval).
  Defines a neighbourhood
  around the maximum importance of shadow fields. A field whose importance
  falls in this interval will be considered "neutral" (neither important or
  unimportant)

# Using the boruta script

One example using the ``iris.csv`` example would be

```
(boruta "dataset/577ef0da7e0a8d4c6900c581"
        "species"
        2
        0.1)
```

The **output** of the script is stored in `importance-output` as a map.
The map will have
three keys: "important", "unimportant" and "neutral" whose associated values
will be the list of fields classified as such. The objective
field is added to the important set of fields.