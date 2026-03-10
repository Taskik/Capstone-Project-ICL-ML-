# Capstone-Project-ICL-ML-

    BBO Capstone Project — Optimising Unknown Functions with Limited Information/Data Points

 **Project Overview**
This capstone project replicates a **real-world Bayesian optimisation challenge**, focusing on finding the maximum of **eight unknown black-box functions** under query constraints.  
Each function simulates a realistic optimisation scenario from **radiation detection** to **hyperparameter tuning** where evaluations are costly and information is limited.
The objective is to design, implement, and iteratively refine strategies that efficiently explore and exploit unknown functions to achieve near-optimal results.

This project demonstrates:
- Experimental design and intelligent search strategies  
- Bayesian optimisation and surrogate modelling  
- Exploration–exploitation trade-offs  
- Iterative model improvement and reflection
- Continuous Hyperparameter tuning depending on the phase of the project  

---

## Challenge Description

The challenge involves **eight synthetic black-box functions** of increasing dimensionality (2D–8D).  
Each function accepts a numeric input vector and returns a single scalar output.  
The task is to identify input combinations that **maximise the unknown function output** using limited queries.One function's maximum point is negated to transform this into a minimization problem.

| Function | Dimensionality | Task Description | Real-World Analogue |
|-----------|----------------|------------------|----------------------|
| 1 | 2D | Detect contamination sources based on sparse readings | Radiation detection |
| 2 | 2D | Optimise a noisy likelihood surface | Probabilistic model tuning |
| 3 | 3D | Combine chemical compounds with minimal side effects | Drug discovery |
| 4 | 4D | Tune hyperparameters for warehouse optimisation | Supply chain optimisation |
| 5 | 4D | Maximise chemical process yield | Industrial optimisation |
| 6 | 5D | Optimise a recipe’s balance of taste, cost, and waste | Product formulation |
| 7 | 6D | Tune ML model hyperparameters for best performance | Model selection |
| 8 | 8D | Optimise high-dimensional parameters with unknown interactions | Deep learning hyperparameter tuning |

All functions are **maximisation/minimization tasks**, and evaluations are limited — simulating real-world cost and delay constraints.

---

##  Inputs and  Outputs

### Inputs
- **Format:** N-dimensional numeric vectors (2D–8D depending on function).All query points must match the dimension of the features our function has.Every week we are returned the numerical output(y) of the function as a single scalar.  
- **Example:**  
  ```python
  x = [0.42, 0.77, 0.19, 0.63]  #4D Function
### Outputs
 ```python
      y = 0.234
 ```
### Project objectives:  
- Maximise the target function within a limited number of queries. (12 weeks,1 query per week)  
- Handle unknown function structure and noise,non convexity/non linearities.  
- Ensure computational efficiency, avoiding overly costly computations per query.  

**Constraints / Limitations:**  
- Maximum number of allowed queries per trial  
- Response delay between queries.  
- Unknown function landscape: may contain multiple local maxima/minima.  

---
## Technical Approach
**Strategies and methods:**
- Wrote a script to visualize and plot the initial data. Also wrote an external script for future use so I can keep appending the new data points each week and save them, since the initial data points came in a **.npy** format that cannot be directly altered like a **.csv** file.
- Initially used **random sampling** and basic grid search for exploration and baseline before designing the **Gaussian Process Regressor**.
- Later applied **Bayesian Optimization** to model the unknown function and guide query selection.
- Considered regression models: Gaussian Processes with various **kernels** and **acquisition functions** for surrogate modeling of the function.  
  - **Kernels used:**  
    - RBF (Radial Basis Function)  
    - Matern  
    - Rational Quadratic  
  - **Acquisition functions used:**  
    - EI (Expected Improvement)  
    - PI (Probability of Improvement)  
    - UCB / LCB (Upper / Lower Confidence Bound) 

**Exploration vs. exploitation:**  
- Early queries prioritize **exploration** to map the function space.Used mostly **UCB/LCB** while tuning their exploration hyperparameter **nu** (higher is more aggressive exploration)  
- Later queries will shift toward **exploitation**, focusing on promising regions suggested by the surrogate model.  
- Employed an acquisition function to balance the two: sometimes more exploratory (UCB/LCB) , sometimes more exploitative (PI), depending on confidence in the surrogate model and the structure of the underlying problem(Sometimes a suggested point fell outside of the physically possible bound region)

## Installation

To run this project locally, follow these steps:

1. **Clone the repository**:

```bash
git clone https://github.com/Taskik/Capstone-Project-ICL-ML-
```
2. **Navigate into the project folder**:
```bash
cd Capstone-Project-ICL-ML-
```
3. **Install the required packages**:
```bash
pip install -r requirements.txt
```
4. **Launch the Jupyter notebook**:
```bash
jupyter notebook
```
