## Overview

### Model name:
Capstone Black Box Optimisation Pipeline

### Version: 
Final Project Submission (July 2026) – v1.0

### Model type: 
Sequential black-box optimisation algorithm using Gaussian Process surrogate modelling and acquisition-function-based query selection.
This optimisation approach was developed for the 2026 Black Box Optimisation (BBO) Challenge as part of the Imperial College Professional Certificate in Machine Learning and Artificial Intelligence. The objective was to identify high-performing regions of eight unknown objective functions using a limited budget of evaluations.




## Intended use 

### Appropriate Uses
This approach is intended for:
•	Expensive black-box optimisation problems.
•	Problems where objective function evaluations are costly.
•	Continuous optimisation tasks with limited evaluation budgets.
•	Sequential experimentation where new observations can be incorporated iteratively.
Examples given in the project background documentation include radiation detection, hyperparameter tuning, and scientific drug experimentation.

### Inappropriate Uses
This approach should not be used for:
•	High-dimensional optimisation problems beyond the practical limits of Gaussian Process modelling.
•	Situations requiring immediate real-time decisions.
•	Problems where objective functions are highly discontinuous or noisy.
•	Applications requiring guarantees of finding a global optimum.



## Details 

The main stages of the process were:
1) Collection of latest evaluations from capstone portal
2) Transformation applied to data to improve model fit
3) Fit data to Gaussian Process surrogate model, using appropriate kernel
4) Apply an acquisition function, tuned to apply an appropriate balance between exploration and exploitation
5) Evaluate this acquisition function on a grid of points covering the search space. The grid was generated using Latin Hypercube sampling to effectively cover the search space.
6) Select the highest scoring point as the next evaluation point and submit in the Capstone portal.

The evolution of the strategy was: 

### Early Rounds
-Standard Gaussian Process surrogate modelling using rbf kernel
-Basic acquisition function tuning. Biased to exploration (ucb with beta parameter = 2.5)
-Limited preprocessing.
-Latin Hypercube for grid coverage

### Intermediate Rounds
-	Alternative kernel approaches introduced
-	Additional transformation approaches
-	Reduction in ucb beta parameter in acquisition function to begin pivot to exploitation-based search.
-	Introduction of “exclusion radius” parameter to ensure that optimization doesn’t get stuck evaluating points in the same area of the grid

### Later Rounds
-	Multiple kernel approach introduced for some functions
-	Additional filtering of the search space using Support Vector machine model
-	“Turbo” approach introduced for function 8
-	Function 1 removed from Bayesian optimization process in favour of pure coverage-based search strategy, pending finding a plausible maximum region.
-	Further reduction in ucb beta parameter to continue pivot to exploitation.



## Performance 
Performance was evaluated using the objective values obtained for each of the eight black-box functions. The main metric tracked was best objective value discovered.
Progress was intentionally nonlinear, it was necessary in earlier rounds to evaluate points in the function space that were unlikely to be  optimal, but would assist in building an overall picture of the function behaviour that could be refined in later rounds.

Function 1  —  0 new max(es) across 11 rounds:
   (no round beat the initial-data best)

Function 2  —  3 new max(es) across 11 rounds:
   R4   Y=0.62052      gain=+0.00931   (0.6994, 0.9696)
   R5   Y=0.67521      gain=+0.05469   (0.7030, 0.1741)
   R9   Y=0.72173      gain=+0.04652   (0.6863, 0.8694)

Function 3  —  2 new max(es) across 11 rounds:
   R1   Y=-0.022984    gain=+0.01185   (0.4000, 0.4000, 0.5000)
   R11  Y=-0.017382    gain=+0.005602   (0.6767, 0.6985, 0.3814)

Function 4  —  3 new max(es) across 11 rounds:
   R1   Y=-0.38486     gain=+3.641   (0.4444, 0.4444, 0.3333, 0.4444)
   R2   Y=-0.036574    gain=+0.3483   (0.3716, 0.4437, 0.3910, 0.4416)
   R5   Y=0.30404      gain=+0.3406   (0.3450, 0.4428, 0.4091, 0.4115)

