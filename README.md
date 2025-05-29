# Whale Optimization Algorithm (WOA & IWOA)

This project provides a Jupyter Notebook implementation of the standard Whale Optimization Algorithm (WOA) and an improved hybrid variant (IWOA) for continuous function minimization. It demonstrates how bio-inspired metaheuristics can solve nonlinear optimization problems efficiently.

## Features

* **WOA Core**: Encircling prey, bubble-net attack, and random search phases.
* **IWOA Enhancement**: Integrates Differential Evolution–style mutation and crossover to boost exploration and avoid premature convergence.
* **Benchmark Functions**: Ready-to-use implementations of Sphere, Schwefel, Ackley, and Griewank test functions.
* **Experimental Analysis**: Automated experiments across multiple dimensions, population sizes, and hyperparameters (scaling factor *F*, crossover rate *CR*, bounding limit *bl*); results are presented as average fitness and execution time tables.

## Requirements

* Python 3.6 or higher
* Dependencies listed in `requirements.txt`

Install dependencies:

```bash
pip install -r requirements.txt
```

## Usage

1. Clone or download this repository.
2. Install dependencies:

   ```bash
   pip install -r requirements.txt
   ```
3. Start Jupyter Notebook:

   ```bash
   jupyter notebook woa_implementation.ipynb
   ```
4. Open and run cells in order. In the **Run experiments** section, adjust parameters to explore different configurations.

## Structure

```
project_root/
├── presentation/
│   └── WhaleOptimization.pptx  # Overview presentation
├── project_requirements/
│   └── MH-Lab9-14.pdf        # Lab assignment details
├── resources/
│   ├── j.jcde.2019.02.002.pdf  # IWOA paper
│   └── mirjalili-lewis_whale-opt-alg.pdf  # Original WOA publication
├── static/
│   ├── Ackley.png/.svg       # Ackley function plots
│   ├── Griewank.png/.svg     # Griewank function plots
│   └── Schwefel_2.21.png/.svg # Schwefel function plots
├── requirements.txt          # Python dependencies
├── woa_implementation.ipynb  # Main notebook with implementation and experiments
└── README.md                 # Project overview
```