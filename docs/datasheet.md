## Motivation

The dataset is my entry to the 2026 Black Box Optimization challenge as part of the Imperial College Professional Certificate in Machine Learning and Artificial Intelligence course. This challenge was the Capstone project element of the course and it counted for 35% of the total course grade.
The dataset was created to evaluate the effectiveness of Bayesian Optimisation strategies for locating near-optimal solutions to expensive black-box functions under a limited evaluation budget.


## Composition

The dataset contains evaluations of 8 different functions. The initial evaluations for each function were provided to all participants as part of the project initiation, the subsequent choices of which points to evaluate were chosen by each participant independently. Evaluations were conducted on a weekly cycle between April and July 2026, with no gaps or missing entries.

| Function | Input dimensions | Input range | Objective | Initial points |
|----------|------------------|-------------|-----------|----------------|
| 1        | 2                | [0,1]       | Maximise  | 10             |
| 2        | 2                | [0,1]       | Maximise  | 10             |
| 3        | 3                | [0,1]       | Maximise  | 15             |
| 4        | 4                | [0,1]       | Maximise  | 30             |
| 5        | 4                | [0,1]       | Maximise  | 20             |
| 6        | 5                | [0,1]       | Maximise  | 20             |
| 7        | 6                | [0,1]       | Maximise  | 30             |
| 8        | 8                | [0,1]       | Maximise  | 40             |

Output values vary widely in scale and sign across functions; some functions (3, 6) have entirely negative ranges, others (1, 5) span several orders of magnitude. 
The files are all in csv format, with one row per evaluation and inputs and outputs being saved in separate files. All values are rounded to 6 decimal places and multi-dimensional inputs are displayed with a “-“ separating the different dimensions.
e.g. 0.002171-0.162139-0.092350-0.212842-0.347231-0.390262-0.052474-0.526157

## Collection process

The queries were generated iteratively using a Bayesian Optimization pipeline. A Gaussian process was fitted to the dataset for each function, and then a UCB acquisition was used to select the next query point. (The exception was function 1 where the Bayesian optimization was paused and replaced with a Sobol  maximin coverage search due to challenges fitting the function)
The collection process ran from April to July 2026, concluding after 13 weekly rounds.
The exact details of the fitting approach evolved round by round. The main summary document in GitHub details the changes made to each function’s strategy.
The overall objective of the exercise was to determine the maximum of each function, so the sampling approach was designed around this objective, resulting in data clustering near likely maxima

## Preprocessing

As the rounds evolved more sophisticated transformations were applied to the data as part of the ingesting it to the Bayesian Optimization pipeline.  The transformations were applied selectively to each function, with the aim of ensuring each function had a mean close to zero and a sensible spread of values.
Transformations that were used include:
Standardise: Subtract mean and divide by standard deviation of the population
Log: Take logs of data before standardising
Yeo-Johnson: Power transform that attempts to make a dataset closer to normal distribution.
The round by round dataset describes the exact transformations adopted for each function for that round.

## Uses

### Intended use
The data in this repository is intended for use in benchmarking Bayesian Optimisation strategies on black-box functions.

### Inappropriate uses 
The evaluations are function-specific and are not applicable in other contexts or situations. The data was sampled with the objective of determining the maximum value of each function. For this reason the data should not be taken as representative of the function as a whole, and should not be used for any other metrics or inference on the function’s behaviour.

### Distribution and maintenance: 
The official dataset was archived in the capstone portal via online submissions and email notification of results in txt format. As the capstone portal location is password protected, I have replicated the dataset here in this GitHub repository. This dataset is maintained by me (Mark Jones) and will not be updated after the project concludes in July 2026. This GitHub repository serves as the permanent archive.
