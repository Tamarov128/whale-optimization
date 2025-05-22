# Whale Search Optimization

## Problem
The optimization problem we choose to solve using the Whale Search Optimization algorithm consists in **function minimization**.

## Functions analyzed
- Schwefel's 2.21 Function (F4 & F4)  
    
  <img 
      style="display: block; 
            margin-left: auto;
            margin-right: auto;
            width: 70%;"
      src="https://al-roomi.org/multimedia/Unconstrained_Benchmarks/ndimensional/SchwefelFunction221/3D_SchwefelFunction221.jpg" 
      alt="Schwefel">
  </img>
    
  <img 
      style="display: block; 
            margin-left: auto;
            margin-right: auto;
            width: 40%;"
      src="static\Schwefel_2.21.svg" 
      alt="Schwefel">
  </img>
  
- Ackley's function (F10 & F7)  
    
  <img 
      style="display: block; 
            margin-left: auto;
            margin-right: auto;
            width: 70%;"
      src="https://www.sfu.ca/~ssurjano/ackley.png" 
      alt="Ackley">
  </img>
    
  <img 
      style="display: block; 
            margin-left: auto;
            margin-right: auto;
            width:70%;"
      src="static\Ackley.svg" 
      alt="Schwefel">
  </img>

- Griewank's Function (F11 & F9)  
    
  <img 
      style="display: block; 
            margin-left: auto;
            margin-right: auto;
            width: 70%;"
      src="https://al-roomi.org/multimedia/Unconstrained_Benchmarks/ndimensional/GeneralizedGriewankFunction/3D_GeneralizedGriewankFunction.jpg" 
      alt="Griewank">
  </img>
    
  <img 
      style="display: block; 
            margin-left: auto;
            margin-right: auto;
            width: 70%;"
      src="https://www.sfu.ca/~ssurjano/griewank.png" 
      alt="Griewank">
  </img>
    
  <img 
      style="display: block; 
            margin-left: auto;
            margin-right: auto;
            width:50%;"
      src="static\Griewank.svg" 
      alt="Schwefel">
  </img>

