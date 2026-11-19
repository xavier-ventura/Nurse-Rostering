# Nurse Rostering Optimization Project

This repository contains the implementation of **Mixed Integer Linear Programming (MILP)** and **Metaheuristic** approaches to solve the **Nurse Rostering (Nurse Scheduling) Problem**. Developed as a team project for the Optimization course at the *Universitat Politècnica de València (ETSINF)*.

## Overview

Nurse rostering is an NP-hard combinatorial optimization problem that involves constructing structured shift schedules for hospital staff over a planning horizon. The goal is to generate feasible schedules that satisfy strict operational requirements while minimizing deviations from legal contracts, hospital staffing demands, and individual nurse preferences.

## Problem Description

Problem instances are provided in JSON format, defining the planning horizon (`number_of_days`), required `skills`, daily `shifts` with fluctuating demand, and `nurses` with specific contracts and preferences.

### Hard Constraints (Feasibility)
A schedule is considered feasible if:
* At least one nurse with the required skill is assigned to every shift each day.
* No nurse is assigned to more than one shift on the same day.

### Soft Constraints (Objective Function)
The objective is to minimize the total number of deviations from the ideal schedule across the following criteria:
* **Shift Demand:** Deviations (understaffing or overstaffing) from the preferred number of nurses per day and shift.
* **Total Assignments:** Working fewer than `min_assignments` or more than `max_assignments` during the planning period.
* **Consecutive Working Days:** Violating the `min_consecutive_days` or `max_consecutive_days` limits.
* **Consecutive Free Days:** Violating the `min_consecutive_free_days` or `max_consecutive_free_days` rest limits.
* **Weekend Policies:** Failing to assign both Saturday and Sunday (`complete_weekends`) or assigning different shifts on Saturday and Sunday (`same_shift_weekend`) when a nurse works a weekend.
* **Unwanted Patterns:** Assigning forbidden consecutive shift sequences (e.g., night shift followed by an early shift).
* **Personal Requests:** Assigning a nurse on requested `days_off` or to specific avoided shifts (`shift_off`).

## Optimization Approaches

The project tackles the problem using two distinct methodologies:
1. **MILP Model (`milp.py`):** A mathematical optimization model built with Google OR-Tools (`pywraplp`) using freely available solvers.
2. **Metaheuristic (`metaheuristic.py`):** A heuristic/evolutionary search algorithm (hybrid methods combining heuristics and exact methods are permitted, provided MILP is not the sole optimizer).

## Repository Structure

* `metaheuristic.py`: Contains the `Metaheuristic` class implementing `read_problem_instance()`, `run()`, and `get_best_solution()`.
* `milp.py`: Contains the MILP formulation and solver execution logic.
* `experiments_metaheuristic.py` & `experiments_milp.py`: Reproducible scripts for hyperparameter tuning, operator selection, and solver benchmarking.
* `tester.py`: Template for executing algorithms within the strict time limit.

### Output Format
The `get_best_solution()` method returns a list of lists of strings representing the schedule for each worker per day, using `"OFF"` for unassigned days.

## Technical Constraints & Evaluation

Both models are benchmarked against baseline strategies (Random Search, a baseline Genetic Algorithm, and a baseline MILP) and competing team submissions in a tournament evaluated via average ranking across hidden test instances.

* **Language:** Python 3.10
* **Allowed Libraries:** Python Standard Library, `NumPy`, `Scikit-learn`, and `OR-Tools` (`pywraplp`)
* **Time Limit:** 120 seconds per run (including instance reading and initialization)
* **Hardware Limits:** Single-threaded execution (no parallelism), maximum 8GB RAM, and no disk read/write operations during execution
