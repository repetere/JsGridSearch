this is a version of JsGridSearch without colors and dependencies - https://github.com/bergloman/JsGridSearch

# JsGridSearch

Simple mechanism for performing grid-search in `node.js` -
parameter tuning for machine learning algorithms.

## Basic idea

Simple class that creates easy-to-use platform for performing parameter
tuning using [grid-search](https://en.wikipedia.org/wiki/Hyperparameter_optimization)
(an exhaustive search over parameter space).

When results are collected, it can output heat-map of the custom
result evaluation to console or plain string.

## Basic usage

### Run different parameters combinations

~~~~~~~~~~~~~~~javascript
let options = {
    params: {
        a: [1, 2], // all possible values for parameter a
        b: ["none", "tf"], // all possible values for parameter b
        c: [0, 100] // all possible values for parameter c
    },
    run_callback: (comb) => {
        // comb parameter contains one of the parameter combinations
        // e.g. { a: 2, b: "tf", c: 0 }

        // here one would run his algorithm (using comb values)
        // and collect the result

        // return the result - shape and content don't matter
        return { 
            some_metric: Math.random(),
            some_other_metric: Math.random()
        };
    }
};
let grid_search = new gs.GridSearch(options);
grid_search.run();
~~~~~~~~~~~~~~~

### Display the result

Call methods `displayTableOfResults` or `getTableOfResults` to 
display heat-map of collected data.

User needs to provide callback that evaluates each result - e.g. from
the same set of results one can display heat-map for accuracy, recall, precision or F1 measure.

~~~~~~~~~~~~~~~~~~~javascript
grid_search.displayTableOfResults(
    ["a"], // columns
    ["b", "c"], // rows
    x => +(x.results.some_metric.toFixed(3)) // evaluation function
);
~~~~~~~~~~~~~~~~~~~

The result would look something like this:

~~~~~~~~~~~~~~~~~~~~
|                 | a=1     | a=2
|-----------------|---------|---------
| b=none,c=0      | 0.009   | 0.501
| b=none,c=100    | 0.872   | 0.088
| b=tf,c=0        | 0.3     | 0.733
| b=tf,c=100      | 0.672   | 0.663
~~~~~~~~~~~~~~~~~~~~

This is easily copy-paste-able into `markdown`:

|                 | a=1     | a=2
|-----------------|---------|---------
| b=none,c=0      | 0.009   | 0.501
| b=none,c=100    | 0.872   | 0.088
| b=tf,c=0        | 0.3     | 0.733
| b=tf,c=100      | 0.672   | 0.663

