# Imperial College Capstone
Capstone Project for Imperial College Machine Learning Course

## Summary
This project tackled eight unknown "black box" functions, where the only way to learn about each was to evaluate it at chosen input points — but we were limited to one evaluation per function per week, for thirteen weeks. The goal was to find the input settings that produced the highest output value for each function. By using Bayesian optimisation — a method that uses each result to decide intelligently where to look next — we improved seven of the eight functions over their starting values, in one case more than quadrupling the initial best. Every decision was documented so the process can be reviewed, reproduced or applied to similar problems.


## Section 1: Project Overview

The purpose of the project was to understand the nature of 8 different “black box” functions. The exact nature of each function was unknown, the only starting information given was the output value of the function for a small set of inputs. Over the course of 13 weeks, more observations of each function were collected, with the ultimate objective of learning an approximation of each function sufficient to identify high-performing input regions under a strict query budget. The main task was to determine the set of input values that maximises the output value for each function. However, it was also important to demonstrate a robust and methodical approach for tackling the problem and to show understanding of ML techniques. This was a black-box optimization problem.
This mirrors real-world scenarios such as hyperparameter tuning or experimental optimisation, where evaluations are costly and the underlying system is not directly observable.

## Section 2: Inputs and Outputs
Each function maps an n-dimensional input vector to a scalar output.
The starting dataset is a set of inputs and outputs for each function.
Each week one new input vector may be requested for each function, producing one output value which is appended to the set of observations.

| Function | Initial data (shape) | Dimensions | Output | Optimisation goal |
|----------|----------------------|------------|--------|-------------------|
| Function 1 | (10, 2) | 2 | Scalar | Maximise |
| Function 2 | (10, 2) | 2 | Scalar | Maximise |
| Function 3 | (15, 3) | 3 | Scalar | Maximise |
| Function 4 | (30, 4) | 4 | Scalar | Maximise |
| Function 5 | (20, 4) | 4 | Scalar | Maximise |
| Function 6 | (20, 5) | 5 | Scalar | Maximise |
| Function 7 | (30, 6) | 6 | Scalar | Maximise |
| Function 8 | (40, 8) | 8 | Scalar | Maximise |


Each input must be a value in the range [0,1] (inclusive). The range of values for the output is not specified.
All inputs are uploaded to the capstone portal in the following format:
•	x1-x2-x3-...-xn, where each xᵢ is in [0,1], specified to six decimal places (e.g. 0.123456).
The corresponding values are returned up to 24 hours later via email notification as scalar values in csv format.
Example input (Function 2):
0.123456-0.654321

Example output:
0.847200

## Section 3: Challenge Objectives
The main goal is to maximise each function. Only a total of 13 queries are permitted, one per week, with a delay of up to 24 hours between submitting the inputs and receiving the new output values for that week.
The key challenge is maximising performance under a strict query budget, requiring each query to provide maximum information gain.


## Section 4: Technical Approach

The primary method is Bayesian optimisation, with the following components:

- **Transformation:** function outputs were transformed to improve the surrogate fit. Transformations varied by round and function — standardisation, log, and Yeo–Johnson power transform.
- **Gaussian Process:** the kernels and lengthscale parameterisations varied by round and function (ARD Matérn and RBF kernels with a learnable noise term).
- **Acquisition function:** primarily UCB, initially with high β (2.5) reducing gradually across rounds to 1 to shift from exploration to exploitation.
- **Multi-kernel selection:** for some functions, several kernels (Matérn-1.5, Matérn-2.5, RBF) were fitted and combined via Thompson sampling, hedging against the wrong smoothness assumption.
- **SVM filter:** an SVM classifier identifying high-value regions was added as a filter on the search space.

Functions 1 and 8 use bespoke processes:
- **Function 1:** Bayesian optimisation was replaced with space-filling coverage sampling, as the sparse-signal data could not be fitted by a Gaussian process.
- **Function 8:** a trust-region approach (TuRBO) was implemented for this higher-dimensional function.

More details are available in the model card and the week by week submission details.

## Documentation 
- [Datasheet](docs/datasheet.md) — dataset composition, collection, and intended uses 
- [Model card](docs/model_card.md) — the optimisation approach in detail
- [References](docs/references.md) — academic papers referenced in the project
- [Pipeline notebook](notebooks/Capstone_BBO.ipynb) — the working BBO pipeline code

## How to run

The full pipeline is in `notebooks/Capstone_BBO.ipynb`. To reproduce:
1. Clone the repository
2. Install dependencies: `pip install -r requirements.txt`
3. Open the notebook from the `notebooks/` folder (Jupyter must be launched from there for the relative paths to resolve)
4. Run cells in order

## Environment

Developed with Python 3.13.9. Install dependencies with:

```
pip install -r requirements.txt
```
