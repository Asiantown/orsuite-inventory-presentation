# Email-Aligned Inventory Presentation Design

## Objective

Replace the current broad project presentation with a concise website that Ryan can present instead of navigating the Jupyter interface. The page must contain every substantive explanation, parameter, method, output, qualification, and reproducibility detail from `inventory_algorithm_comparison.ipynb`, while its visible narrative follows only the four items requested in Professor Sinclair's July 15 email.

## Audience And Tone

The audience already understands ORSuite and assigned the work. The page must therefore avoid project history, meeting context, introductory reinforcement-learning lectures, private speaker notes, and explanations of why the assignment exists. It should read as a technical walkthrough of completed work: direct, conversational, accurate, and ready to screen-share.

## Information Architecture

The page will have four primary numbered sections matching the email:

1. **Environment**
   - High-level description of the single-product, dual-supplier environment.
   - Compact decision flow from observation to orders, arrivals, demand, and cost.
   - Exact state, action, reward/cost, backlog, horizon, and Gymnasium termination semantics needed to understand the experiment.

2. **Simulation Parameters**
   - Lead times, sourcing costs, holding and backlog costs, demand distribution, maximum order, inventory/backlog bound, episode length, and initial state.
   - Concise attribution to the Böttcher, Asikis, and Fragkos benchmark.
   - Necessary qualifications: normalized units, regular cost is incremental, 0/2 lead times are shifted to 1/3, maximum order is local, and the result is an ORSuite-compatible analogue rather than a paper reproduction.

3. **Stable-Baselines3 Algorithms**
   - PPO and DQN setup and their practical distinction.
   - The reversible DQN action wrapper from `Discrete(25)` to every `MultiDiscrete([5, 5])` order pair.
   - Fairness controls: same feasible actions, training seeds, held-out evaluation stream, reward scaling during training only, uninterrupted training, checkpoint evaluation, and actual timestep labels.
   - TBS identified only as a separately calibrated inventory-specific reference, not a third ML algorithm or reproduced paper result.

4. **Performance**
   - The committed learning-curve SVG with actual SB3 timesteps on the x-axis and mean unscaled cost per period on the y-axis.
   - Complete PPO and DQN checkpoint table plus final TBS reference.
   - Runtime versions, seed counts, evaluation episode counts, and final output values.
   - A short, honest interpretation: lower cost is better; learning is unstable; final values are not a universal algorithm ranking.

## Notebook Completeness

Every substantive notebook element must be represented natively on the page or as execution evidence:

- Setup command and pinned-environment expectation.
- Benchmark source and configuration output.
- Experiment constants and evaluation design.
- Full checkpoint summary, not only final values.
- Learning curve and final compact output.
- Limitations and next steps, integrated into the performance interpretation rather than a separate broad section.
- Runtime versions, artifact types, and reproducibility manifest explanation.
- Clickable screenshots for setup/validation, experiment design, and final output.

Raw Python dictionary dumps should be converted to readable tables or labeled value lists. Screenshots provide evidence but do not replace accessible native text.

## Visual Design

- Retain the existing restrained green, charcoal, white, and amber visual language.
- Use a compact first viewport headed `Dual-Sourcing Inventory: PPO vs DQN`; do not use a marketing hero.
- Use four navigation items matching the email request.
- Keep the learning curve as the primary visual.
- Show notebook screenshots at full content width and make them clickable for original resolution.
- Avoid cards inside cards, oversized headings, decorative imagery, and text that resembles a presenter script.
- Preserve readable layouts at desktop and mobile widths without horizontal overflow.

## Data And Source Of Truth

The website content will be derived from:

- `examples/inventory_algorithm_comparison.ipynb`
- `examples/inventory_algorithm_comparison.py`
- `examples/results/inventory_algorithm_comparison_summary.csv`
- `examples/results/inventory_algorithm_comparison_manifest.json`
- `examples/results/inventory_algorithm_comparison.svg`

The committed results are PPO `216.529` at `50,176` actual timesteps, DQN `420.887` at `50,000`, and TBS `33.810`. The complete table must include all five PPO and five DQN checkpoints.

## Verification

Before deployment:

- Compare every notebook markdown section and code-cell output against the page.
- Confirm all checkpoint values match the committed summary CSV.
- Confirm all screenshots and the learning-curve SVG load.
- Verify the four email-requested topics are present and no names, meeting notes, or assignment background appear.
- Test desktop and mobile in WebKit for errors, overflow, text clipping, and image readability.
- Verify GitHub Pages deploys the new commit and the public HTML exactly matches the local file.

## Out Of Scope

- Changing the environment, algorithms, parameters, saved results, or notebook logic.
- Running a new training experiment.
- Adding multi-product inventory, stochastic lead times, unrelated ORSuite environments, or new ML algorithms.
- Sending email or modifying the presenter guide.
