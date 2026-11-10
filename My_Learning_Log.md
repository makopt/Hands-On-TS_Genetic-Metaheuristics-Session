# My Learning Log — Tabu Search & Genetic Algorithm (Self-Learning)

**Student Information:**
- **Student ID:** ___________________________
- **Student Name:** ___________________________
- **Gurobi license:** [ ] Academic [ ] Learner (size-limited)
- **Machine (OS / RAM):** ___________________________

> Fill this log **while** you work. Write your **prediction before running** and your **explanation after**. Wrong predictions are welcome — they are where you learn the most.

---

## Study Plan Tracker

| Module | Planned date | Time spent (min) | Done |
|--------|--------------|------------------|------|
| 0 — Get ready | | | [ ] |
| 1 — Knapsack, neighborhoods, Local Search | | | [ ] |
| 2 — Tabu Search | | | [ ] |
| 3 — Genetic Algorithm | | | [ ] |
| 4 — Compare, conclude, choose | | | [ ] |
| 5 — Transfer to Assignment 2 + final self-assessment | | | [ ] |

---

## Module 0 — Get Ready

- [ ] Fork cloned, Python and libraries installed, Gurobi license activated
- [ ] Smoke test (`max_instances = 3`) completed: *"TESTING COMPLETE"* displayed
- Problems met and how I solved them: ___________________________
- Curves I saw in a convergence plot: ___________________________

---

## Module 1 — Knapsack, Neighborhoods and Local Search

### Paper exercise 1 (`s = [1,1,0,1,0,0]`, n = 6)
- Number of N1 neighbors: ______  Number of N3 neighbors: ______  Number of N4 neighbors: ______
- One N3 neighbor: ___________________________

### Prediction (before running)
Ranking from best to worst — **average gap:** ___ > ___ > ___ > ___  **time (fastest → slowest):** ___ < ___ < ___ < ___

### Experiment 1 results (`max_instances = 16`)

| LS neighborhood | LS-Best Avg_Gap (%) | LS-Best Avg_Time (s) | LS-First Avg_Gap (%) | LS-First Avg_Time (s) | Avg_Iters (LS-Best) |
|---|---|---|---|---|---|
| N1 | | | | | |
| N2 | | | | | |
| N3 | | | | | |
| N4 | | | | | |

### My explanations
- (a) Was my ranking correct? Best quality / cost in time:

- (b) Why do LS-Best and LS-First give the same gap with N1?

- (c) Name of the situation where LS stops while a better solution exists; what does LS lack?

### Self-check (my answers *before* opening the hidden ones)
1. best vs first improvement:
2. why only feasible neighbors:
3. when does LS stop:

Score (out of 3): ______

---

## Module 2 — Tabu Search

### Paper exercise 2 — hand trace (tenure = 2, start `[1,1,0,0,1,1]`, value 41)

| Iter | Allowed moves examined (flip → value) | Chosen flip | New current solution | Value | Tabu list after the move | Best value so far |
|------|---|---|---|---|---|---|
| 1 | | | | | | |
| 2 | | | | | | |
| 3 | | | | | | |
| 4 | | | | | | |
| 5 | | | | | | |

- (a) Iteration where the best solution is found and its value: ___________
- (b) Forbidden attractive move in iteration 2, and why aspiration does not apply: ___________
- (c) With tenure 3, what changes at iteration 4: ___________

### Prediction (before running)
- Effect of tenure 1 / 7 / 40 on the average gap: ___________
- Effect of diversification threshold 5 / 200: ___________

### Experiment 2 results (`max_instances = 16`)

| Parameter setting | TS Avg_Gap (%) | TS Avg_Time (s) | Comment (what I notice in `TS Stats`) |
|---|---|---|---|
| default | | | |
| tabu_tenure = 1 | | | |
| tabu_tenure = 7 | | | |
| tabu_tenure = 40 | | | |
| diversification_threshold = 5 | | | |
| diversification_threshold = 200 | | | |
| *stress test:* ts_max_iter = 1 | | | |
| *stress test:* ts_max_iter = 2 | | | |
| *stress test:* ts_max_iter = 10 | | | |
| *stress test:* diversification_threshold = 1 | | | |

### My explanations
- (a) Did the tenure have the effect I predicted?

- (b) On how many instances does TS find an improvement, and after how many iterations? What does that tell me about the instances / about the role of the parameters?

- (c) Which stress test changed the result? What does it reveal about *when* TS finds its improvement (think about two consecutive 1-flip moves)?

### Self-check (my answers before opening the hidden ones)
1. why item indices in the tabu list:
2. too small / too large tenure:
3. what triggers diversification and what it does:

