# BIT 2118 Scientific Computing – Group Work (Tasks 2–20)

**Student name:** Sumaya Bigenimana
**Student ID:** BSCCS202532262
**Course:** BIT 2118 Scientific Computing

This is my **individual** GitHub submission for the group work. The tasks were discussed as a group, but this repository contains my own code, documentation, screenshots and interpretations.

Task 1 is the self-paced prerequisite (*Data Science Essentials with Python*) and has no program.

---

## What I did for every task

| Step | Description |
|---|---|
| **B** | Identified the input values and the mathematical operation performed |
| **C** | Changed at least one input/parameter and explained how the result changes |
| **D** | Explained the meaning of the output in words and interpreted the graph |
| **E** | Modified the program to solve a different but related real-world case and documented the assumptions, input data, output and interpretation |

The full write-up for every task is in [`DOCUMENTATION.pdf`](DOCUMENTATION.pdf) (also available as [`DOCUMENTATION.md`](DOCUMENTATION.md)).

---

## Repository structure

```text
.
├── code/
│   ├── Task_XX_<name>.py     # original program + Part C (parameter change)
│   └── Task_XX_modified.py   # Part E (modified real-world case)
├── outputs/                  # text output of every program run
├── screenshots/              # terminal screenshots (*_terminal.png) and graphs (*_plot.png)
├── DOCUMENTATION.pdf         # full documentation (B, C, D, E for all tasks)
├── DOCUMENTATION.md
├── requirements.txt
└── README.md
```

---

## Tasks and modified real-world cases

| Task | Mathematical concept | Modified real-world case (Part E) |
|---|---|---|
| 2 | NumPy arrays and numerical computation | Greenhouse heat-stress monitoring |
| 3 | Mathematical functions and vectorization | Mobile-money fees for 1,000,000 transactions |
| 4 | Floating-point arithmetic and numerical errors | Accumulated round-off in a payments ledger |
| 5 | Matrix algebra and matrix operations | Re-positioning a farm plot (homogeneous coordinates) |
| 6 | Solution of systems of linear equations | Two-loop electrical circuit (Kirchhoff's law) |
| 7 | Gaussian elimination and LU decomposition | Leontief input–output model of an economy |
| 8 | Iterative methods for linear systems | Steady-state temperature of a heated metal rod |
| 9 | Eigenvalues and eigenvectors | PageRank of five web pages |
| 10 | Root finding: bisection and fixed-point | Bakery break-even point |
| 11 | Root finding: Newton-Raphson and secant | Effective interest rate of a digital loan |
| 12 | Polynomial and nonlinear equation solving | Pest coexistence point and profit polynomial |
| 13 | Interpolation | Thermistor calibration table |
| 14 | Curve fitting and approximation | Logistic growth curve of a maize plant |
| 15 | Numerical differentiation | Speed and acceleration from noisy GPS data |
| 16 | Numerical integration | Daily energy of a rooftop solar panel |
| 17 | ODEs – Euler method | SIR epidemic among school learners |
| 18 | Numerical solution of ODEs using SciPy | Series RLC circuit step response |
| 19 | Numerical optimization | Least-cost dairy feed mix |
| 20 | Random numbers and Monte Carlo methods | Probability that a project meets its deadline |

---

## How to run

**Requirements:** Python 3.9+ with NumPy ≥ 2.0 (Task 16 uses `np.trapezoid`), SciPy and Matplotlib.

```bash
# 1. Clone the repository
git clone https://github.com/<your-username>/<your-repo-name>.git
cd <your-repo-name>

# 2. Install the libraries
pip install -r requirements.txt

# 3. Run a task (from inside the code folder)
cd code
python Task_02_numpy_arrays.py      # original program + parameter change
python Task_02_modified.py          # modified real-world case
```

Graphs are shown with `plt.show()` and also saved automatically to the `screenshots/` folder.

---

## Notes

- The original programs from the brief were kept; I only added extra `print` lines, saved the graphs to `screenshots/`, and added a few comparison graphs.
- Data used in the modified cases (prices, tariffs, sensor and crop measurements) are illustrative assumptions and are stated in the documentation for each task.
- Timing results (Tasks 3 and 7) vary slightly from one computer to another.

---

## Libraries used

NumPy · SciPy (`linalg`, `optimize`, `interpolate`, `integrate`, `stats`) · Matplotlib · `timeit`

---

*Sumaya Bigenimana – BSCCS202532262*
