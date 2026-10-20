# IA25_P01_18650

Artificial Intelligence - Project 01

Cretor: Luís Costa  
Repository: https://github.com/ffernandesc/IA25_P01_18650

## Note:

This project is strictly an academic solution to a given problem. It should not be used comercially.
I, the creator, **do not** take responsibility for the improper use of the code present in this repository.
All rights reserved.

## Project Description

This project implements an agent for weekly night-shift factory scheduling.

The objective is to generate a feasible production schedule while respecting constraints related to production lines, tasks, machines, operators, machine maintenance, operator availability, mandatory machine assignments and remote tasks.

The problem is represented as a Constraint Satisfaction Problem (CSP) and solved in Python using a Jupyter Notebook and the `python-constraint` library.

The work was developed individually. The project identifier uses the student number as the group identifier, as there is no group in this particular work.

## Theme

Night-Shift Factory Scheduling

The factory schedule is organised into 35 weekly slots:

- 5 nights;
- 7 two-hour blocks per night.

Each task-run must be assigned to a valid time slot and machine.

## Files

Main notebook:

- `IA_Project01.ipynb`

Factory datasets:

- `factory_tiny.txt`
- `factory_real.txt`

Dependency file:

- `requirements.txt`

Informational files:

- `README.md`
- `readme.txt`

## Notebook Contents

The notebook contains the following sections:

1. Introduction
2. Environment and imports
3. Dataset parsing
4. Conversion of raw sections into structured data
5. Dataset inspection
6. Agent design and CSP data preparation
7. CSP formulation
8. Hard constraints
9. CSP execution with `factory_tiny.txt`
10. Solution counting
11. Soft constraints and scoring
12. Schedule visualisation
13. Execution with `factory_real.txt`
14. Critical analysis
15. Future improvements
16. Conclusion

## CSP Formulation

Variables:

- Each weekly task execution is represented as a variable.
- Example: `("T11", 1)` represents the first weekly execution of task `T11`.

Domains:

- Each variable receives possible `(slot, machine)` assignments.
- Domains are filtered using machine compatibility, operator availability, machine maintenance and mandatory-machine restrictions.

Hard constraints:

- A task-run must use a valid slot and machine.
- A task can only use a compatible machine.
- Mandatory machine restrictions must be respected.
- A production line cannot run more than one task in the same slot.
- A machine cannot execute two tasks at the same time.
- An operator cannot execute two tasks at the same time.
- A line cannot run more than three tasks per night.
- The first run of a task must occur before the second run.
- Remote task-runs must be scheduled on the same night.

Soft constraints:

- Repeated runs of the same task should occur on distinct nights.
- Each line should preferably use only four nights.
- Tasks for the same line should be consecutive within a night when possible.
- The number of machines used by each line should be minimised.

The notebook evaluates feasible schedules using a penalty function. Lower scores represent better schedules.

## How to Run

Create and activate a virtual environment.

Mac/Unix-based systems:

```bash
python3 -m venv .venv
source .venv/bin/activate
```

Windows:

```bash
python -m venv .venv
.venv\Scripts\activate
```

Install the required dependencies:

```bash
pip install -r requirements.txt
```

Open the notebook:

```bash
jupyter notebook IA_Project01.ipynb
```

Then run the notebook cells in order from top to bottom.

## Requirements

The project requires Python 3 and the dependency listed in `requirements.txt`:

- `python-constraint`

## Notes

The notebook first explains the model step by step using `factory_tiny.txt`, then applies the same scheduling pipeline to `factory_real.txt`.

The solution returned by the notebook is feasible and validated against the main hard constraints. The best schedule reported is the best solution found within the configured search and enumeration limits, not a guaranteed global optimum.

### Performance Notes

**With:**
```
SOLUTION_SEARCH_COUNT_LIMIT = 1.000.000
SOLUTION_SEARCH_TIME_LIMIT = 60
```

The benchmarks were:
- **Tiny Dataset:** 
    - 34.5s Total (23.1s search + 11.4s scoring)

- **Real Dataset**
    - 29.1s Total (15.7s search + 13.4s scoring)


---

**With:**
```
SOLUTION_SEARCH_COUNT_LIMIT = 5.000.000
SOLUTION_SEARCH_TIME_LIMIT = 180
```

The benchmarks were:
- **Tiny Dataset:** 
    - 181.4s Total (121.78s search + 59.6s scoring)

- **Real Dataset**
    - 167.66s Total (98.59s search + 69.07s scoring)