Score (out of 3): ______

---

## Module 3 — Genetic Algorithm

### Paper exercise 3 (8 items, W = 20)
1. P1: value ____ weight ____ · P2: value ____ weight ____
2. Child 1 = `[ _ , _ , _ , _ , _ , _ , _ , _ ]` value ____ weight ____ (feasible? ___) · Child 2 = `[ _ , _ , _ , _ , _ , _ , _ , _ ]` value ____ weight ____ (feasible? ___)
3. Repair: items removed (in order) ___________ → repaired child `[ _ , _ , _ , _ , _ , _ , _ , _ ]` value ____
4. Mutation at position 8 of the other child: `[ _ , _ , _ , _ , _ , _ , _ , _ ]` feasible? ___ value ____
5. Tournament winner: ___________

### Prediction (before running)

| Change | Gap (↑ / ↓ / =) | Time (↑ / ↓ / =) | Why |
|---|---|---|---|
| population size 10 | | | |
| population size 200 | | | |
| mutation rate 0.0005 | | | |
| mutation rate 0.2 | | | |
| tournament size 2 / 10 | | | |
| uniform instead of single-point crossover | | | |

### Experiment 3 results (`max_instances = 16`; run each setting twice)

| Setting | Run 1 Avg_Gap (%) | Run 2 Avg_Gap (%) | Avg_Time (s) | Comment (`GA Stats`) |
|---|---|---|---|---|
| default | | | | |
| population_size = 10 | | | | |
| population_size = 200 | | | | |
| mutation_rate = 0.0005 | | | | |
| mutation_rate = 0.2 | | | | |
| tournament_size = 2 | | | | |
| tournament_size = 10 | | | | |
| uniform crossover | | | | |

### My explanations
- (a) Strongest effect on quality / on time; matches my prediction?

- (b) What the number of repairs tells me:

- (c) Why a mutation rate that is too high or too low hurts (diversity, premature convergence):

- (d) Where the GA beats Tabu Search (if anywhere):

### Self-check (my answers before opening the hidden ones)
1. role of elitism:
2. crossover vs mutation:
3. greedy first individual vs random others:

Score (out of 3): ______

---

## Module 4 — Compare, Conclude, Choose

*(Run with default parameters and `max_instances = 50`, or `30` with a Learner license.)*

| Algorithm | Avg_Gap (%) | Max_Gap (%) | Avg_Time (s) |
|---|---|---|---|
| Gurobi | | | |
| LS-Best | | | |
| LS-First | | | |
| Tabu Search | | | |
| Genetic Algorithm | | | |

1. Best quality / fastest / cost of the quality improvement:

2. What each plot shows that the table does not (`quality_vs_time_tradeoff`, `performance_distribution`, `average_performance_comparison`):

3. Two convergence plots (names: ____________ / ____________) — what I observe:

4. Instance with a negative gap: ____________ — my explanation:

5. Why still study heuristics although Gurobi is fast here:

### Decisions (algorithm + justification with my results)
- (a) Real time (milliseconds):
- (b) Quality is the top priority:
- (c) Thousands of variables, no MIP model:
- (d) Problem changes every few seconds:

---

## Module 5 — Transfer to Assignment 2 and Final Self-Assessment

### My design for the CFLP (before reading the Assignment 2 guide)
1. Encoding:

2. Tabu Search moves and the attribute stored in the tabu list for each:

3. GA chromosome / crossover / mutation / repair:

4. Differences with the knapsack (objective, constraints, evaluation cost):

### Comparison with the provided implementation (after reading `Assignment_2_Student_Guide.md`)
What I got right / what is different and why:

### Final self-assessment (10 questions — my answers first)
1.
2.
3.
4.
5.
6.
7.
8.
9.
10.

**Score:** ______ / 10

### Confidence (1 = lost, 5 = could teach it)

| Topic | 1 | 2 | 3 | 4 | 5 |
|---|---|---|---|---|---|
| Neighborhoods and Local Search | [ ] | [ ] | [ ] | [ ] | [ ] |
| Tabu Search (tabu list, tenure, aspiration, diversification) | [ ] | [ ] | [ ] | [ ] | [ ] |
| Genetic Algorithm (selection, crossover, mutation, repair, elitism) | [ ] | [ ] | [ ] | [ ] | [ ] |
| Designing experiments and reading results | [ ] | [ ] | [ ] | [ ] | [ ] |
| Adapting TS / GA to a new problem (CFLP) | [ ] | [ ] | [ ] | [ ] | [ ] |

### My 3 muddiest points (things I still find unclear)
1.
2.
3.

### Additional comments (optional)

---

**END OF MY LEARNING LOG**
