# Model Card  
## Bayesian Optimisation Strategy for Black-Box Optimisation Capstone

---

## 1. Overview

**Model name:** Adaptive Bayesian Optimisation Strategy  
**Type:** Sequential model-based optimisation  
**Version:** v1.0  

This approach combines Gaussian Process (GP) surrogate modelling with an Upper Confidence Bound (UCB) acquisition function. The strategy dynamically adjusts between global exploration and local refinement across ten optimisation rounds.

---

## 2. Intended Use

### Suitable use cases
- Black-box optimisation problems with limited query budgets  
- Continuous search spaces (e.g. \( x \in [0,1]^d \))  
- Problems where function evaluations are expensive  

### Not suitable for
- Highly discontinuous or chaotic functions  
- Extremely high-dimensional problems with very sparse sampling  
- Scenarios requiring guaranteed global optimality  

---

## 3. Method Details

The optimisation was performed over ten sequential rounds across eight functions with varying dimensionality.

### Core components
- **Surrogate model:** Gaussian Process (primary), occasionally Random Forest  
- **Acquisition function:** Upper Confidence Bound (UCB)  
- **Candidate generation:**  
  - Global random sampling  
  - Local perturbation around best point  
  - Hybrid strategies  

---

### Strategy evolution

#### Early stage (Rounds 1–3)
- Focus on **global exploration**
- Large candidate sets sampled uniformly
- Higher kappa values to prioritise uncertainty

#### Mid stage (Rounds 4–7)
- Shift to **mixed strategy**
- Local refinement near high-performing regions
- Occasional return to global search if improvement stagnates

#### Late stage (Rounds 8–10)
- Function-specific strategies applied:
  - Some functions used **fine-grained local search**
  - Others reverted to **global exploration** when local search failed
- Distance-based filtering introduced to avoid redundant sampling
- Increased candidate pool size and exploration strength in difficult cases

---

## 4. Performance

Performance was evaluated based on the **maximum observed objective value** for each function.

### Metrics used
- Best observed \( y \) value  
- Improvement over iterations  
- Stability of convergence  

### Observations
- Lower-dimensional functions showed faster convergence  
- Higher-dimensional functions required more global exploration  
- Some functions exhibited sharp local optima, making local refinement less effective  

---

## 5. Assumptions and Limitations

### Key assumptions
- The objective function has some degree of **local smoothness**  
- Nearby points provide informative signals  
- Model uncertainty estimates are meaningful for guiding exploration  

### Limitations
- Performance is sensitive to **kappa tuning**  
- GP models may **oversmooth sharp peaks**  
- High-dimensional spaces remain **sparsely explored**  
- Strategy decisions rely on **heuristics rather than optimal rules**

---

## 6. Ethical Considerations

Transparency in the optimisation process improves:
- **Reproducibility** — others can follow and replicate the strategy  
- **Interpretability** — decisions are explainable (e.g. why switching strategies)  
- **Adaptability** — the approach can be transferred to real-world optimisation tasks  

Clear documentation of assumptions and limitations helps prevent misuse and overconfidence in results.

---

## 7. Reflection

The approach makes decisions based on:
- Current best observed value  
- Model predictions (mean + uncertainty)  
- Recent improvement trends  

### Strengths
- Flexible adaptation across functions  
- Effective balance of exploration and exploitation  
- Ability to recover from local optima via global search  

### Limitations
- Heavily dependent on manual parameter tuning  
- Strategy transitions are heuristic  
- Limited coverage in high-dimensional spaces  

Additional detail (e.g. per-round logs or parameter tracking) could further improve reproducibility, but the current structure captures the key logic and decisions of the optimisation process.
