# Dataset Datasheet  
## Black-Box Optimisation Capstone

---

## 1. Motivation

This dataset was created as part of a black-box optimisation (BBO) capstone project. The objective is to iteratively identify high-value inputs for a set of unknown functions under limited query budgets.

The dataset supports optimisation under uncertainty, where the underlying function is not accessible and only pointwise evaluations are available. It enables analysis of exploration–exploitation trade-offs and model-based decision making.

---

## 2. Composition

The dataset consists of query–response pairs collected over ten optimisation rounds across eight independent functions.

### Data structure
Each data point contains:
- **Input vector**: \( x \in [0,1]^d \)
- **Output value**: \( y = f(x) \)

### Dimensionality
- Function 1: 2D  
- Function 2: 2D  
- Function 3: 3D  
- Function 4: 4D  
- Function 5: 4D  
- Function 6: 5D  
- Function 7: 6D  
- Function 8: 8D  

### Size and format
- ~10 samples per function (one per iteration)  
- ~80 total data points  
- Stored as numerical arrays (NumPy / CSV)  
- All inputs are continuous within \([0,1]\)

### Gaps and biases
- Data is **not uniformly distributed**
- Sampling becomes increasingly concentrated around high-value regions
- Large areas remain unexplored, especially in higher dimensions

---

## 3. Collection Process

The dataset was generated through a sequential optimisation process over ten rounds.

At each iteration:
1. Fit a surrogate model (primarily Gaussian Process)
2. Generate candidate points (global sampling, local perturbation, or hybrid)
3. Apply an acquisition function (UCB)
4. Select one point and evaluate

### Strategy evolution
- Early rounds: global exploration  
- Mid rounds: mixed exploration and local refinement  
- Late rounds: targeted local search + structured global search  

---

## 4. Preprocessing and Uses

### Preprocessing
- Optional transformations (e.g. signed log scaling) applied in some cases
- Inputs normalised within \([0,1]\)

### Intended uses
- Studying black-box optimisation strategies  
- Evaluating model-based search methods  
- Analysing exploration vs exploitation  

### Inappropriate uses
- Not suitable for supervised learning benchmarks  
- Not a full representation of the function space  
- Not designed for predictive modelling  

---

## 5. Distribution and Maintenance

- Available in the project GitHub repository  
- Format: NumPy arrays / CSV  
- Maintained by the project author  
- No further updates expected  

---

## 6. Reflection and Assumptions

This dataset reflects sequential decision-making during optimisation, including:
- Model selection (GP / RF)
- Acquisition tuning (kappa)
- Switching between local and global strategies

### Key assumptions
- The function exhibits local smoothness  
- Nearby points provide useful information  
- Model uncertainty is informative  

These assumptions shape the sampling distribution and may bias the dataset toward specific regions.
