# Email-Aligned Inventory Presentation Implementation Plan

> **For agentic workers:** REQUIRED SUB-SKILL: Use superpowers:subagent-driven-development (recommended) or superpowers:executing-plans to implement this plan task-by-task. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** Rebuild the public inventory presentation so it contains the complete executed-notebook evidence while presenting only the four topics requested in the July 15 email.

**Architecture:** Keep the dependency-free single-page site and replace its broad seven-section narrative with four numbered sections: Environment, Simulation Parameters, Stable-Baselines3 Algorithms, and Performance. Present notebook outputs as native tables and labeled metadata, use the committed SVG as the primary result visual, and retain three clickable notebook screenshots as execution evidence.

**Tech Stack:** Semantic HTML5, embedded CSS, static PNG/SVG assets, GitHub Pages, WebKit browser verification.

---

### Task 1: Rebuild The Page Structure Around The Email

**Files:**
- Modify: `index.html`

- [x] **Step 1: Replace the navigation and first viewport**

Use exactly four navigation targets and a compact technical header:

```html
<nav aria-label="Presentation sections">
  <a href="#environment">1. Environment</a>
  <a href="#parameters">2. Parameters</a>
  <a href="#algorithms">3. Algorithms</a>
  <a href="#performance">4. Performance</a>
</nav>
<header class="hero" id="top">
  <div class="eyebrow">ORSuite inventory benchmark</div>
  <h1>Dual-Sourcing Inventory: PPO vs DQN</h1>
  <p>One product, two suppliers, stochastic demand, and a controlled Stable-Baselines3 comparison.</p>
</header>
```

Remove meeting context, assignment history, ORSuite history, and general explanations of reinforcement learning.

- [x] **Step 2: Add exactly four primary section shells**

```html
<section id="environment" aria-labelledby="environment-title">...</section>
<section id="parameters" aria-labelledby="parameters-title">...</section>
<section id="algorithms" aria-labelledby="algorithms-title">...</section>
<section id="performance" aria-labelledby="performance-title">...</section>
```

- [x] **Step 3: Update responsive CSS**

Retain the existing color variables and 6px maximum corner radius. Add stable grids for the decision flow, algorithm comparison, runtime metadata, output tables, and notebook figures. Below 680px, switch grids to one column and keep tables inside horizontal scroll wrappers.

- [x] **Step 4: Verify the structural rewrite locally**

Run `python3 -m http.server 8766 --bind 127.0.0.1`. Expected: HTTP 200 and exactly four primary sections.

### Task 2: Add The Environment And Parameter Content

**Files:**
- Modify: `index.html`

- [x] **Step 1: Implement the environment decision flow**

Render this sequence as native HTML:

```text
Observe inventory and incoming orders
→ choose [fast order, regular order]
→ due shipments arrive
→ demand is sampled from {0,1,2,3,4}
→ pay sourcing, holding, or backlog cost
```

Include action space `MultiDiscrete([5, 5])`, five conceptual observation values, reward as negative cost, and the 100-period truncation horizon.

- [x] **Step 2: Add the exact parameter table**

```text
Fast lead time                 1 period
Regular lead time              3 periods
Fast incremental cost          10 per unit
Regular incremental cost       0 per unit
Holding cost                   5 per unit-period
Backlog cost                   95 per unit-period
Demand                         discrete uniform 0 through 4
Maximum order                  4 units per supplier-period
Inventory/backlog bound        +/-1000
Episode length                 100 periods
Initial net inventory          0
```

State that units are normalized, regular cost is incremental, 0/2 delays were shifted to 1/3, and maximum order is a local action restriction.

- [x] **Step 3: Add compact benchmark attribution**

Link the DOI, arXiv preprint, and official archived instance in one source note. Use the phrase `ORSuite-compatible analogue, not a finite-horizon reproduction`.

- [x] **Step 4: Add setup/configuration execution evidence**

Show `assets/notebook-setup.png` full width, clickable, with a concise caption.

### Task 3: Add The Algorithm Setup And Controlled Comparison

**Files:**
- Modify: `index.html`

- [x] **Step 1: Add equal-weight PPO and DQN columns**

```text
PPO: policy-based, uses MultiDiscrete([5,5]) directly, clipped updates.
DQN: value-based, scores 25 complete actions, uses replay memory and a target network.
```

- [x] **Step 2: Explain the reversible DQN action wrapper**

```text
Discrete index 0  -> [0 fast, 0 regular]
Discrete index 1  -> [0 fast, 1 regular]
Discrete index 24 -> [4 fast, 4 regular]
```

State that both algorithms receive the same 25 feasible order pairs.

- [x] **Step 3: Add the experiment controls**

```text
Training seeds: 0, 1, 2
Requested budget: 50,000 timesteps per algorithm and seed
Checkpoint interval: 10,000 requested timesteps
Evaluation: 20 held-out episodes per seed and checkpoint
Episode length: 100 periods
Evaluation seed: 100,000
Training reward scale: divide by 100
Evaluation metric: original unscaled mean cost per period
```

State that training is uninterrupted and every checkpoint uses the same held-out demand stream.

- [x] **Step 4: Add algorithm execution evidence**

Show `assets/notebook-experiment-design.png` full width and clickable.

### Task 4: Add Complete Performance Outputs

**Files:**
- Modify: `index.html`
- Reuse: `assets/inventory-learning-curves.svg`
- Reuse: `assets/notebook-results-output.png`

- [x] **Step 1: Keep the committed learning curve as the primary visual**

Explain that the x-axis is actual SB3 timesteps, the y-axis is held-out mean cost per period on a log scale, lower is better, bands are plus or minus one training-seed sample standard deviation, and the green dashed line is the TBS reference.

- [x] **Step 2: Add the complete checkpoint table**

```text
PPO   10,240   2,138.702   2,749.816
PPO   20,480   7,516.058   2,088.437
PPO   30,208   5,164.829   4,182.560
PPO   40,448   1,804.643   1,432.673
PPO   50,176     216.529     279.952
DQN   10,000   1,001.716     174.420
DQN   20,000     174.992      44.662
DQN   30,000     112.031      26.060
DQN   40,000     371.508     224.872
DQN   50,000     420.887     226.614
TBS        0      33.810       n/a
```

Columns are method, actual timesteps, mean cost per period, and training-seed standard deviation.

- [x] **Step 3: Add runtime and reproducibility output**

Render Python 3.11.13, Gymnasium 1.3.0, Stable-Baselines3 2.9.0, NumPy 2.4.6, and PyTorch 2.13.0. Identify the raw CSV, summary CSV, SVG, and JSON manifest.

- [x] **Step 4: Add final output evidence and interpretation**

Show `assets/notebook-results-output.png` full width and clickable. State that PPO's lower final cost is not a universal ranking, DQN's best shown checkpoint is 30,000 timesteps, large bands show instability, and TBS is separately calibrated.

- [x] **Step 5: Integrate the notebook limitations**

Add a compact block covering synthetic demand, three seeds, no systematic tuning, one-hot observation inefficiency, no saved model weights, and single-product scope. Keep this inside Performance rather than adding a fifth primary section.

### Task 5: Verify Notebook Completeness And Presentation Quality

**Files:**
- Verify: `index.html`
- Verify: all four assets

- [x] **Step 1: Compare the page against all 12 notebook cells**

Expected: every markdown claim and each code-cell output category has a native page element or screenshot.

- [x] **Step 2: Compare values against the summary CSV**

Expected: 11 exact method/timestep/mean-cost/seed-SD rows from `inventory_algorithm_comparison_summary.csv`.

- [x] **Step 3: Run local WebKit verification**

Test 1440x900 and 390x844. Expected: HTTP 200, four topics present, four images loaded, no errors, no overflow, and no names or meeting context.

- [x] **Step 4: Inspect rendered captures**

Expected: readable screenshots, tables, graph, and no overlaps on desktop/mobile.

- [x] **Step 5: Run repository checks**

Run `git diff --check` and `git status --short`. Expected: only intended presentation and plan changes.

### Task 6: Commit, Deploy, And Verify GitHub Pages

**Files:**
- Commit: `index.html`
- Commit: this plan

- [x] **Step 1: Commit the verified presentation**

```bash
git add index.html docs/superpowers/plans/2026-07-16-email-aligned-inventory-presentation.md
git commit -m "Align inventory presentation with notebook walkthrough"
```

- [x] **Step 2: Push `main`**

Run `git push origin main`.

- [x] **Step 3: Verify deployment SHA**

Expected: the latest GitHub Pages build reports the exact `git rev-parse HEAD` SHA and status `built`.

- [x] **Step 4: Verify the public artifact**

Compare the public HTML byte-for-byte with local `index.html`, then run the same WebKit checks against the public URL.
