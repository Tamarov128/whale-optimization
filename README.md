# Whale Search Optimization

## Problem
The optimization problem we choose to solve using the Whale Search Optimization algorithm consists in **function minimization**.

## Functions analyzed
- Schwefel's 2.21 Function (F4 & F4)  
  $$ f(\mathbf{x}) = \max_{i=1,\dots,n} |x_i| $$
  <img 
      style="display: block; 
            margin-left: auto;
            margin-right: auto;
            width: 70%;"
      src="https://al-roomi.org/multimedia/Unconstrained_Benchmarks/ndimensional/SchwefelFunction221/3D_SchwefelFunction221.jpg" 
      alt="Schwefel">
  </img>
  
- Ackley's function (F10 & F7)  
  $$ f(\mathbf{x}) = -20 \exp\left(-0.2 \sqrt{\frac{1}{n} \sum_{i=1}^{n} x_i^2} \right) - \exp\left( \frac{1}{n} \sum_{i=1}^{n} \cos(2\pi x_i) \right) + 20 + e $$
  <img 
      style="display: block; 
            margin-left: auto;
            margin-right: auto;
            width: 70%;"
      src="https://www.sfu.ca/~ssurjano/ackley.png" 
      alt="Ackley">
  </img>

- Griewank's Function (F11 & F9)  
  $$ f(\mathbf{x}) = 1 + \frac{1}{4000} \sum_{i=1}^{n} x_i^2 - \prod_{i=1}^{n} \cos\left( \frac{x_i}{\sqrt{i}} \right) $$
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

## Papers analyzed
  
1. 