# 📘 Self-Learning Guide — Tabu Search & Genetic Algorithm

**Notebook:** `Knapsack_Tabu_Genetic.ipynb` · **Log to fill:** `My_Learning_Log.md` · **Overview:** `README.md`

> **Golden rule:** write your answer / prediction in your log **first**, and open the hidden answers (▶ *Show answer*) **afterwards**.

---

## 📋 Table of Contents

0. [Module 0 — Get ready (30 min)](#module-0--get-ready-30-min)
1. [Module 1 — Knapsack, neighborhoods and Local Search (60 min)](#module-1--knapsack-neighborhoods-and-local-search-60-min)
2. [Module 2 — Tabu Search (60 min)](#module-2--tabu-search-60-min)
3. [Module 3 — Genetic Algorithm (60 min)](#module-3--genetic-algorithm-60-min)
4. [Module 4 — Compare, conclude, choose (45 min)](#module-4--compare-conclude-choose-45-min)
5. [Module 5 — Transfer to Assignment 2 and final self-assessment (45 min)](#module-5--transfer-to-assignment-2-and-final-self-assessment-45-min)
6. [Appendix — How to change a parameter and re-run quickly](#appendix--how-to-change-a-parameter-and-re-run-quickly)

---

## Module 0 — Get ready (30 min)

**Goal:** have a working environment and have seen the notebook run once from top to bottom.

1. Follow the setup of the `README.md` (fork, Python, libraries, Gurobi license, VS Code).
2. Open `Knapsack_Tabu_Genetic.ipynb`, set `max_instances = 3` (Section 1.2) and run all cells.
3. **Skim the notebook structure** by scrolling through the section titles (you will study each part later):

   | Notebook section | Content |
   |------------------|---------|
   | 1 | Libraries, configuration classes (`AlgorithmConfig`, `GeneticAlgorithmConfig`) |
   | 2–3 | Solution encoding/evaluation, greedy and random constructive heuristics |
   | 4 | Neighborhoods N1–N4 |
   | 5 | Gurobi exact solver |
   | 6.1 / 6.2 / 6.3 | **Local Search / Tabu Search / Genetic Algorithm** |
   | 7–8 | Visualization; **Phase 1** (Gurobi) and **Phase 2** (heuristics) experiments |
   | 9–11 | Excel statistics, summary report, comparative plots, conclusions |

4. Look at one file in `plots/` (e.g., `0001_kp_instances_100_convergence.png`): which curves do you see?

✅ **Checkpoint:** the run ended with *"TESTING COMPLETE"* and `plots/`, `statistics/`, `results/` contain files.  
📝 **Log:** tick Module 0 in your log and note your machine/licence (academic or learner).

---

## Module 1 — Knapsack, neighborhoods and Local Search (60 min)

**Goal:** understand the problem, the solution encoding, neighborhoods, and why Local Search gets stuck.

### 📖 1.1 Read (15 min)
Read the markdown cells of: the introduction (problem definition and model), *Part 2 (encoding and evaluation)*, *Part 3 (constructive heuristics)*, *Part 4 (neighborhoods)* and *6.1 Local Search*.

Questions to keep in mind: *What is a solution? When is it feasible? What is the objective? Why is the greedy solution (by value/weight ratio) not always optimal?*

### ✍️ 1.2 Paper exercise 1 — Neighborhood sizes (5 min)
Consider a solution with $n=6$ items: `s = [1, 1, 0, 1, 0, 0]` (assume all neighbors are feasible).

1. List the neighbors of `s` in **N1** (flip one bit). How many are there?
2. How many neighbors does **N3** (swap: remove one selected item and add one non-selected item) have? Write one of them.
3. How many neighbors does **N4** (flip any two bits) have?

<details>
<summary>▶ Show answer</summary>

1. N1: flip each of the 6 positions → **6** neighbors, e.g., `[0,1,0,1,0,0]`, `[1,0,0,1,0,0]`, `[1,1,1,1,0,0]`, …
2. N3: $k\times(n-k)$ with $k=3$ selected items → $3\times3=$ **9** neighbors, e.g., remove item 1 and add item 3 → `[0,1,1,1,0,0]`.
3. N4: $\binom{6}{2}=\frac{6\cdot5}{2}=$ **15** neighbors.

*(With the knapsack capacity, only the feasible ones are kept — so the real neighborhood can be smaller.)*
</details>

### 🔮 1.3 Predict (5 min) and ▶️ Run experiment 1 (15 min)

**Question:** Local Search starts from the greedy solution. In Phase 2, LS uses neighborhood **N1**. What would happen with **N2, N3 or N4**?

📝 **In your log, before running:** rank N1, N2, N3, N4 from best to worst for (a) the average gap and (b) the running time.

**Experiment 1 — LS neighborhoods**
1. Set `max_instances = 16` (Section 1.2).
2. Run the notebook up to the end of **Phase 1** (Section 8.1: Gurobi, ≈ 1 minute is more than enough).
3. In the **Phase 2 cell** (Section 8.2) find the two LS calls (*LS-Best* and *LS-First*): they contain `neighborhood='N1'`. Change **both** to `'N2'`, run Phase 2 and Sections 9.1–9.2, and write down in the log the **Avg_Gap** and **Avg_Time** of LS-Best and LS-First.
4. Repeat with `'N3'` and `'N4'`. (See the [appendix](#appendix--how-to-change-a-parameter-and-re-run-quickly) to re-run quickly.)

### 💬 1.4 Explain (10 min)
Answer in your log, using your numbers:
- (a) Was your ranking correct? Which neighborhood gives the best quality, and what does it cost in time?
- (b) With **N1**, *LS-Best* and *LS-First* give the same gap. Why? (Hint: look at the number of LS iterations `Avg_Iters` and at the gap of the greedy start.)
- (c) Even the best neighborhood is not always optimal. What is the name of the situation where LS stops while a better solution exists, and what does LS lack to escape it?

### ✅ 1.5 Self-check (5 min)
1. What does *best-improvement* do differently from *first-improvement*?
2. Why does the notebook keep only **feasible** neighbors?
3. When does a Local Search stop?

<details>
<summary>▶ Show answers</summary>

1. Best-improvement evaluates **all** neighbors and moves to the best one; first-improvement moves to the **first** neighbor that improves the current solution (cheaper iterations, possibly lower quality).
2. A neighbor whose weight exceeds the capacity is not a solution of the problem; keeping it would require repair or penalties.
3. When **no neighbor is better** than the current solution (a **local optimum**), or when an iteration/time limit is reached.
</details>

---

## Module 2 — Tabu Search (60 min)

**Goal:** understand how memory lets the search leave local optima, and **see** the effect of each component.

### 📖 2.1 Read (10 min)
Read the markdown cell **6.2 Tabu Search** (pseudocode, components, parameters) and then the code of `tabu_search`. Identify in the code: the tabu list, the aspiration test, the diversification step.

Notice that this implementation: starts from a **Local Search** solution, uses the **N1** neighborhood, stores **item indices** (the *attribute* of the move) in the tabu list, and keeps the best non-tabu neighbor **even if it is worse** than the current solution.

### ✍️ 2.2 Paper exercise 2 — Trace Tabu Search by hand (20 min)

Instance: 6 items, capacity $W=18$.

| Item | 1 | 2 | 3 | 4 | 5 | 6 |
|------|---|---|---|---|---|---|
| Value | 12 | 11 | 7 | 11 | 8 | 10 |
| Weight | 5 | 5 | 5 | 5 | 3 | 4 |

Start from `s₀ = [1,1,0,0,1,1]` (value 41, weight 17: it is a local optimum for N1). Apply **5 iterations** of Tabu Search with **tabu tenure = 2**, using these rules (the ones of the notebook):
- at each iteration, consider the **feasible 1-flip neighbors**;
- a neighbor is **forbidden** if the item flipped is in the tabu list, **unless** its value is **strictly better than the best value found so far** (aspiration);
- move to the **best allowed** neighbor, **even if it is worse** than the current solution;
- add the flipped item to the tabu list (a queue of size 2: the oldest item leaves when it is full).

Fill this table in your log:

| Iter | Allowed moves examined (flip → value) | Chosen flip | New current solution | Value | Tabu list after the move | Best value so far |
|------|---------------------------------------|-------------|----------------------|-------|--------------------------|-------------------|
| 1 | | | | | | |
| 2 | | | | | | |
| 3 | | | | | | |
| 4 | | | | | | |
| 5 | | | | | | |

Then answer: **(a)** At which iteration is the best solution found, and what is its value? **(b)** In iteration 2, which attractive move is forbidden, and why is the aspiration criterion not applied? **(c)** Repeat the trace with **tenure = 3**: what changes at iteration 4?

<details>
<summary>▶ Show answer</summary>

| Iter | Feasible neighbors (flip → value) | Chosen | New solution | Value | Tabu list | Best |
|------|-----------------------------------|--------|--------------|-------|-----------|------|
| 1 | 1→29, 2→30, **5→33**, 6→31 (flipping 3 or 4 on is infeasible) | flip 5 | `[1,1,0,0,0,1]` | 33 | [5] | 41 |
| 2 | 1→21, 2→22, 5→41 (**tabu**), **6→23** | flip 6 | `[1,1,0,0,0,0]` | 23 | [5,6] | 41 |
| 3 | 1→11, 2→12, 3→30, **4→34**, 5→31 (tabu), 6→33 (tabu) | flip 4 | `[1,1,0,1,0,0]` | 34 | [6,4] | 41 |
| 4 | 1→22, 2→23, 4→23 (tabu), **5→42** | flip 5 | `[1,1,0,1,1,0]` | **42** | [4,5] | **42** |
| 5 | 1→30, **2→31**, 4→31 (tabu), 5→34 (tabu) | flip 2 | `[1,0,0,1,1,0]` | 31 | [5,2] | 42 |

**(a)** Iteration **4**, value **42** (the optimum): the search accepted three consecutive worsening moves (33, 23, 34) — plain LS would have stopped at 41.
**(b)** In iteration 2 the move "flip 5" (back to value 41) is the best neighbor but is **tabu** (item 5 was just flipped), and 41 is **not strictly better** than the best value 41, so aspiration does not apply: the search is forced to explore elsewhere (flip 6). This is how the tabu list **prevents cycling**.
**(c)** With tenure 3 the tabu list at iteration 4 is [5,6,4], so "flip 5" is **tabu**, but its value **42 > 41 (best so far)**: the **aspiration criterion** overrides the tabu status, and the move is accepted (the notebook counts it in `tabu_overrides`).
</details>

### 🔮 2.3 Predict and ▶️ Run experiment 2 (20 min)

📝 **Predict (in your log):** with `max_instances = 16`, what will happen to the average gap of Tabu Search if you (i) set the tabu tenure to **1**, **7**, **40**? (ii) change the diversification threshold to **5** or **200**? Will the gap increase, decrease, or stay the same? Why?

**How to run it:** add, at the **end** of `AlgorithmConfig.__init__` (Section 1.3), the line to test, for example:
```python
self.tabu_tenure = 1                      # then 7, 40
self.diversification_threshold = 20       # then 5, 200
```
Then add the **stress tests** (one at a time, to see what happens when a mechanism is almost switched off):
```python
self.ts_max_iter = 1                      # then 2, 10
self.diversification_threshold = 1
```
Re-run the class cell, then Phase 2 (Section 8.2) and Section 9.2, and record the **Avg_Gap** and **Avg_Time** of *Tabu Search* in your log (one row per setting).

**Then look inside TS** for a few instances: in the Phase 2 output, read the line **`TS Stats: X improvements, Y aspiration uses, Z diversifications`**.

### 💬 2.4 Explain (10 min)
- (a) Compare your results with your predictions. Did the tenure have the effect you expected?
- (b) **If you observe little or no difference between settings, do not assume your experiment is wrong.** Use the `TS Stats` lines and the convergence plots to answer: *on how many of the instances does Tabu Search find any improvement over the Local Search starting solution, and after how many iterations?*
- (c) Which **stress test** finally changed the result, and what does it reveal about *when* Tabu Search finds its improvement on these instances? (Hint: what do **two consecutive 1-flip moves** — one removing an item, one adding another — amount to, and which neighborhood does that correspond to?)

<details>
<summary>▶ Show explanation (open it after you have tried (a)–(c))</summary>

On these instances the parameters `tabu_tenure`, `diversification_threshold` and the number of iterations make **no visible difference** (as long as they are not extreme), because **all the improvement happens in the first two moves**: the first move is a **worsening** 1-flip (an item leaves the knapsack), the second one adds an item that now fits and gives a better value — together they act as a **swap (N3)**, which Local Search with N1 cannot do because it refuses the first (worsening) move. After that, the search does not find anything better, so a longer run or a different tenure cannot change the best value. The stress tests break exactly this mechanism: with `ts_max_iter = 1` there is only one (worsening) move, and with `diversification_threshold = 1` the search is sent back to the best solution at each iteration — in both cases Tabu Search ends with the **same gap as the Local Search/greedy start**. Tenure and diversification would matter on **harder instances** or **longer runs**, where cycling really happens: *a parameter only matters if the search runs long enough to face the problem it solves*.
</details>

### ✅ 2.5 Self-check (5 min)
1. Why does the tabu list store *item indices* and not complete solutions?
2. What happens if the tenure is too small? Too large?
3. What triggers diversification in this notebook, and what does it do?

<details>
<summary>▶ Show answers</summary>

1. Storing complete solutions is expensive (memory and comparisons); storing the **attribute** of the move (the flipped item) is cheap and forbids a whole family of solutions that would undo the recent moves.
2. Too small: the search can **cycle** (return to recent solutions). Too large: the search is **over-restricted** and may miss good moves (many moves forbidden).
3. When there are `diversification_threshold` iterations **without improvement** (or when no feasible neighbor exists): the search restarts from the best solution after a **random perturbation** (a few flips, then repair) and the tabu list is cleared.
</details>

---

## Module 3 — Genetic Algorithm (60 min)

**Goal:** understand population-based search and each genetic operator, and see how its parameters behave.

### 📖 3.1 Read (10 min)
Read the markdown cell **6.3 Genetic Algorithm** (evolutionary cycle, pseudocode, components) and the code of `genetic_algorithm` and `GeneticAlgorithmConfig`. Identify: initialization, tournament selection, crossover, mutation, repair, elitism.

Notice that in this implementation: the **crossover is single-point** (a `uniform_crossover` helper exists but is not used), the **repair** removes the selected item with the **worst value/weight ratio** until the solution is feasible, and the first individual of the population is the **greedy** solution.

### ✍️ 3.2 Paper exercise 3 — One GA step by hand (15 min)

Instance: 8 items, capacity $W=20$.

| Item | 1 | 2 | 3 | 4 | 5 | 6 | 7 | 8 |
|------|---|---|---|---|---|---|---|---|
| Value | 8 | 5 | 9 | 6 | 7 | 3 | 4 | 10 |
| Weight | 4 | 3 | 6 | 5 | 4 | 2 | 3 | 7 |

Parents: `P1 = [1,1,0,0,1,1,1,0]` and `P2 = [0,0,1,1,0,1,0,1]`.

1. Compute the value and the weight of `P1` and `P2`. Are they feasible?
2. Apply a **single-point crossover** with the cut point after position **2** (the first 2 genes come from the first parent, the rest from the other). Write the two children, their values and their weights. Which child is infeasible?
3. **Repair** the infeasible child with the notebook's rule (repeatedly remove the **selected** item with the **lowest value/weight ratio** until feasible). Which items are removed? What is the final value?
4. Apply a **mutation** to the other child by flipping the gene at **position 8**. Is the mutated child feasible? What is its value?
5. (Tournament) A population has fitness values `[27, 28, 25, 14]` for individuals 1–4. A tournament of size 2 draws individuals 3 and 4. Who wins?

<details>
<summary>▶ Show answer</summary>

1. `P1`: value 8+5+7+3+4 = **27**, weight 4+3+4+2+3 = **16** (feasible). `P2`: value 9+6+3+10 = **28**, weight 6+5+2+7 = **20** (feasible, exactly full).
2. Child 1 = `P1[1..2] + P2[3..8]` = `[1,1,1,1,0,1,0,1]` → value **41**, weight **27 > 20 → infeasible**. Child 2 = `P2[1..2] + P1[3..8]` = `[0,0,0,0,1,1,1,0]` → value **14**, weight **9** (feasible).
3. Ratios of the selected items of child 1: item 1: 2.00, item 2: 1.67, item 3: 1.50, item 4: **1.20**, item 6: 1.50, item 8: 1.43. Remove item 4 (weight 22, still > 20), then item 8 (ratio 1.43; weight 15). Repaired child: `[1,1,1,0,0,1,0,0]`, value **25**, weight **15**.
4. Flipping position 8 of child 2 → `[0,0,0,0,1,1,1,1]`: weight 4+2+3+7 = **16** (feasible), value 7+3+4+10 = **24**.
5. Individual **3** (fitness 25 > 14): the tournament keeps the fittest of the sampled individuals.
</details>

### 🔮 3.3 Predict and ▶️ Run experiment 3 (25 min)

📝 **Predict (in your log):** for each change below, will the average gap of the GA go **up, down, or stay the same**, and will the **time** change?
- (a) population size 10 / 30 / 200 · (b) mutation rate 0.0005 / 0.2 · (c) tournament size 2 / 10 · (d) **single-point → uniform crossover**.

**How to run it (a–c):** at the **end** of `GeneticAlgorithmConfig.__init__` (Section 1.3.1, after `self.selection_pressure = 'medium'`) add one line at a time, for example:
```python
self.population_size = 10        # 30, 200
self.mutation_rate = 0.2         # 0.0005
self.tournament_size = 2         # 10
```
**(d)** In the `genetic_algorithm` code cell (Section 6.3) replace `single_point_crossover(parent1, parent2)` by `uniform_crossover(parent1, parent2)` (one place only). Re-run the changed cell(s), Phase 2 (Section 8.2) and Section 9.2.

The GA is **random**: run each setting **twice** (or add `np.random.seed(…)` at the top of the Phase 2 cell) and note the variation between runs before concluding anything.

In the Phase 2 output, read the line **`GA Stats: … crossovers, … mutations, … repairs`**.

### 💬 3.4 Explain (10 min)
- (a) Which parameter has the strongest effect on **quality**? On **time**? Does it match your prediction?
- (b) What do the very large number of **repairs** tell you about how crossover and the knapsack constraint interact?
- (c) Why can a mutation rate that is too high (0.2) or too low (0.0005) hurt a GA? Connect it with the notions of *diversity* and *premature convergence*.
- (d) In which situations does the GA beat Tabu Search in your results, if any?

### ✅ 3.5 Self-check (5 min)
1. What is the role of **elitism**, and what could happen without it?
2. What is the difference between *crossover* and *mutation* in terms of exploration?
3. Why is the first individual of the population the greedy solution, and the others random?

<details>
<summary>▶ Show answers</summary>

1. Elitism copies the best individual(s) unchanged into the next generation. Without it, the best solution found can be **lost** by crossover/mutation/replacement, and the quality can go down from one generation to the next.
2. Crossover **recombines** existing genetic material of good parents (it mainly *exploits* what the population already knows); mutation introduces **new** genes at random (it *explores* and restores diversity).
3. The greedy solution gives a **good starting point** (guarantees the GA is never worse than greedy); the random individuals provide **diversity** so that the population does not start in a single region.
</details>

---

## Module 4 — Compare, conclude, choose (45 min)

**Goal:** compare all algorithms on the **full set of instances** and take a justified decision.

### ▶️ 4.1 Run (10 min)
1. **Restore the default code** (remove the lines you added to the configuration classes and put back `single_point_crossover`), and set `max_instances = 50` (or `30` with a Learner license).
2. Run the whole notebook (≈ 5 minutes). Open `results/` and `statistics/`.

### 💬 4.2 Analyze (25 min) — answer in your log with numbers

> ⚠️ Some markdown cells of the notebook describe *"expected"* gaps (for example, several percent for LS). **Do not copy them.** Report what **your own run** shows, and comment on any difference with those statements.

1. From the *Summary Statistics* table (Section 9.2): give the **Avg_Gap**, **Max_Gap** and **Avg_Time** of LS-Best, LS-First, Tabu Search and Genetic Algorithm. Which algorithm has the best quality? Which is the fastest? What is the cost (time) of the quality improvement?
2. Open `results/quality_vs_time_tradeoff.png`, `performance_distribution.png` and `average_performance_comparison.png`. What does each plot show that the table does not?
3. Open two convergence plots in `plots/` (one small instance and one large instance). On which one do the heuristics reach the optimum quickly? How does Gurobi's behavior compare?
4. **A puzzle:** find in `statistics/detailed_results.xlsx` an instance where a heuristic gap is **negative** (a heuristic seems better than the "optimal" Gurobi value). Is it a bug? Suggest an explanation. *(Hint: what stopping tolerance does Gurobi use by default?)*
5. **A question to think about:** here Gurobi solves each instance in a fraction of a second. So why do we still study heuristics?

<details>
<summary>▶ Show hints (open after you have written your own answers)</summary>

- **Q1:** differences are small in absolute terms (gaps of a fraction of a percent); judge them in **relative** terms (e.g., "TS gap is X times smaller than LS") and compare with the extra time.
- **Q4:** Gurobi stops when the proven gap is below its default optimality tolerance (**MIPGap = 0.01%**), so its "optimal" value can be very slightly below the true optimum; a heuristic that finds a marginally better value gets a tiny negative gap (−0.01%). It is a **measurement tolerance**, not a bug.
- **Q5:** knapsack is a *very easy* NP-hard problem for a modern MIP solver (a single constraint). Heuristics matter when the problem is **larger or structurally harder** (e.g., the CFLP of Assignment 2), when a **time limit** is strict, when **no good MIP model** exists, or when you need a **good solution in milliseconds**.
</details>

### 🎯 4.3 Decide (10 min)
For each scenario, choose Gurobi, LS, TS or GA, and justify with your results: (a) a solution is needed in **real time** (milliseconds); (b) quality is the **top priority** and time is available; (c) the model has **thousands of variables and no MIP model** is available; (d) the problem changes **every few seconds**.

---

## Module 5 — Transfer to Assignment 2 and final self-assessment (45 min)

### 🔁 5.1 Transfer (25 min) — design on paper
**Assignment 2** applies **Tabu Search** and a **Genetic Algorithm** to the **Capacitated Facility Location Problem (CFLP)**: choose which facilities to open and assign every client to exactly one open facility, minimizing opening + assignment costs, with facility capacities.

Without looking at the Assignment 2 guide, answer in your log:
1. **Encoding.** How would you encode a CFLP solution? What is the equivalent of the knapsack bit-vector?
2. **Neighborhood / moves for Tabu Search.** Propose at least three move types. For each one, what **attribute** would you store in the tabu list?
3. **GA operators.** What would a chromosome, crossover and mutation look like? Which constraint can be violated by crossover/mutation, and how would you **repair** it?
4. **Objective and feasibility.** What are the differences with the knapsack (minimization vs maximization, constraints, cost of an evaluation)?

Then open `Assignment_2_Student_Guide.md` (Section 4, *Tabu Search* and *Genetic Algorithm*) and **compare** your design with the provided implementation: what did you get right, what is different, and why?

### 🧪 5.2 Final self-assessment (20 min)
Without notes, answer these 10 questions in your log (2 minutes each), then check.

1. Why can a simple Local Search not escape a local optimum, and which two mechanisms of Tabu Search allow it?
2. Give the effect of a too small / too large tabu tenure.
3. State exactly the condition of the **aspiration criterion** in this notebook.
4. Why is a Tabu Search *trajectory-based* and a GA *population-based*? Give one advantage of each.
5. In the GA, why do children often need to be **repaired**? Which rule is used here?
6. What does **elitism** guarantee?
7. What is **premature convergence** and how do mutation / population size relate to it?
8. How is the **gap** computed, and why can it be (slightly) negative?
9. Why must parameters scale with the instance size (e.g., fewer generations and smaller populations for large instances)?
10. Name one situation where you would prefer Gurobi, and one where you would prefer a metaheuristic.

<details>
<summary>▶ Show answers</summary>

1. LS only accepts **improving** moves, so it stops at a local optimum. TS accepts the **best non-tabu neighbor even if it is worse** (non-monotone moves), and uses the **tabu list** (short-term memory) to avoid returning to recent solutions; **diversification** is the long-term complement.
2. Too small → cycling; too large → over-restriction (too many forbidden moves, poor exploration).
3. A tabu move is accepted if its value is **strictly better than the best value found so far** (`neighbor_value > best_value`).
4. TS follows **one** solution through the search space (low memory, fast, strongly guided, but can be trapped in one region). GA evolves **many** solutions at once (wide exploration and recombination of good building blocks, but needs more evaluations and parameters).
5. One-point crossover and mutation mix genes without looking at the capacity, so children can exceed it. Here, **remove the selected item with the lowest value/weight ratio** until the child is feasible.
6. That the **best solution found is never lost** from one generation to the next (the best individual(s) survive unchanged).
7. The population becomes **too similar** and stops exploring before reaching a good region. A reasonable **mutation rate** and a large enough **population** maintain diversity.
8. gap = (optimal − heuristic) / optimal × 100 (for maximization). It can be slightly negative because Gurobi stops within its **default tolerance (MIPGap = 0.01 %)**.
9. The cost of an iteration/evaluation grows with $n$ (neighborhood of size $n$): the same number of iterations would take too long, so budgets and parameters are scaled to keep a **fair and feasible time budget**.
10. Gurobi: small/medium problems with a good MIP model when you need a **proven optimal** solution. Metaheuristic: very large or hard problems, tight time limits, no MIP model, or when a good solution **quickly** is enough.
</details>

📝 **Finish your log:** score yourself on the 10 questions, rate your confidence (1–5) on each topic, and write your **3 muddiest points** (the 3 things you still find unclear). Email them to me if you want an answer.

---

## Appendix — How to change a parameter and re-run quickly

1. Edit the parameter in the cell that defines it (`AlgorithmConfig` for LS/TS, `GeneticAlgorithmConfig` for GA, the Phase 2 cell for LS neighborhoods, the GA code for the crossover).
2. **Re-run that cell** (otherwise Python keeps the old definition).
3. **Do not re-run Phase 1 (Gurobi)**: its results are kept in memory. Re-run **Phase 2 (Section 8.2)**.
4. Re-run **Section 9.1 and 9.2** to print the *Summary Statistics* table with `Avg_Gap` and `Avg_Time` per algorithm.
5. Record the row in your log **before** changing the next parameter. Change **one thing at a time**.
6. Remember that each Phase 2 run overwrites the convergence plots in `plots/`.

> ⚠️ If you restart the kernel, you must re-run Phase 1 once (Gurobi) before Phase 2.