Image sources: 
- Surjanovic, S. & Bingham, D. (2013). Virtual Library of Simulation Experiments: Test Functions and Datasets. http://www.sfu.ca/~ssurjano.
- Ali R. Al-Roomi (2015). Unconstrained Single-Objective Benchmark Functions Repository [https://www.al-roomi.org/benchmarks/unconstrained]. Halifax, Nova Scotia, Canada: Dalhousie University, Electrical and Computer Engineering.

## Whale Optimization Algorithm

Algorithm overview:
    
### Phase 1: Encircling Prey
Whales assume that the current best solution is the prey, and all other whales update their positions to move toward it.

Equations:
- Calculate distance to best solution: `D = |C * X_best - X|`
  
- Update position: `X(t + 1) = X_best - A * D`

Parameters:
- X = current whale position
- X_best = position of the best whale (best solution found so far)
- A and C = coefficient vectors that influence movement
- D = distance between current whale and best whale
- t = current iteration

How A and C are calculated: `A = 2 * a * r - a`, `C = 2 * r`

Where:
- a decreases from 2 → 0 over time (controls exploration vs exploitation)
- r is a random number in [0, 1]

### Phase 2: Bubble-Net Attacking Method (Exploitation Phase)
Whales use two behaviors to simulate hunting prey in water:

1. Shrinking Encircling Mechanism
  As a → 0, the whale gets closer to the prey.  
  Smaller |A| (< 1) pulls whale near X_best.

2. Spiral Updating Position  
  ```python
    X(t + 1) = D * e^(b * l) * cos(2πl) + X_best
  ```

Where:
- D = |X_best - X|
- b = constant defining spiral shape
- l = random number in [–1, 1]
- e^(...) * cos(...) = models the spiral movement of whales around prey

Combined Model (probabilistic behavior)

- The whale chooses either of the two methods with 50% probability:  
```python
  X(t + 1) = {
    X_best - A * D                        if p < 0.5
    D * e^(b * l) * cos(2πl) + X_best     if p ≥ 0.5
  }
```

Where p is a random number in [0, 1].

### Phase 3: Search for Prey (Exploration Phase)
When whales explore instead of exploit, they move relative to a random whale:

```python
  D = |C * X_rand - X|
  X(t + 1) = X_rand - A * D
```

Here:
- X_rand = position of a randomly selected whale
- Used when |A| > 1 to move whales away from the best and encourage exploration

```python
Initialize whale population
Evaluate fitness and get X_best

while (not max iterations):
    for each whale:
        Update a, A, C, l, p
        if p < 0.5:
            if |A| < 1:
                use encircling update (toward X_best)
            else:
                use exploration update (toward random X)
        else:
            use spiral update (around X_best)
        Correct position if out of bounds
    Evaluate fitness
    Update X_best if needed
```

##  Improved Whale Optimization Algorithm

### 1. *Why Improve WOA?*

The *Standard Whale Optimization Algorithm (WOA)* is inspired by how humpback whales hunt prey using bubble-net feeding. While effective, it suffers from:

* *Premature convergence*: it can get stuck in local optima.
* *Weak exploration*: it often fails to search new areas of the solution space effectively.

The *Improved WOA (IWOA)* was designed specifically to fix these issues.

### 2. *Key Differences Between IWOA and WOA*

### *Hybrid with Differential Evolution (DE)*

* *IWOA combines WOA with the mutation operator from Differential Evolution (DE)*.
* DE is strong at exploring the search space, so this combination enhances WOA’s exploration capabilities.
* Standard WOA does not use DE—it’s purely inspired by whale behavior.

### *Hybrid Search Operator*

* IWOA uses a *hybrid operator* that merges:

  * DE’s mutation
  * WOA’s encircling, prey-searching, and spiral behaviors
* It separates the algorithm into two distinct parts:

  * *Exploration part*: uses DE mutation + WOA's prey search
  * *Exploitation part*: uses WOA’s shrinking circle and spiral update

### **Adaptive Exploration–Exploitation Balance (with k)**

* IWOA introduces a new parameter k (defined in Eq. 8 in the paper).
* k starts at *1* and *gradually decreases to 0* over time.
* This lets the algorithm *explore more in early iterations* and *exploit more in later ones*.
* Standard WOA also switches between exploration/exploitation (using A and random p), but IWOA’s k-based control is more *systematic and adaptive*.

### *Elitism*

* IWOA is *elitist*, meaning it always keeps the best solution between:

  * the current whale position Xi, and
  * the new candidate position Ui from the DE-based mutation.
* In standard WOA, new positions replace the old ones directly without comparing fitness.

### *Selection Method*

* IWOA uses a *selection strategy similar to Differential Evolution*, helping it maintain diversity and choose better offspring.

### *Performance*

* In experiments, *IWOA outperforms WOA, especially on **complex, multimodal test functions* (those with many local optima).
* It finds better solutions but might take slightly more time to converge due to the deeper exploration.

## IWOA vs WOA - Summary table


| Feature                        | WOA (Standard)   | IWOA (Improved)                  |
| ------------------------------ | ---------------- | -------------------------------- |
| Exploration capability         | Weak             | Strong (via DE mutation)         |
| Premature convergence          | Possible         | Reduced                          |
| Hybrid with DE                 | No             | Yes                            |
| Adaptive balance control       | Basic (A, p) | Advanced (via decreasing k)    |
| Elitism                        | Not guaranteed | Always selects better solution |
| Selection method               | WOA-style        | DE-style                         |
| Performance on multimodal      | Moderate         | High                             |
| Spiral + encircling behavior   | Yes            | Yes                            |
| Random agent-based exploration | Yes            | Enhanced with DE mutation      |
  

Think of WOA as a group of whales hunting based on instinct. They might succeed, but they sometimes get confused or swim in circles.

Now imagine IWOA as whales that:

* Still use instinct,
* But also share smart strategies from other sea creatures (DE),
* Learn over time when to explore and when to focus,
* And always keep the best fish they catch.

## Papers Analyzed
1. Seyedali Mirjalili, Andrew Lewis, The Whale Optimization Algorithm, Advances in Engineering Software, Volume 95, 2016, Pages 51-67, ISSN 0965-9978, https://doi.org/10.1016/j.advengsoft.2016.01.008.
  
2. Xiang Wang, Liangsa Wang, Han Li, Yibin Guo, An Improved Whale Optimization Algorithm for Global Optimization and Realized Volatility Prediction, Computers, Materials and Continua, Volume 77, Issue 3, 2023, Pages 2935-2969, ISSN 1546-2218, https://doi.org/10.32604/cmc.2023.044948.

## Results for the selected functions
### 1. WOA - Seyedali Mirjalili, Andrew Lewis
WOA algorithm performance compared with PSO, GSA, DE.  
  
| F   | WOA ave     | WOA std   | PSO ave    | PSO std   | GSA ave    | GSA std   | DE ave     | DE std    |
|-----|-------------|-----------|------------|-----------|------------|-----------|------------|-----------|
| F4 - Schwefel 2.21 | 0.072581    | 0.39747   | 1.086481   | 0.317039  | 7.35487    | 1.741452  | 0          | 0         |
| F10 - Ackley | 7.4043      | 9.897572  | 0.276015   | 0.50901   | 0.062087   | 0.23628   | 9.7E−08    | 4.2E−08   |
| F11 - Griewank | 0.000289    | 0.001586  | 0.009215   | 0.007724  | 27.70154   | 5.040343  | 0          | 0         |

### 2. IWOA - Xiang Wang, Liangsa Wang, Han Li, Yibin Guo
IWOA algorithm performance compared with DE and WOA.
  
| Fun. | DE mean  | DE Std.  | WOA mean  | WOA Std.  | IWOA mean | IWOA Std. |
|------|----------|----------|-------------|-------------|-----------|-----------|
| F4 - Schwefel 2.21 | 0.0011   | 0.0014   | 34.517    | 27.501    | 1.13e−4   | 1.38e−4   |
| F7 - Ackley | 1.9390   | 1.1466   | 7.93e−9   | 1.40e−9   | 8.85e−9   | 8.2e−10   |
| F9 - Griewank | 0.0162   | 0.0200   | 0.0018    | 0.0046    | 0.0027    | 0.0065    |
