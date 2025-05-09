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
  
- Ackley's function (F10 & F7)  
    
  <img 
      style="display: block; 
            margin-left: auto;
            margin-right: auto;
            width: 70%;"
      src="https://www.sfu.ca/~ssurjano/ackley.png" 
      alt="Ackley">
  </img>

- Griewank's Function (F11 & F9)  
    
  <img 
      style="display: block; 
            margin-left: auto;
            margin-right: auto;
            width: 70%;"
      src="https://www.sfu.ca/~ssurjano/griewank.png" 
      alt="Griewank">
  </img>

Image sources: 
- Surjanovic, S. & Bingham, D. (2013). Virtual Library of Simulation Experiments: Test Functions and Datasets. http://www.sfu.ca/~ssurjano.
- Al-Roomi, A. H. (n.d.). Schwefel’s Function No. 2.21. AL-Roomi. https://al-roomi.org/benchmarks/unconstrained/n-dimensions/189-schwefel-s-function-no-2-21

## Algorithm variants analyzed

### Whale Optimization Algorithm

Algorithm overview:
    
#### Phase 1: Encircling Prey
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

#### Phase 2: Bubble-Net Attacking Method (Exploitation Phase)
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

#### Phase 3: Search for Prey (Exploration Phase)
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

### Improved Whale Optimization Algorithm

Algorithm overview: