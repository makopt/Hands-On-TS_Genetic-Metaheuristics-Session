# 🧠 Hands-On TS & Genetic Metaheuristics — Self-Learning Edition

**Course:** CS616 - Optimization Algorithms  
**Instructors:** Dr. Mahdi Khemakhem & Dr. Essra Aldessouki  
**Problem used:** the 0/1 Knapsack Problem  
**Algorithms:** Local Search (LS) · **Tabu Search (TS)** · **Genetic Algorithm (GA)** · exact solver Gurobi (reference)

This session is **not a live session**: you learn **by yourself, at your own pace**, following a guided path. The notebook is already implemented — your job is to **understand it, test it, break it, and explain it**.

> 🎯 This path is also the **best preparation for Assignment 2** (Tabu Search + Genetic Algorithm for the Capacitated Facility Location Problem).

---

## 📌 1. What You Will Learn

After completing this path you will be able to:

1. **Describe** how Tabu Search escapes local optima (tabu list, tenure, aspiration criterion, diversification) and **trace it by hand** on a small instance.
2. **Describe** how a Genetic Algorithm evolves a population (selection, crossover, mutation, repair, elitism) and **apply each operator by hand**.
3. **Run controlled experiments** on algorithm parameters and **explain** the results with data (including surprising ones).
4. **Compare** LS, TS, GA and Gurobi in terms of solution quality (gap), time and convergence.
5. **Transfer** the ideas to a new problem (the CFLP of Assignment 2).

---

## 🧭 2. How the Self-Learning Path Works

Everything is described step by step in **[`Self_Learning_Guide.md`](Self_Learning_Guide.md)**. You record your work in **[`My_Learning_Log.md`](My_Learning_Log.md)**.

Each module follows the same learning cycle:

| Step | What you do |
|------|-------------|
| 📖 **Read** | Study the indicated notebook sections (do not skip the markdown cells!) |
| ✍️ **Try on paper** | Solve a small hand exercise **before** looking at the code results |
| 🔮 **Predict** | Write in your log what you expect to happen **before** you run an experiment |
| ▶️ **Run** | Run the notebook / experiment and record the numbers |
| 💬 **Explain** | Explain in your own words **why** you observed it (especially if it differs from your prediction) |
| ✅ **Check** | Answer the self-check questions, then open the hidden answers to verify |

### ⏱️ Suggested plan (about 5 hours in 4–5 sittings)

| Module | Topic | Time |
|--------|-------|------|
| **0** | Get ready: setup + smoke test | 30 min |
| **1** | Knapsack, neighborhoods and Local Search | 60 min |
| **2** | Tabu Search | 60 min |
| **3** | Genetic Algorithm | 60 min |
| **4** | Compare, conclude, choose | 45 min |
| **5** | Transfer to Assignment 2 (CFLP) + final self-assessment | 45 min |

📅 **Finish at least one week before the Assignment 2 deadline (December 05, 2026).**

### 📜 Ground rules
- Work **individually** (you may discuss *concepts* with classmates, but do your own experiments and write your own explanations).
- **Predict before you run.** A prediction that turns out wrong is the best learning moment — keep it in your log.
- AI tools may be used to **understand** a concept, never to replace your own experiments or your own explanations.
- Be honest in the self-check: look at the answer **only after** you have written yours.

---

## ⚙️ 3. Setup (Module 0)

Complete these steps **before** starting Module 1 (if you attended the Gurobi session, you already have most of them).

1. **Fork and clone** this repository: https://github.com/makopt/Hands-On-TS_Genetic-Metaheuristics-Session  
   ```bash
   git clone https://github.com/<your-username>/Hands-On-TS_Genetic-Metaheuristics-Session.git
   ```
2. **Python** 3.13.7 or above recommended: https://www.python.org/downloads/
3. **Libraries:**
   ```bash
   pip install gurobipy numpy pandas matplotlib openpyxl tqdm
   ```
   (`tqdm` is optional: it only adds progress bars.)
4. **Gurobi license** (Academic or Learner): https://www.gurobi.com/academia/academic-program-and-licenses/  
   ⚠️ A **Learner (size-limited) license** accepts models up to 2,000 variables: with it, set `max_instances = 30` (instances with at most 2,000 items) instead of 50.
5. **Editor:** Visual Studio Code with the *Python* and *Jupyter* extensions.
6. **Smoke test:** open `Knapsack_Tabu_Genetic.ipynb`, set `max_instances = 3` in Section 1.2, and run all cells.  
   ✅ You should see, in a few seconds, 3 convergence plots in `plots/`, two Excel files in `statistics/` and `results/`, and the message *"TESTING COMPLETE"*.

> On the first 3 instances all algorithms reach a 0.00% gap: this is normal (the greedy start is already optimal). The interesting instances come later — the guide tells you which ones to use.

---

## 📁 4. Repository Contents

| Item | Description |
|------|-------------|
| 📓 `Knapsack_Tabu_Genetic.ipynb` | The notebook (fully implemented): LS, TS, GA, Gurobi, experiments and plots |
| 📘 `Self_Learning_Guide.md` | **Your learning path** (modules, hand exercises, experiments, self-checks) |
| 📝 `My_Learning_Log.md` | **Your log** (predictions, results, explanations) — fill it in as you go |
| 📁 `kp_instances/` | 50 knapsack instances (100 to 5,000 items, 5 per size) |
| 📁 `plots/`, `statistics/`, `results/` | Output folders created by the notebook (convergence plots, Excel statistics, summary plots) |

---

## 🚀 5. Three Run Sizes (Remember Them)

Change `max_instances` in **Section 1.2** of the notebook:

| `max_instances` | Use it for | Approx. time |
|-----------------|------------|--------------|
| `3` | Smoke test | a few seconds |
| `16` | **Experiments** (includes the first instances where heuristics differ) | about 1 minute |
| `50` | Final full comparison (Module 4) | about 5 minutes |

> ⏱️ Times are indicative (they depend on your machine). If you only changed algorithm parameters, you do **not** need to re-run Gurobi (Phase 1): re-run **Phase 2** (Section 8.2) and then Section 9.2 to see the summary table.

---

## 🆘 6. Getting Help

- Stuck for more than 15 minutes on a **setup** problem? Contact me by email **with the full error message and a screenshot**.
- For **concept** questions: first re-read the markdown cell of the notebook and your log, then write your question as a *muddiest point* at the end of `My_Learning_Log.md` and email it to me — a precise question is answered faster.

Good luck, and enjoy the experiments! 🚀
