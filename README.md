# Predictive Optimization for a Wastewater Treatment Process

> A decision-support system that predicts the optimal aeration setpoint for a wastewater
> treatment process — balancing water quality against energy use, across changing seasonal
> and operating conditions.
>
> *Industrial data is confidential — this write-up focuses on the problem, methodology,
> and system design. Process specifics are generalized and raw values are omitted.*

---

## 1. Problem

In wastewater treatment, **aeration** (pumping air into the tank) drives the biological process
that cleans the water. The amount of air needed is not fixed — it shifts with season, load,
and operating conditions.

This creates a costly trade-off operators have to guess at:

- **Too much air (over-aeration)** → wasted electricity (aeration is one of the largest energy costs in treatment).
- **Too little air (under-aeration)** → degraded water quality, risk of failing discharge limits.

**Goal:** Predict the **dissolved oxygen (DO) response** to different aeration (blower) settings,
so operators can choose the lowest air volume that still keeps water quality in spec.

---

## 2. System Overview

```
Sensor + lab data
        │
        ▼
Data integration & preprocessing
        │
        ▼
Multi-scenario prediction model  ──►  predicts DO for several candidate blower settings
        │
        ▼
Interactive decision dashboard   ──►  operator compares scenarios vs. history
        │
        ▼
API on internal ML platform      ──►  accessible from the web, in operations
```

---

## 3. Data

| | |
|---|---|
| **Inputs** | Process sensor signals + structured lab measurements |
| **Target** | Dissolved oxygen (DO) concentration |
| **Control variable** | Aeration / blower volume |

**Key challenges**

- **Seasonality & regime change** — the right aeration level drifts over the year and with load.
- Fusing **continuous sensor data** with **discrete lab analysis**.

---

## 4. Approach — Multi-Scenario Prediction

Rather than outputting a single number, the model answers a **"what-if" question** operators
actually care about:

> *"If I set the blower to X, what dissolved oxygen will I get?"*

The system predicts DO across **several candidate aeration settings at once**, so the operator
can see the full trade-off curve and pick the most efficient setting that still meets the
quality target — instead of trial-and-error on a live process.

---

## 5. Decision Dashboard

A core part of the value was making the prediction **usable by operators**, not just accurate:

- Built an interactive **HTML dashboard (Plotly)**
- Lets operators **compare candidate scenarios against historical tracking data** side by side
- Turns a model output into an actual operating decision

---

## 6. Evaluation

On held-out validation data: **MAE ≈ 0.13, R² ≈ 0.56** — accurate enough to guide aeration
decisions and surface the energy-vs-quality trade-off, while being honest about the
inherent noise of a live biological process.

---

## 7. Deployment

- Deployed as an **API service on the internal ML platform**, accessible from the web
- Python modeling + JavaScript front-end for the decision dashboard
- Built to fit into the existing operations workflow, not as a standalone notebook

---

## 8. Impact

| Before | After |
|---|---|
| Aeration set by experience / guesswork | Aeration guided by predicted DO across scenarios |
| Over-aeration → wasted energy | Operators can pick the efficient setting that still meets spec |
| No way to compare "what-if" options | Side-by-side scenario comparison in a dashboard |

The result: a tool that helps **reduce energy waste from over-aeration** while protecting
water quality — a direct operating-cost lever.

---

## 9. Reflections / What I'd Do Next

- Framing the model as a **multi-scenario "what-if" tool** made it far more useful to operators
  than a single point prediction would have been.
- The **dashboard was as important as the model** — an accurate prediction nobody acts on has no value.
- Next steps I'd explore: closing the loop toward setpoint recommendation, and adding
  confidence bounds so operators know when to trust the prediction.

---

*Owned end-to-end — from problem framing through deployment and the operator-facing dashboard.*
*Stack: Python, scikit-learn, JavaScript, Plotly, Pandas, NumPy.*
