# Carpooling Route Optimisation Algorithms

[![Python](https://img.shields.io/badge/Python-3.8+-blue?logo=python)](https://www.python.org/)
[![License](https://img.shields.io/badge/License-MIT-yellow.svg)](LICENSE)

Comparative implementation and analysis of optimization algorithms for the carpooling routing problem, focusing on minimizing total travel distance while respecting vehicle capacity constraints.

## Overview

This project tackles the carpooling route optimization problem for commuters traveling to Polo Scientifico Tecnologico UniFe. The objective is to minimize the total distance traveled by all vehicles while ensuring each car carries up to 5 people (including the driver). Multiple algorithmic approaches are implemented and compared, ranging from constructive greedy methods to advanced metaheuristics.

## Problem Statement

Given the locations of individuals who travel daily to a common destination (Polo Scientifico Tecnologico UniFe), determine optimal carpooling routes that minimize total distance traveled. 

**Constraints:**
- Each vehicle has a capacity of 5 people (driver + 4 passengers)
- Drivers are available to provide carpooling service
- Passengers who cannot be assigned to a route travel independently
- The goal is to minimize the sum of all travel distances

## Greedy Algorithms

### Greedy 1 Algorithm

**Idea:**

Each driver picks up the nearest passenger until the car is full. This approach ensures that the carpooling routes are as short as possible by always selecting the closest passenger, thereby minimizing the additional distance traveled for each pickup. This greedy strategy is simple and efficient, making it a good starting point for route optimization.

**Implementation:**

For each driver, a function is used to calculate the nearest passenger among all users, in terms of Euclidean distance. Once the nearest passenger is found, the driver moves to its location, and the calculation is repeated from that point. The algorithm terminates when the driver's car is full, and then they can proceed to the destination.

### Greedy 2 Algorithm

**Idea:** 

The basic idea is the same as the Greedy 1 algorithm, but changes the method for evaluating which passenger is most convenient to pick up before reaching the destination. In this method, the distance between the driver and the passenger and the distance between the passenger and the destination are evaluated.

**Implementation:**

For each driver, a function is used to calculate the nearest passenger among all users. The distance is calculate using the formula:

$$ dist = \frac{k \cdot d_{D-U}}{d_{U-P}}$$

where:
- $k$ is a parameter used to add weight to the distance $d_{D-U}$
- $d_{D-U}$ is the Euclidean Distance between the driver and the user
- $d_{U-P}$ is the Euclidean Distance between the user and the polo

Once the nearest passenger is found, the driver moves to its location, and the calculation is repeated from that point. The algorithm terminates when the driver's car is full, and then they can proceed to the destination.

### Greedy Results

![ResultGreedy](https://github.com/user-attachments/assets/49ae1815-7f5b-4c14-9d07-457badb15634)

## Constructive Greedy Algorithms

### K-Means 1 Algorithm

**Idea:** 

This algorithm uses clustering techniques to optimize the routes taken by drivers. The algorithm used is K-Means, which, starting from the drivers' positions, finds the nearest users in terms of Euclidean distance and adds them to the same cluster. Once the clusters are obtained, the route is created by finding the four nearest neighbors of the driver.

**Implementation:**

For this algorithm, we used the sklearn library, which provides the implementation of the K-Means algorithm. The centroids are initialized at the drivers' positions, and the algorithm iterates by modifying the centroids' positions and assigning users to clusters until it reaches a stable state where no further changes are made. Once the final clusters are obtained, starting from the driver, the route is created using an algorithm that finds the four nearest neighbors of the driver, that belong to the same cluster.

### K-Means 2 Algorithm

**Idea:** 

This algorithm uses the same K-Means algorithm described in K-Means 1 but once the clusters are obtained, the route is created using the distance algorithm also used in Greedy 2.

**Implementation:**

Also for this algorithm the centroids are initialized at the drivers' positions, and the algorithm iterates by modifying the centroids' positions and assigning users to clusters until it reaches a stable state where no further changes are made. Once the final clusters are obtained, starting from the driver, the route is created using the formula described in Greedy 2, which considers both the distance between the driver and the passenger and the distance between the passenger and the destination.

### K-Means Results

![ResultKMeans](https://github.com/user-attachments/assets/d12875c9-84e2-474c-9919-0bcd0f8437ff)

## Local Search Algorithms

These algorithms aim to improve an initial solution (typically generated by a constructive method such as Greedy or K-Means) by exploring its neighborhood to find better alternatives based on the objective function — minimizing the total kilometers driven.

### Local Search 1 Algorithm (Simple Local Search)

**Idea:**

This basic local search iteratively explores the neighborhood of the current solution using simple moves. If a better solution is found in the neighborhood, it replaces the current one. The process continues until no further improvements are possible.

**Implementation:**

- **Move 1**: Swap a passenger not included in any route with one that is part of a route.
- **Move 2**: Change the order of passengers within the same route to reduce the total route cost.
- **Move 3**: Insert an unassigned passenger into a route that has available capacity.

These moves are applied in sequence, and if any of them improves the objective function, the change is accepted.

### Local Search 1 Results

![ResultLocalSearch1](https://github.com/user-attachments/assets/20922a68-ed32-461f-8f3a-6c0dbc9bdc3d)

---

### Local Search 2 Algorithm (Variable Neighborhood Descent - VND)

**Idea:**

This method sequentially applies each neighborhood move to local optimality. It explores different neighborhoods one after another and always adopts the best improving move before moving to the next neighborhood structure.

**Implementation:**

- Executes **Move 1** until no improvement is found.
- Then continues with **Move 2**, again until no improvement is possible.
- Finally applies **Move 3** in the same way.
- Among the final solutions obtained from each phase, the one with the best cost is chosen.

### Local Search 2 Results

![ResultLocalSearch2](https://github.com/user-attachments/assets/a618270d-7574-423b-88be-acd6d0051fe4)

---

### Local Search 3 Algorithm (Very Large Neighborhood Search - VLSN)

**Idea:**

This variant considers all neighborhood moves and applies the one that gives the largest improvement in each iteration. The process is repeated until no move can further improve the solution.

**Implementation:**

- At each step, evaluate all three moves across all users and drivers.
- Select the single move that yields the **maximum decrease in total distance**.
- Repeat until all moves fail to produce improvements.

This is the most computationally demanding of the three but also the most effective in many cases.

### Local Search 3 Results

![ResultLocalSearch3](https://github.com/user-attachments/assets/693225ce-4d86-4fec-8aa5-116668e8dd00)

---

## Multi Start Local Search Algorithm

This metaheuristic enhances the search by performing multiple independent runs of the VLSN algorithm, each starting from a different initial solution. These initial solutions are generated using randomized versions of the constructive algorithms (Greedy Random, Greedy, K-Means).

**Idea:**

By exploring several starting points, the algorithm increases diversity and reduces the risk of getting trapped in local minima.

**Implementation:**

- Generate 10 randomized starting solutions using Greedy Random, Greedy 1, or K-Means 2.
- For each, apply the Very Large Neighborhood Search (VLSN).
- Keep the solution with the best objective function value across all runs.

### Multi Start Local Search Results

![ResultMultiStartLocalSearch](https://github.com/user-attachments/assets/c1df94fa-7459-4f25-9374-883ed717b30e)

---

## Genetic Algorithm
The last algorithm we implemented is the genetic algorithm, which is a multi-threaded and population-based algorithm. This type of algorithm is inspired by the mechanism of natural selection and uses the operators of selection, crossover, and mutation, repeating them for several iterations and evolving the population of solutions.
We tested the algorithm on 3 different initial solutions provided by the Greedy Random, Greedy, and K-Means algorithms. The algorithm generates an initial population of 100 individuals, which are essentially the initial solutions obtained through the chosen algorithm, differing from each other due to the random ordering of the drivers. 
We did not encode the solution into a bit string because it was difficult to operate on the solutions; instead, we kept the representation as a matrix of points where each row represents a driver's route, passing through users and reaching the destination. The algorithm iterates for 100 generations, and in each generation, it applies the evolve function, allowing the population to evolve to find the best solution. 
The evolve operation consists of 3 phases: selection, where 2 parent individuals are selected from the population to create offspring. These are chosen using the roulette wheel method, favoring individuals with better fitness, which in our case is calculated as the cost of the solution, i.e., the value of the objective function. After the selection phase, we implemented the crossover, which allows the offspring to inherit genes from both parent individuals. Only the routes with drivers are selected from these genes, excluding those related to passengers who go directly to the destination. A random number of genes from parent 1, less than half of the possible genes, are inserted, and the genes from parent 2 are tested for insertion, only if the points are not already present in the offspring. 
Finally, we implemented the mutate phase, which consists, with a 20% probability, of performing the best possible swap between a passenger in a track and a free one, if this brings an improvement. The remaining free driver nodes create the route with the remaining passengers using a Greedy-like algorithm and reach the destination, and finally, the last remaining users reach the destination independently.
Repeating these steps for 100 generations, the initial population evolves, and in the end, the individual with the lowest objective function value among all those tested is kept. 
Applying this algorithm to the Greedy Random, we obtained an improvement of almost 2000 on the objective function, which still remains quite high, and we also obtained some improvement on Greedy 1 and K-Means 2. 
In the small and medium instances, there is not much randomness in the initial solutions, so the populations of the generations are quite homogeneous and struggle to find solutions that significantly change the objective function. 
In any case, some improvement is achieved, but this algorithm is the most computationally intensive.

**Implementation:**

- **Initial Population**: 100 individuals generated from randomized variants of the solutions obtained with Greedy Random, Greedy 1, or K-Means 2.
- **Representation**: Each individual is a matrix where each row corresponds to a driver’s route.
- **Selection**: Roulette wheel method based on fitness (lower total distance implies higher fitness).
- **Crossover**: 
  - Select a random number of "genes" (routes) from parent 1.
  - Complete the offspring with routes from parent 2, ensuring no duplicate nodes.
  - Only driver routes are inherited; direct passengers are excluded.
- **Mutation**:
  - With 20% probability, swap a passenger from a route with an unassigned passenger, if it leads to improvement.
  - Remaining drivers complete their routes greedily; unassigned passengers travel alone.

The algorithm runs for 100 generations, keeping the individual with the lowest cost.

**Performance Considerations**:

- GA is computationally intensive.
- More effective on larger and more diverse instances.
- Achieves improvements over initial solutions, especially when the solution space is diverse.

### Genetic Algorithm Results

![ResultGeneticAlgorithm](https://github.com/OnestiFilippo/CarPooling/blob/6fd47cee1e7a1264d447dd2d3be52ed929eb4e28/screenshots/GA_ALL.png)
