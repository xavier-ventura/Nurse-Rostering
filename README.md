# Medical Scheduling Optimization Project

This repository contains the implementation of **Mixed Integer Linear Programming (MILP)** and **Metaheuristic** approaches to solve the **Nurse Rostering (Nurse Scheduling) Problem**[cite: 1, 3, 11]. Developed as a team project for the Optimization course at the *Universitat Politècnica de València (ETSINF)*[cite: 1].

## Overview

Nurse rostering is an NP-hard combinatorial optimization problem that involves constructing structured shift schedules for hospital staff over a planning horizon[cite: 3]. The goal is to generate feasible schedules that satisfy strict operational requirements while minimizing deviations from legal contracts, hospital staffing demands, and individual nurse preferences[cite: 4, 9].

## Problem Description

Problem instances are provided in JSON format, defining the planning horizon (`number_of_days`), required `skills`, daily `shifts` with fluctuating demand, and `nurses` with specific contracts and preferences[cite: 11, 12].

### Hard Constraints (Feasibility)
A schedule is considered feasible if[cite: 4]:
* At least one nurse with the required skill is assigned to every shift each day[cite: 4].
* No nurse is assigned to more than one shift on the same day[cite: 4].

### Soft Constraints (Objective Function)
The objective is to minimize the total number of deviations from the ideal schedule across the following criteria[cite: 9, 10]:
* **Shift Demand:** Deviations (understaffing or overstaffing) from the preferred number of nurses per day and shift[cite: 4, 5].
* **Total Assignments:** Working fewer than `min_assignments` or more than `max_assignments` during the planning period[cite: 4, 5].
* **Consecutive Working Days:** Violating the `min_consecutive_days` or `max_consecutive_days` limits[cite: 4, 6].
* **Consecutive Free Days:** Violating the `min_consecutive_free_days` or `max_consecutive_free_days` rest limits[cite: 4, 6].
* **Weekend Policies:** Failing to assign both Saturday and Sunday (`complete_weekends`) or assigning different shifts on Saturday and Sunday (`same_shift_weekend`) when a nurse works a weekend[cite: 4, 7, 8].
* **Unwanted Patterns:** Assigning forbidden consecutive shift sequences (e.g., night shift followed by an early shift)[cite: 4, 8].
* **Personal Requests:** Assigning a nurse on requested `days_off` or to specific avoided shifts (`shift_off`)[cite: 4, 8, 9].

## Optimization Approaches

The project tackles the problem using two distinct methodologies[cite: 11]:
1. **MILP Model (`milp.py`):** A mathematical optimization model built with Google OR-Tools (`pywraplp`) using freely available solvers[cite: 13, 18].
2. **Metaheuristic (`metaheuristic.py`):** A heuristic/evolutionary search algorithm (hybrid methods combining heuristics and exact methods are permitted, provided MILP is not the sole optimizer)[cite: 11, 17].

## Repository Structure

* `metaheuristic.py`: Contains the `Metaheuristic` class implementing `read_problem_instance()`, `run()`, and `get_best_solution()`[cite: 13, 14].
* `milp.py`: Contains the MILP formulation and solver execution logic[cite: 13, 14].
* `experiments_metaheuristic.py` & `experiments_milp.py`: Reproducible scripts for hyperparameter tuning, operator selection, and solver benchmarking[cite: 14, 15].
* `tester.py`: Template for executing algorithms within the strict time limit[cite: 14].

### Output Format
The `get_best_solution()` method returns a list of lists of strings representing the schedule for each worker per day, using `"OFF"` for unassigned days[cite: 14].

## Technical Constraints & Evaluation

Both models are benchmarked against baseline strategies (Random Search, a baseline Genetic Algorithm, and a baseline MILP) and competing team submissions in a tournament evaluated via average ranking across hidden test instances[cite: 15, 16, 17, 18].

* **Language:** Python 3.10[cite: 17, 18]
* **Allowed Libraries:** Python Standard Library, `NumPy`, `Scikit-learn`, and `OR-Tools` (`pywraplp`)[cite: 17, 18]
* **Time Limit:** 120 seconds per run (including instance reading and initialization)[cite: 14]
* **Hardware Limits:** Single-threaded execution (no parallelism), maximum 8GB RAM, and no disk read/write operations during execution[cite: 17, 18]
