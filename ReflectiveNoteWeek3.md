# README — Critical Reflection on Query Strategy (Iteration 3)
## Overview

This document provides a critical reflection on the query strategy adopted in the third iteration of the optimisation process. The focus is on how modelling assumptions, uncertainty estimates, and decision rules evolved under limited data and high-dimensional black-box settings.

### 1. Changes in Query Strategy

In earlier iterations, the query strategy relied mainly on global random sampling combined with a basic UCB acquisition function. By the third iteration, the strategy became more targeted and diagnostic-driven.

Key changes include:

Explicit separation of signals
Regions of high model uncertainty were distinguished from regions with potentially high predicted values, rather than applying a single strategy uniformly.

Improved exploratory diagnostics
For several functions (e.g. Functions 3 and 7), additional checks were introduced before candidate selection, including input coverage inspection and simple correlation analysis. This revealed a limitation in earlier EDA, where input ranges were not sufficiently examined.

More flexible acquisition strategies
Alongside UCB, Thompson sampling was introduced, particularly for high-dimensional functions with small output variation, to prevent stagnation when UCB repeatedly selects similar candidates.

Conservative hyperparameter tuning
Hyperparameter adjustments were limited mainly to the exploration parameter κ. Aggressive tuning of kernels or model structures was avoided, as it can easily lead to overfitting in high-dimensional, low-sample regimes.

Overall, the third iteration relied more heavily on model outputs (mean and uncertainty) to guide principled exploration, while remaining exploration-focused due to limited information.

### 2. Balancing Exploration and Exploitation

The balance between exploration and exploitation was handled on a per-function basis, rather than through a single global rule.

High-dimensional functions with isolated extreme values (e.g. Function 7)
Global exploration was prioritised, as isolated high values in high-dimensional spaces may represent narrow regions or noise. A two-stage approach was adopted: global exploration by default, with local exploitation considered only after small-budget local probes verified reproducibility.

Functions with highly imbalanced output ranges (e.g. Function 5)
Extreme values were attractive for maximisation but destabilised surrogate modelling. A log transformation was applied to stabilise GP fitting, enabling cautious local refinement while avoiding domination by a single extreme observation.

Well-behaved but very high-dimensional functions (e.g. Function 8)
A conservative strategy was maintained, favouring model-guided global exploration due to insufficient evidence for reliable local refinement.

In summary, exploration remained dominant, and exploitation was introduced gradually only when supported by verifiable local structure rather than by the presence of a single maximum.

### 3. Role of SVM in This Setting

Support Vector Machines were not considered suitable as a primary optimisation tool in this setting.

The objective is to maximise a continuous response, and acquisition functions depend on both predictive mean and uncertainty.

Standard SVMs do not naturally provide calibrated uncertainty estimates.

Soft-margin SVMs require unstable thresholds in high-dimensional, low-sample regimes.

Kernel SVMs model non-linear boundaries but produce decision boundaries rather than ranked query priorities.

As a result, SVMs may serve as auxiliary analysis tools once more data is available, but GP and tree-based surrogates remain better suited for driving iterative query selection.

### 4. Emerging Limitations as Data Increases

As more data is collected, several limitations become clearer:

Overfitting risk
GP models frequently exhibit length-scales hitting bounds, often accompanied by convergence warnings, reflecting either strong smoothness assumptions or insufficient data coverage. Random Forests may also produce unstable splits and noisy uncertainty estimates when data is sparse.

Irrelevant or weakly informative dimensions
With more data, some dimensions are likely to show consistently low importance (RF) or large ARD length-scales (GP). Continued uniform exploration across these dimensions becomes inefficient.

Noise modelling assumptions
Current models often assume low noise levels. If the true objective is noisy or non-stationary, this can lead to overconfidence or excessive smoothing, directly affecting acquisition reliability.

### 5. Data-Scientific Thinking Under a Black-Box Setting

This black-box optimisation framework promotes disciplined decision-making under uncertainty and limited budgets.

Key lessons include:

Explicitly distinguishing between learning the objective structure and improving performance

Using uncertainty to manage risk rather than chasing isolated extreme values

Avoiding over-tuning in low-data regimes and prioritising robust decision frameworks over fragile optimisation

These principles transfer directly to real-world data science problems involving incomplete information and constrained experimentation.
