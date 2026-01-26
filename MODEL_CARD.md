# Model Card: Bayesian Black-Box Optimisation Strategy

## Model Overview
Model name: Adaptive Bayesian Black-Box Optimisation GPR
Type: Bayesian optimisation with Gaussian Process Regressor using a surrogate model.
Task: Sequential optimisation of expensive black-box functions  
Deployment: Coursework experimentation  
Version: v1.0  
Developer: Taskik  
License: None

## Intended Use
Primary tasks:
- Optimisation of synthetic benchmark functions
- Evaluation of Bayesian optimisation strategies

Use cases:
- Capstone project experimentation
- Algorithm benchmarking
- Educational demonstrations

Limitations:
- Should be used as a decision-support tool only,real world functions are way more complicated.

## Training Data

Dataset:
The model was trained and evaluated using a custom initially provided dataset followed by sequential function evaluations.

Link:

Size:
Approximately 10 evaluations across eight functions.

Features:
Continuous input variables and corresponding objective values.

## Inputs and Outputs
Inputs:
- Continuous parameter vectors representing candidate solutions.
- Function identifiers indicating the benchmark task.

Outputs:
- Scalar objective values returned by the black-box functions.
- Predicted mean and uncertainty estimates from the surrogate model.
  
## Optimisation Strategy


This Bayesian Black-Box Optimisation (BBO) approach uses Gaussian Process (GP) regression to model the objective function at each iteration and guide sequential query selection via acquisition functions. The workflow combines global and local search strategies, including a TuRBO-lite variant for adaptive local exploration which was introduced in the middle of the project.


### Initialisation
- Initial data points were loaded from pre-specified datasets for the target benchmark function.
- Inputs were normalised to [0,1] to improve GP stability.
- The initial best objective value (`y_best`) was determined from these data.

### Surrogate Modelling
- A Gaussian Process regressor was fitted to the observed inputs and outputs at each iteration.Half the queries were initially performed using a vanilla GP regressor using sci-kit learn with safe,generous hyperparameters for either noise modelling and exploration/exploitation parameters.
- Multiple kernels were tested, including:
  - RBF(Radial Basis Function,vanilla)
  - RBF with Automatic Relevance Determination (ARD)
  - Matern without ARD
  - Matern with ARD
  - Rational Quadratic
  - WhiteKernel for noise modelling
- Kernel hyperparameters (length-scales, noise) were optimised using maximum likelihood and `n_restarts_optimizer` to avoid local optima.
- ARD was used to adaptively weight dimensions of the input space based on their relevance.(Non-constant length_scale across dimensions)

### Acquisition Functions
- Multiple acquisition functions were employed to balance exploration and exploitation:
  - Expected Improvement (EI)
  - Probability of Improvement (PI)
  - Upper Confidence Bound (UCB)/Lower Confidence Bound(LCB)
- Each function was evaluated both globally and locally within a bounded region around the current best solution.Each function was modeled as python function according to their statistical definition.

### Acquisition Optimisation
- Acquisition functions were maximised using L-BFGS-B with multiple random restarts, inclusion of existing data points, and corner points to ensure broad coverage.
- Local optimisation bounds were adaptively defined using the TuRBO radius, which shrinks or expands based on recent successes and failures.

### TuRBOLocal Strategy
- In addition to global acquisition optimisation, TuRBO was applied to focus search around promising regions.
- The local search radius is dynamically adjusted:
  - Expanded after consecutive successes
  - Shrunk after consecutive failures
- This allows efficient exploitation of local optima without sacrificing exploration.

### Candidate Selection
- Multiple candidate points were generated from different acquisition functions.
- The candidate with the highest predicted improvement or probability of improvement was chosen for evaluation.
- Predicted mean and uncertainty estimates from the GP were taken into account for the next query.


## Performance & Monitoring
Evaluation Metrics

Predicted mean (μ) and uncertainty (σ) at suggested candidate points

Comparison across acquisition functions: EI, PI, and UCB values for each candidate

Ethical Considerations

Transparency: All kernel choices, acquisition functions, and predictions are documented per function

Responsible Use: Users should combine GP suggestions with domain knowledge; avoid using blindly for critical decisions

Code, initial datasets, and candidate prediction scripts are available on GitHub for Functions 1–8