Function 5  —  4 new max(es) across 11 rounds:
   R1   Y=1650.7       gain=+561.9   (0.3333, 0.7778, 1.0000, 0.8889)
   R2   Y=1834.1       gain=+183.4   (0.4772, 0.5845, 0.9807, 0.9988)
   R7   Y=4069.3       gain=+2235   (0.4221, 0.9881, 0.9970, 0.9778)
   R11  Y=6281.5       gain=+2212   (0.9973, 0.9906, 0.8819, 0.9775)

Function 6  —  2 new max(es) across 11 rounds:
   R2   Y=-0.29023     gain=+0.424   (0.4243, 0.2976, 0.5111, 0.7324, 0.1505)
   R10  Y=-0.18152     gain=+0.1087   (0.4847, 0.4425, 0.6344, 0.7815, 0.0152)

Function 7  —  3 new max(es) across 11 rounds:
   R1   Y=2.317        gain=+0.952   (0.0000, 0.3333, 0.3333, 0.3333, 0.3333, 0.6667)
   R2   Y=2.5441       gain=+0.2271   (0.0052, 0.2594, 0.5189, 0.3146, 0.2761, 0.7362)
   R3   Y=2.9735       gain=+0.4294   (0.1612, 0.2864, 0.4012, 0.2899, 0.2975, 0.6096)

Function 8  —  5 new max(es) across 11 rounds:
   R1   Y=9.7833       gain=+0.1848   (0.0000, 0.0000, 0.0000, 0.0000, 1.0000, 0.5000, 0.0000, 0.5000)
   R6   Y=9.8194       gain=+0.03611   (0.0436, 0.1733, 0.1067, 0.2017, 0.3415, 0.3491, 0.0560, 0.5739)
   R8   Y=9.8345       gain=+0.01513   (0.0223, 0.1696, 0.1212, 0.2093, 0.3362, 0.3796, 0.0837, 0.5573)
   R10  Y=9.839        gain=+0.004444   (0.0463, 0.1498, 0.0906, 0.1846, 0.3110, 0.3789, 0.1129, 0.5878)
   R11  Y=9.8653       gain=+0.02629   (0.0654, 0.1249, 0.0987, 0.1946, 0.3404, 0.3912, 0.1324, 0.5571)


Gains are presented in original output units (not transformed values)
Function 1 is the only one where the process has so far failed to identify a new maximum. 



## Assumptions and limitations 

### Assumptions
The optimisation strategy assumes:
•	The objective function is sufficiently smooth and lacking discontinuities.
•	The objective function can be reasonably approximated by a Gaussian Process.
•	Historical evaluations provide useful information about unexplored regions.
•	Data transformations can be applied appropriately to improve surrogate model quality.

### Limitations
Several limitations were identified:
•	Gaussian Processes scale poorly with increasing dimensionality and dataset size.
•	Performance depends heavily on kernel selection and hyperparameter tuning.
•	Acquisition functions may become trapped in local optima, or
•	Acquisition functions may fail to distinguish between valuable and non-valuable areas if the noise parameter collapses to zero, meaning that search is random rather than driven by exploitation/exploration criteria.
•	Transformations are function specific. A transformation that improves performance on one function may degrade performance on another.
•	The approach is sensitive to early observations because initial samples influence subsequent exploration.

### Failure Modes
Potential failure modes include:
•	Over-exploitation of regions incorrectly predicted to be promising.
•	Poor uncertainty estimates leading to inadequate exploration.
•	Poor fitting of the surrogate model.
•	Reduced effectiveness on highly irregular or discontinuous functions.
•	Insufficient number of iterations available to converge on an optimal value



## Ethical considerations 
Although developed for a benchmark optimisation challenge, transparency remains important for reproducibility and responsible deployment.
Documenting the optimisation process enables:
•	Reproduction of results by other researchers.
•	Independent evaluation of methodological choices.
•	Identification of assumptions and limitations.
•	Adaptation of the approach to other optimisation domains.

This is particularly important for this project where the strategy, code and approaches have evolved significantly as the rounds progressed.



## Reflection

The approach is a combination of formal optimization techniques such as Bayesian optimization, augmented by human judgement to identify edge cases or areas where the standard approach needs to be refined.
The documentation structure provides high level details of the approach followed. More detail is available in the round by round logs saved at https://github.com/marktjones1/Imperial_College_Capstone/new/main
