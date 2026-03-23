# Black-Box Optimization (BBO) Capstone Project

## Overview

This repository contains my **Black-Box Optimization (BBO)** capstone project.

The goal of this project is to explore how to optimize unknown objective functions when:

- the internal structure is completely hidden,
- gradients are unavailable,
- and the number of allowed function evaluations is very limited.

This setting closely resembles many real-world machine learning and applied optimization problems, such as hyperparameter tuning, system optimization, and decision-making under uncertainty.

The core idea is to learn how to make effective decisions from **sparse and noisy observations**, gradually improving performance through adaptive querying strategies. This project serves as a practical starting point for connecting machine learning methods with real-world black-box scenarios, with room for continuous experimentation and model improvement.

---

## Inputs and Outputs

### Input

Each query to the black-box function is a real-valued vector.

- Dimensionality ranges from **2D to 8D**, depending on the function
- Each dimension is constrained to the interval **[0, 1]**

**Example input format:**
```text
f1: [0.500000, 0.500000]

f3: [0.200000, 0.600000, 0.800000]
```
### Output

Each query returns a **single scalar value**, representing the performance signal of the function at the queried point.

- Output values may be positive, negative, or close to zero
- The scale depends on the hidden function structure
- Noise may be present

**Example output format:**

```text
1088.8596181962705
```

## Optimization Objective

The objective of this project is to **maximize** the value of each black-box function.

Key constraints include:

- A very limited query budget
- No access to gradients or analytical forms
- Unknown noise levels and feature interactions

A central challenge is balancing **exploration** (sampling uncertain regions) and **exploitation** (focusing on promising regions).

---

## Methodology

### Current Strategy

The initial strategy differs based on **function dimensionality**:

- **Lower-dimensional functions**  
  Local exploitation is applied once promising regions emerge.

- **Higher-dimensional functions**  
  More conservative exploration is maintained, as single high-value points are less informative.

This strategy is **iterative and evolving**, and will be refined as more observations become available.

---

### Models and Techniques

Currently used approaches include:

- Simple surrogate models to approximate unknown functions
- Heuristic, data-driven sampling strategies
- Strategy adjustments based on dimensionality and observed responses

Potential future extensions include:

- More flexible regression models
- Bayesian optimization frameworks
- Uncertainty-aware acquisition functions

---

### Exploration vs. Exploitation

The balance between exploration and exploitation is adjusted dynamically based on:

- The number of collected samples
- Function dimensionality
- Stability of observed response values

Early stages and high-dimensional settings emphasize exploration, while exploitation is applied selectively when consistent high-performing regions are identified.

## Documentation

- [Datasheet](./datasheet.md)
- [Model Card](./model_card.md)


