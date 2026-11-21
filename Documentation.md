# BIT 2118 Scientific Computing – Group Work Documentation (Tasks 2–20)

**Student:** Sumaya Bigenimana  |  **Student ID:** BSCCS202532262


## How to read this document

For every task the sections follow the instructions of the group work:

* **B** – input values and the mathematical operation performed;
* **C** – a parameter was changed and the change in the result is explained;
* **D** – meaning of the output in words and interpretation of the graph;
* **E** – the program was modified to solve a different but related real-world case (assumptions, input data, output and interpretation).

All code is in `code/` (each `Task_XX_*.py` file contains the original program followed by the parameter-change experiment, and `Task_XX_modified.py` is the new case). Screenshots of the terminal output and the graphs are in `screenshots/`. All numbers quoted below come from the actual program output. Timing results (Tasks 3, 7) vary slightly from computer to computer. Task 1 is the self-paced prerequisite and has no program.



# Task 2: NumPy Arrays and Numerical Computation

**Code:** `code/Task_02_numpy_arrays.py` (original + parameter change) and `code/Task_02_modified.py` (modified case)

## B. Input values and mathematical operation

**Input values:** a NumPy array of 24 hourly temperatures in °C (from 17.5 °C at 03:00 to 29.4 °C at 12:00).

**Mathematical operations:** mean $\bar T=\frac{1}{24}\sum T_i$, minimum, maximum, and the vectorised conversion $F=1.8C+32$ applied to all 24 values in one expression (no loop).

### Original program output

![Task 2: terminal output of the original program and the parameter change](screenshots/Task_02_numpy_arrays_terminal.png)


## C. Parameter change

**Change made:** every reading was increased by 5 °C (a simulated heat wave).

**Result:** mean 23.12 → **28.12 °C**, minimum 17.50 → 22.50 °C, maximum 29.40 → 34.40 °C. The mean, minimum and maximum all shifted by exactly +5 °C, and the maximum in °F rose by +9 °F (because $1.8\times5=9$). Adding a constant moves every statistic by that constant; the Fahrenheit conversion multiplies the size of the shift by 1.8.

## D. Meaning of the output and graph

The daily mean is 23.12 °C, the coolest hour is 17.5 °C and the hottest is 29.4 °C, so the day swings about 12 °C. The first six Fahrenheit values (65.3 ... 66.2 °F) are the early-morning readings converted to °F. There is no graph in the original program.

## E. Modified case: Greenhouse heat-stress monitoring (tomato farm, Thika)

**Assumptions:** 24 hourly greenhouse temperatures; tomatoes suffer heat stress above 30 °C. Readings are illustrative.

**Input data:** Hourly array from 15.0 °C (03:00) to 37.5 °C (12:00); threshold = 30 °C.

**Output:** Mean 24.61 °C; maximum 37.50 °C at 12:00; **8 hours above 30 °C (09:00–16:00)**; mean temperature during stress hours 34.62 °C; heat-stress degree-hours 37.0 °C·h; first Kelvin values 289.15, 288.65, 288.35, 288.15 K.

![Task 2: terminal output of the modified program](screenshots/Task_02_modified_terminal.png)

![Task 2: graph from the modified program](screenshots/Task_02_modified_plot.png)

**Interpretation:** The boolean mask `greenhouse_c > threshold` tests all 24 values at once. The crop is under stress for 8 hours a day and exceeds the threshold by 7.5 °C at the peak, so shading or ventilation is needed between 09:00 and 16:00. The red shaded area in the graph is the degree-hours (37.0 °C·h).


# Task 3: Mathematical Functions and Vectorization

**Code:** `code/Task_03_vectorization.py` (original + parameter change) and `code/Task_03_modified.py` (modified case)

## B. Input values and mathematical operation

**Input values:** 1,000,000 random latency readings between 5 and 250 ms (seed 42).

**Mathematical operations:** $s=\text{ms}/1000$ then $s^2$, done once with a Python `for` loop and once with a vectorised NumPy expression; `timeit` measures both.

### Original program output

![Task 3: terminal output of the original program and the parameter change](screenshots/Task_03_vectorization_terminal.png)


## C. Parameter change

**Change made:** the number of measurements $N$ was changed to 10,000, 100,000 and 1,000,000.

**Result:** the loop time grew roughly in proportion to $N$ (0.0012 s, 0.0133 s, 0.146 s) while the vectorised time stayed in the microsecond to millisecond range. The speed-up was between about 34× and 113× in every case. The speed-up figures vary between runs because timing is affected by the CPU cache and other programs, but vectorisation was always more than 30× faster.

## D. Meaning of the output and graph

The first five squared latencies (e.g. 0.0379 s²) are the latencies converted to seconds and squared. The loop took about 0.15 s and the vectorised code about 0.003 s, i.e. roughly **50× faster** for identical results. Vectorised code runs the loop inside compiled C code instead of the Python interpreter.

## E. Modified case: Mobile-money transaction fees for 1,000,000 payments

**Assumptions:** Illustrative (not real) tariff: amount ≤ 100 → fee 0; ≤ 1,000 → 10; ≤ 10,000 → 30; above → 60 KES. Excise duty is 15 % of the fee. Amounts follow a log-normal distribution (many small payments, a few large).

**Input data:** 1,000,000 amounts; median 403.49 KES, maximum 152,885.68 KES.

**Output:** Total fees + excise = **15,384,355.00 KES** from both the loop and the vectorised version; loop 0.120 s, vectorised 0.0142 s (**about 8× faster**); 12.23 % of transactions pay no fee.

![Task 3: terminal output of the modified program](screenshots/Task_03_modified_terminal.png)


**Interpretation:** Identical totals prove that the vectorised `np.select` is correct. The speed-up (8×) is smaller than in the simple arithmetic case because the tier test has to be evaluated for every row, but it is still a clear gain for a million records.


# Task 4: Floating-Point Arithmetic and Numerical Errors

**Code:** `code/Task_04_numerical_errors.py` (original + parameter change) and `code/Task_04_modified.py` (modified case)

## B. Input values and mathematical operation

**Input values:** $0.1+0.2$ versus $0.3$; $\sqrt2$ versus the approximation 1.414; $x=10^{-4},10^{-8},10^{-12}$.

**Mathematical operations:** exact equality test versus `np.isclose`; absolute error $|true-approx|$; relative error $=\text{abs}/|true|$; and two algebraically identical formulas $\frac{\sqrt{1+x}-1}{x}=\frac{1}{\sqrt{1+x}+1}$.

### Original program output

![Task 4: terminal output of the original program and the parameter change](screenshots/Task_04_numerical_errors_terminal.png)


## C. Parameter change

**Change 1:** approximation 1.414 → 1.41421. Absolute error fell from 2.1356×10⁻⁴ to 3.56×10⁻⁶ and relative error from 0.0151 % to 0.000252 % (about 60× smaller) because more correct digits are kept.

**Change 2:** smaller $x$ (10⁻¹⁴, 10⁻¹⁵, 10⁻¹⁶). The unstable formula's error grew from 2.30 % to 11.18 % and finally to 100 % (it returned 0) while the stable formula stayed at 0.5.

## D. Meaning of the output and graph

`0.1 + 0.2` prints 0.30000000000000004 so `==` gives False but `np.isclose` gives True: computers store 0.1 and 0.2 only approximately in binary. In the last loop the "direct" formula loses accuracy as $x$ shrinks (0.500044 instead of 0.5 at $x=10^{-12}$) because $\sqrt{1+x}$ is almost 1 and subtracting 1 cancels most of the significant digits (**catastrophic cancellation**). The rewritten stable formula has no subtraction of nearly equal numbers. At $x=10^{-16}$, $1+x$ is rounded to exactly 1, so the direct result is 0.

## E. Modified case: Accumulated round-off in a payments ledger

**Assumptions:** A system adds a 0.10 KES micro-fee 10,000,000 times; the exact total is 1,000,000.00 KES. (To keep the run short, the naive Python loop adds 1,000,000 times and the result is multiplied by 10.)

**Input data:** Fee 0.10 repeated 10 million times; summed with a naive loop, `np.sum`, `math.fsum` and `np.float32`.

**Output:** Naive loop error 1.3×10⁻⁵ KES; `np.sum` (pairwise) error 0; `math.fsum` error 0; **float32 total 1,000,000.125 (error 0.125 KES)**. `0.1+0.2 == 0.30` is False but integer cents `10 + 20 == 30` is True.

![Task 4: terminal output of the modified program](screenshots/Task_04_modified_terminal.png)


**Interpretation:** Small errors accumulate when millions of additions are made, and lower precision (float32) makes it worse. Financial software should use integer cents or `Decimal`, accurate summation (`fsum`) and tolerance-based comparison.


# Task 5: Matrix Algebra and Matrix Operations

**Code:** `code/Task_05_matrix_algebra.py` (original + parameter change) and `code/Task_05_modified.py` (modified case)

## B. Input values and mathematical operation

**Input values:** the four corners of a unit square, rotation angle 45°, scale factors (2, 1.5).

**Mathematical operations:** rotation matrix $R=\begin{bmatrix}\cos\theta&-\sin\theta\\ \sin\theta&\cos\theta\end{bmatrix}$, scaling matrix $S=\text{diag}(2,1.5)$, combined $T=RS$ (matrix product), points transformed by $P T^{T}$, and the determinant $\det T$.

### Original program output

![Task 5: terminal output of the original program and the parameter change](screenshots/Task_05_matrix_algebra_terminal.png)

![Task 5: graph from the original program](screenshots/Task_05_original_plot.png)

## C. Parameter change

**Change made:** angle 45° → 90° and scale (2, 1.5) → (3, 3).

**Result:** $T=\begin{bmatrix}0&-3\\3&0\end{bmatrix}$, the square's corners moved to (0,0), (0,3), (−3,3), (−3,0) and the determinant changed from 3.0 to **9.0**. The determinant is the area scale factor ($3\times3=9$); rotation does not change area.

## D. Meaning of the output and graph

The transformed points (0,0), (1.414,1.414), (0.354,2.475), (−1.061,1.061) form a stretched and tilted parallelogram. The determinant 3.0 equals $2\times1.5$, so the new shape has three times the area of the unit square. The graph shows the blue original square and the orange scaled-and-rotated shape.

## E. Modified case: Re-positioning a farm plot on a map (homogeneous coordinates)

**Assumptions:** The plot is a 40 m × 25 m rectangle. On the map it must be drawn at scale 0.5, rotated 30°, and moved to position (100, 60). A 3×3 matrix $M=T_{translate}R S$ combines all three steps.

**Input data:** Corners (0,0), (40,0), (40,25), (0,25).

**Output:** New corners (100,60), (117.321,70), (111.071,80.825), (93.75,70.825). Area on the map **250 m²** (= 1000 × 0.5² as expected); determinant of the 2×2 part 0.25; the inverse matrix returns the original corners with an error of about 3×10⁻¹⁴.

![Task 5: terminal output of the modified program](screenshots/Task_05_modified_terminal.png)

![Task 5: graph from the modified program](screenshots/Task_05_modified_plot.png)

**Interpretation:** One matrix performs all three operations on every point consistently. Translation needs an extra coordinate (the column of ones). The determinant 0.25 confirms that halving both lengths cuts the area to a quarter, and the successful inverse shows that the transformation can be undone.


# Task 6: Solution of Systems of Linear Equations

**Code:** `code/Task_06_linear_systems.py` (original + parameter change) and `code/Task_06_modified.py` (modified case)

## B. Input values and mathematical operation

**Input values:** $A=\begin{bmatrix}4&2\\8&12\end{bmatrix}$ (CPU and memory use per server), $b=[20,72]$ (observed totals).

**Mathematical operation:** solve $Ax=b$ with `np.linalg.solve` and verify with the residual $b-Ax$.

### Original program output

![Task 6: terminal output of the original program and the parameter change](screenshots/Task_06_linear_systems_terminal.png)


## C. Parameter change

**Change 1:** totals 20/72 → 24 CPU and 80 GB. Solution changes from (3, 4) to **(4, 4)** servers.

**Change 2:** totals 28 CPU and 96 GB. The solution is (4.5, 5), a fractional number of servers, so the measurements cannot come from whole servers and the data must be checked. The condition number 6.98 shows the system itself is well conditioned (small errors in $b$ do not get magnified much).

## D. Meaning of the output and graph

There are **3 Type A and 4 Type B servers**. The residual is exactly 0, meaning that $Ax$ reproduces the observed totals ($3\cdot4+4\cdot2=20$ CPU and $3\cdot8+4\cdot12=72$ GB).

## E. Modified case: Two-loop electrical circuit (Kirchhoff's voltage law)

**Assumptions:** 12 V source with R1 = 4 Ω in loop 1, 6 V opposing source with R2 = 3 Ω in loop 2, shared resistor R3 = 2 Ω. Equations: $6I_1-2I_2=12$ and $-2I_1+5I_2=-6$.

**Input data:** $A=[[6,-2],[-2,5]]$, $b=[12,-6]$.

**Output:** $I_1=1.8462$ A, $I_2=-0.4615$ A, current through R3 $=2.3077$ A. Power: R1 13.633 W, R2 0.639 W, R3 10.651 W, total **24.923 W**, equal to the power delivered by the sources.

![Task 6: terminal output of the modified program](screenshots/Task_06_modified_terminal.png)


**Interpretation:** The negative $I_2$ means loop 2's current actually flows opposite to the direction assumed. Equal power in and out (energy conservation) confirms that the solution is physically correct.


# Task 7: Gaussian Elimination and LU Decomposition

**Code:** `code/Task_07_gaussian_lu.py` (original + parameter change) and `code/Task_07_modified.py` (modified case)

## B. Input values and mathematical operation

**Input values:** a 3×3 matrix $A$ (diagonally dominant) and three measurement vectors $b$.

**Mathematical operation:** factor $PA=LU$ once with `lu_factor`, then for each $b$ do forward and back substitution with `lu_solve`.

### Original program output

![Task 7: terminal output of the original program and the parameter change](screenshots/Task_07_gaussian_lu_terminal.png)


## C. Parameter change

**Change made:** the system size was increased from 3×3 to **800×800** with **300** right-hand sides.

**Result:** 300 full solves took 3.073 s but one LU factorisation plus 300 cheap `lu_solve` calls took 0.082 s, a **37.5× speed-up**. Factoring costs about $n^3$ operations, whereas each substitution costs only about $n^2$.

## D. Meaning of the output and graph

Solutions: set 1 = (5, 5, 5), set 2 = (5.317, 5.268, 4.756), set 3 = (4.902, 5.610, 5.537). Residual norms are about 10⁻¹⁵, i.e. only round-off. The direct solve gives the same answer as LU.

## E. Modified case: Leontief input–output model of a three-sector economy

**Assumptions:** Sectors: Agriculture, Manufacturing, Services. $C_{ij}$ is the amount of sector $i$ needed to make 1 unit of sector $j$. Total output $x$ satisfies $(I-C)x=d$ where $d$ is the final demand. The coefficients are illustrative.

**Input data:** $C=[[.20,.10,.05],[.15,.25,.20],[.10,.15,.10]]$; four demand scenarios, e.g. baseline $d=(100,150,200)$.

**Output:** Baseline output (182.85, 315.26, 295.08), total 793.19; export boom (202.52, 459.52, 321.31); tourism growth (196.77, 355.81, 436.72); drought (130.79, 302.75, 287.21). Output multiplier (baseline) 1.763.

![Task 7: terminal output of the modified program](screenshots/Task_07_modified_terminal.png)


**Interpretation:** The matrix is factored once and reused for every scenario. Total output (793) is far more than the final demand (450) because sectors buy from each other. An export boom in manufacturing also raises agriculture and services output, and a drought lowers all three sectors.


# Task 8: Iterative Methods for Linear Systems

**Code:** `code/Task_08_iterative_linear.py` (original + parameter change) and `code/Task_08_modified.py` (modified case)

## B. Input values and mathematical operation

**Input values:** a 5×5 tridiagonal matrix ($2$ on the diagonal, $-1$ beside it), boundary vector $b=[100,0,0,0,20]$, tolerance $10^{-8}$.

**Mathematical operations:** Jacobi $x^{(k+1)}=D^{-1}(b-Rx^{(k)})$ and Gauss–Seidel (uses the newest values immediately); the stopping test is the maximum change between iterations.

### Original program output

![Task 8: terminal output of the original program and the parameter change](screenshots/Task_08_iterative_linear_terminal.png)

![Task 8: graph from the original program](screenshots/Task_08_original_plot.png)

## C. Parameter change

**Change 1:** tolerance $10^{-8}$ → $10^{-3}$. Iterations fell from 150 to **70** (Jacobi) and from 75 to **35** (Gauss–Seidel), and the real error rose to 0.0034 and 0.0029 °C. A looser tolerance saves work but gives a less accurate answer (the real error can exceed the tolerance because only the change between steps is tested).

**Change 2:** both boundaries 60 °C gives a flat profile of 60 °C at every point, as physics predicts.

## D. Meaning of the output and graph

Both methods give temperatures 86.67, 73.33, 60.00, 46.67, 33.33 °C: a straight-line drop between the 100 °C and 20 °C ends, identical to the direct solution. In the graph (log scale) both lines are straight, meaning the error shrinks by a constant factor each iteration; Gauss–Seidel (orange) needs **half as many iterations** (75 versus 150).

## E. Modified case: Steady-state temperature of a heated metal rod

**Assumptions:** One end held at 150 °C (furnace), the other at 25 °C (room), no internal heat source, 10 interior nodes; finite-difference equations $-T_{i-1}+2T_i-T_{i+1}=0$.

**Input data:** $A$ = 10×10 tridiagonal matrix, $b=[150,0,...,0,25]$, tolerance $10^{-6}$.

**Output:** Jacobi 385 iterations, Gauss–Seidel 194; Gauss–Seidel error against the exact solution 1.09×10⁻⁵ °C. Temperatures from 138.64 °C down to 36.36 °C, dropping 11.364 °C per node.

![Task 8: terminal output of the modified program](screenshots/Task_08_modified_terminal.png)

![Task 8: graph from the modified program](screenshots/Task_08_modified_plot.png)

**Interpretation:** The temperature falls linearly along the rod, as expected for steady conduction. With more nodes both methods need more iterations (385 versus 150 earlier), but Gauss–Seidel still needs about half. For very large sparse systems, iterative methods avoid the cost of factorisation.


# Task 9: Eigenvalues and Eigenvectors

**Code:** `code/Task_09_eigenvalues.py` (original + parameter change) and `code/Task_09_modified.py` (modified case)

## B. Input values and mathematical operation

**Input values:** the adjacency matrix of a 4-node undirected network (A–B, A–C, B–C, B–D, C–D).

**Mathematical operation:** solve $Av=\lambda v$ with `np.linalg.eig`; take the largest eigenvalue and normalise its eigenvector so the scores add up to 1 (eigenvector centrality).

### Original program output

![Task 9: terminal output of the original program and the parameter change](screenshots/Task_09_eigenvalues_terminal.png)

![Task 9: graph from the original program](screenshots/Task_09_original_plot.png)

## C. Parameter change

**Change made:** a new link A–D was added so every node is connected to every other node.

**Result:** the dominant eigenvalue rose from 2.5616 to **3.0** and every node's importance became **0.25** (all nodes are now equivalent).

## D. Meaning of the output and graph

Eigenvalues are 2.5616, 0, −1.5616, −1. B and C score **0.2808** (they have three links each) and A and D score 0.2192 (two links). The verification error of $10^{-15}$ shows that $Av=\lambda v$ holds. The bar chart displays these importance scores.

## E. Modified case: Simplified Google PageRank for five web pages

**Assumptions:** Links: Home→News, Shop; News→Home, Blog; Shop→Home; Blog→Home, News, Shop; About→Home. Damping factor $d=0.85$; Google matrix $G=dM+\frac{1-d}{n}\mathbf{1}\mathbf{1}^T$.

**Input data:** 5×5 column-stochastic link matrix $M$.

**Output:** Power iteration converged in 106 iterations; dominant eigenvalue of $G$ is 1.0. PageRank: **Home 0.3840**, News 0.2293, Shop 0.2293, Blog 0.1275, About 0.0300. The power iteration and `eig` results agree.

![Task 9: terminal output of the modified program](screenshots/Task_09_modified_terminal.png)

![Task 9: graph from the modified program](screenshots/Task_09_modified_plot.png)

**Interpretation:** Home is the most important page because almost every page links to it. About has no incoming links, so it only receives the small random-jump share (0.15/5 = 0.03). The eigenvalue 1 means the rank vector is a steady state of the web surfer.


# Task 10: Root Finding: Bisection and Fixed-Point Methods

**Code:** `code/Task_10_bisection_fixed_point.py` (original + parameter change) and `code/Task_10_modified.py` (modified case)

## B. Input values and mathematical operation

**Input values:** $f(T)=T-25-10e^{-T/20}$, bracket [20, 40] ($f(20)<0$, $f(40)>0$), tolerance $10^{-10}$, fixed-point start $T_0=30$.

**Mathematical operations:** bisection halves the interval each step; fixed-point iteration repeats $T_{n+1}=25+10e^{-T_n/20}$.

### Original program output

![Task 10: terminal output of the original program and the parameter change](screenshots/Task_10_bisection_fixed_point_terminal.png)

![Task 10: graph from the original program](screenshots/Task_10_original_plot.png)

## C. Parameter change

**Change 1:** tolerance $10^{-10}$ → $10^{-3}$. Bisection stopped after **15** iterations instead of 38, with an error of only 1.9×10⁻⁴ °C.

**Change 2:** ambient temperature 25 → 35 °C. The equilibrium rose from 27.5252 to **36.6038 °C** (+9.08 °C, slightly less than +10 because the exponential term shrinks as $T$ increases).

## D. Meaning of the output and graph

The equilibrium temperature is **27.525211 °C**. Bisection needed 38 iterations; fixed-point iteration needed only 13 because $|g'(T)|\approx0.13<1$, so it converges quickly. The graph shows $f(T)$ crossing zero at about 27.5 °C (the dashed line).

## E. Modified case: Break-even point of a bakery

**Assumptions:** Price per loaf $=60-0.004q$ KES, revenue $R=q(60-0.004q)$, cost $C=15000+28q$ per day. Break-even means $R-C=0$.

**Input data:** Brackets [100, 1000] and [7000, 8000]; fixed-point form $q=(15000+0.004q^2)/32$.

**Output:** Lower break-even **500 loaves/day** (bisection 37 iterations); upper break-even 7,500 loaves/day; fixed-point also converges to 500 in 15 iterations; profit at the root 0.000000 KES.

![Task 10: terminal output of the modified program](screenshots/Task_10_modified_terminal.png)

![Task 10: graph from the modified program](screenshots/Task_10_modified_plot.png)

**Interpretation:** The bakery makes a profit only between 500 and 7,500 loaves a day. Below 500 the fixed cost is not covered; above 7,500 the falling price (the model's assumption) wipes out the margin. In the graph the revenue and cost curves cross at those two points.


# Task 11: Root Finding: Newton-Raphson and Secant Methods

**Code:** `code/Task_11_newton_secant.py` (original + parameter change) and `code/Task_11_modified.py` (modified case)

## B. Input values and mathematical operation

**Input values:** cash flows (−10 000, 3 000, 3 500, 4 000, 2 500), start $r_0=0.10$ (Newton) and 0.05, 0.20 (secant).

**Mathematical operation:** solve $NPV(r)=\sum CF_t/(1+r)^t=0$ with Newton $r_{n+1}=r_n-NPV/NPV'$ and with the derivative-free secant method.

### Original program output

![Task 11: terminal output of the original program and the parameter change](screenshots/Task_11_newton_secant_terminal.png)


## C. Parameter change

**Change 1:** Newton start 0.10 → 0.90. Newton converged to **−154.41 %**, a different, meaningless root (a rate below −100 %). A poor starting guess can lead to the wrong root.

**Change 2:** initial investment 10 000 → 12 000. The IRR fell from 11.54 % to **3.33 %** because the same inflows are earned on a larger investment.

## D. Meaning of the output and graph

The internal rate of return is **11.542461 %**; Newton took 4 iterations and secant 7. NPV at the solution is 0, so discounting the cash flows at 11.54 % exactly recovers the investment.

## E. Modified case: Effective interest rate of a digital loan

**Assumptions:** The borrower receives 50,000 KES and repays 4,800 KES at the end of each month for 12 months. Find the monthly rate with $50000=\sum_{t=1}^{12}4800/(1+r)^t$.

**Input data:** P = 50,000; payment = 4,800; n = 12.

**Output:** Monthly rate **2.247028 %** (Newton 4 iterations, secant 6, bisection 46); nominal annual 26.96 %; effective annual rate **30.56 %**; total interest 7,600 KES (15.2 % of the principal).

![Task 11: terminal output of the modified program](screenshots/Task_11_modified_terminal.png)


**Interpretation:** Although the total interest looks like only 15.2 %, the true cost is about 30.6 % per year because the borrower repays part of the money every month. Newton and secant are much faster than bisection (4–6 versus 46 iterations).


# Task 12: Polynomial and Nonlinear Equation Solving

**Code:** `code/Task_12_nonlinear_equations.py` (original + parameter change) and `code/Task_12_modified.py` (modified case)

## B. Input values and mathematical operation

**Input values:** circle $x^2+y^2=4$, parabola $y=x^2-1$, initial guesses (1, 1) and (−1, 1).

**Mathematical operation:** `scipy.optimize.root` finds $(x,y)$ that makes both residuals zero.

### Original program output

![Task 12: terminal output of the original program and the parameter change](screenshots/Task_12_nonlinear_equations_terminal.png)

![Task 12: graph from the original program](screenshots/Task_12_original_plot.png)

## C. Parameter change

**Change 1:** radius 2 → 1.5. The intersections moved to (±1.3133, 0.7247).

**Change 2:** a poor initial guess (0, −2) on the original system. The solver reported `Converged: False`. At $x=0$ both equations have zero slope in $x$, so Newton-type steps cannot move, and the guess is also far from a solution.

## D. Meaning of the output and graph

The intersections are **(1.5175, 1.3028)** and **(−1.5175, 1.3028)** with residuals about 10⁻¹³. Substituting $x^2=y+1$ gives $y^2+y-3=0$, so $y=1.3028$ or $-2.3028$; the second value needs $x^2<0$, so the circle and parabola meet at only two points. The graph shows the two black dots where the curves cross.

## E. Modified case: Coexistence of two competing pests and a polynomial profit equation

**Assumptions:** Competition model (thousands per hectare): $\dot x=x(1-x-0.5y)$, $\dot y=y(0.8-y-0.3x)$. Operating points are where both rates are zero. Polynomial: profit $P(q)=-q^3+9q^2-18q+4$ ($q$ in hundreds of units).

**Input data:** Initial guesses (0.1,0.1), (1,0), (0,0.8), (0.9,0.5), (2,2); coefficients [−1, 9, −18, 4].

**Output:** Equilibria: (0,0), (1,0), (0,0.8) and the coexistence point **(0.7059, 0.5882)**, which equals the exact linear solution. `np.roots` gives break-even quantities 0.2534, 2.5451 and 6.2015 (hundreds of units).

![Task 12: terminal output of the modified program](screenshots/Task_12_modified_terminal.png)


**Interpretation:** Both pests persist together at 0.71 and 0.59, each below its single-species level (1.0 and 0.8) because of competition. For the profit cubic, profit is positive for $q<25$ units, negative between 25 and 255 units, and positive again between 255 and 620 units; zero at the three roots.


# Task 13: Interpolation

**Code:** `code/Task_13_interpolation.py` (original + parameter change) and `code/Task_13_modified.py` (modified case)

## B. Input values and mathematical operation

**Input values:** hourly temperatures at 08:00–14:00 (18.0 ... 25.0 °C); target times every 10 minutes and 10:30.

**Mathematical operation:** `interp1d` joins points with straight lines; `CubicSpline` joins them with smooth cubic pieces.

### Original program output

![Task 13: terminal output of the original program and the parameter change](screenshots/Task_13_interpolation_terminal.png)

![Task 13: graph from the original program](screenshots/Task_13_original_plot.png)

## C. Parameter change

**Change made:** the 12:00 reading was changed from 25.0 to 30.0 °C (a faulty-sensor spike).

**Result:** linear interpolation changed only locally (at 12:30 it gave 27.75 °C and 10:30 stayed 23.25 °C). The cubic spline changed everywhere: 10:30 moved from 23.36 to 22.74 °C, 12:30 gave 28.75 °C, the curve overshoots to 30.06 °C and wiggles below the data after 13:00. One bad point disturbs the whole spline.

![Task 13: graph after the parameter change](screenshots/Task_13_param_change_plot.png)

## D. Meaning of the output and graph

At 10:30 the linear estimate is 23.25 °C and the cubic one 23.36 °C: the two differ by only 0.11 °C because the data are smooth. The graph shows the straight-line and spline curves almost on top of each other, passing through every measured point.

## E. Modified case: Thermistor calibration table (voltage → temperature)

**Assumptions:** A calibration table gives temperature at 8 voltages; new readings fall between rows. Accuracy is tested by leaving one known point out and predicting it.

**Input data:** Voltages 0.50–2.60 V and temperatures 85 → 12 °C.

**Output:** Estimates at 0.95, 1.25, 1.85, 2.45 V: linear 59.00, 46.50, 28.75, 15.25 °C; cubic 58.53, 46.15, 28.64, 15.34 °C. Leave-one-out errors: cubic 0.21–0.44 °C, linear 0.50–2.50 °C (except at 2.30 V where linear was exact and cubic 0.35 °C).

![Task 13: terminal output of the modified program](screenshots/Task_13_modified_terminal.png)

![Task 13: graph from the modified program](screenshots/Task_13_modified_plot.png)

**Interpretation:** The sensor response is curved, so the cubic spline is generally more accurate than straight lines (errors up to 6 times smaller). Linear interpolation is safer when data are noisy or have jumps.


# Task 14: Curve Fitting and Approximation

**Code:** `code/Task_14_curve_fitting.py` (original + parameter change) and `code/Task_14_modified.py` (modified case)

## B. Input values and mathematical operation

**Input values:** 7 measurements of users (50–350) and response time (82–305 ms); quadratic model $y=ax^2+bx+c$.

**Mathematical operation:** least squares (`curve_fit`) chooses $a,b,c$ to minimise $\sum(y_i-\hat y_i)^2$; RMSE measures the fit.

### Original program output

![Task 14: terminal output of the original program and the parameter change](screenshots/Task_14_curve_fitting_terminal.png)

![Task 14: graph from the original program](screenshots/Task_14_original_plot.png)

## C. Parameter change

**Change made:** polynomial degree 1, 2 and 3.

**Result:** RMSE 18.83 ms (line), 1.55 ms (quadratic) and 0.14 ms (cubic); the prediction at 400 users was 312.9, 377.9 and 387.9 ms. A straight line is clearly too simple. The cubic fits best on the data but has 4 parameters for 7 points and may be over-fitting, so its extrapolation should be treated with caution.

## D. Meaning of the output and graph

The fitted curve is $y=0.002167x^2-0.1345x+85.00$ with RMSE 1.55 ms, so the model is typically within about 1.5 ms of the data. Response time rises faster at heavy load (the curve bends upward) and the model predicts about **378 ms at 400 users**. The residual plot shows small errors scattered on both sides of zero.

## E. Modified case: Logistic growth curve of a maize plant

**Assumptions:** Height follows $h(t)=L/(1+e^{-k(t-t_0)})$ where $L$ is the maximum height. The measurements are illustrative.

**Input data:** Days 10–90 and heights 6.0–225.5 cm.

**Output:** $L=225.8\pm1.1$ cm, $k=0.1043\pm0.0020$/day, $t_0=41.7\pm0.2$ days; RMSE 1.28 cm, $R^2=0.9998$; predicted height 132.3 cm on day 45 and 225.3 cm on day 100; fastest growth 5.89 cm/day. A straight line has RMSE 20.52 cm.

![Task 14: terminal output of the modified program](screenshots/Task_14_modified_terminal.png)

![Task 14: graph from the modified program](screenshots/Task_14_modified_plot.png)

**Interpretation:** The S-shaped curve describes slow early growth, rapid growth near day 42 and levelling off at about 226 cm. The logistic model is about 16 times more accurate than a straight line (dashed), showing why the model type matters more than adding data.


# Task 15: Numerical Differentiation

**Code:** `code/Task_15_numerical_differentiation.py` (original + parameter change) and `code/Task_15_modified.py` (modified case)

## B. Input values and mathematical operation

**Input values:** GPS times 0–6 s and positions 0–38 m.

**Mathematical operations:** forward difference at the start, central difference $\frac{x_{i+1}-x_{i-1}}{t_{i+1}-t_{i-1}}$ inside, backward difference at the end.

### Original program output

![Task 15: terminal output of the original program and the parameter change](screenshots/Task_15_numerical_differentiation_terminal.png)

![Task 15: graph from the original program](screenshots/Task_15_original_plot.png)

## C. Parameter change

**Change made:** step size $h$ from 1 down to $10^{-12}$ on $f(x)=\sin x$ at $x=1$ (exact derivative 0.5403023).

**Result:** the forward error is proportional to $h$ (0.47 → 4.2×10⁻⁷) and the central error to $h^2$ (0.086 → 2.8×10⁻¹¹ at $h=10^{-6}$). At $h=10^{-12}$ both errors grow again (4.3×10⁻⁵, 1.2×10⁻⁵) because of round-off. There is an optimum step size.

## D. Meaning of the output and graph

The vehicle speeds up from 4.80 m/s to 8.00 m/s over 6 s, an average acceleration of about 0.5 m/s² ((8.0 − 4.8)/6 ≈ 0.53). The graph is an upward line: the vehicle is accelerating steadily.

## E. Modified case: Speed and acceleration of a vehicle from noisy GPS positions

**Assumptions:** Positions every 2 s follow $x=0.8t^2+2t$ (true speed $1.6t+2$, acceleration 1.6 m/s²) plus ±0.15 m noise. (The motion is chosen to demonstrate the method and is not meant to be a realistic matatu.)

**Input data:** 11 samples, $t=0,2,\dots,20$ s.

**Output:** Interior speed errors are at most 0.13 m/s but the end points are poor (+1.26 m/s at $t=0$ and −1.83 m/s at $t=20$); mean absolute speed error 0.322 m/s. Acceleration from the second difference: mean 1.606 m/s² (true 1.6) but standard deviation 0.165.

![Task 15: terminal output of the modified program](screenshots/Task_15_modified_terminal.png)

![Task 15: graph from the modified program](screenshots/Task_15_modified_plot.png)

**Interpretation:** One-sided differences at the ends are less accurate than central differences. Differentiating twice amplifies noise: the acceleration values scatter widely around 1.6 m/s² in the right-hand graph even though their mean is accurate. Smoothing the data first would help.


# Task 16: Numerical Integration

**Code:** `code/Task_16_numerical_integration.py` (original + parameter change) and `code/Task_16_modified.py` (modified case)

## B. Input values and mathematical operation

**Input values:** smart-meter power every 2 h (0.8–1.8 kW) from 0 to 12 h.

**Mathematical operations:** trapezoidal rule $\sum\frac{(y_i+y_{i+1})h}{2}$ and Simpson's rule (parabolas through three points).

### Original program output

![Task 16: terminal output of the original program and the parameter change](screenshots/Task_16_numerical_integration_terminal.png)

![Task 16: graph from the original program](screenshots/Task_16_original_plot.png)

## C. Parameter change

**Change 1:** power at 06:00 raised from 1.8 to 2.6 kW. Energy rose from 15.70/15.80 kWh to **17.30 (trapezoid)** and **17.93 (Simpson)** kWh.

**Change 2:** more points on $\int_0^\pi\sin x\,dx=2$. With 65 points the trapezoid error is 4.0×10⁻⁴ while Simpson's is 6.5×10⁻⁸. Simpson's rule converges much faster for smooth functions.

## D. Meaning of the output and graph

The home used about **15.7–15.8 kWh in 12 hours** (area under the power curve). The two rules differ by only 0.1 kWh (0.6 %). The shaded area in the graph is the energy.

## E. Modified case: Daily energy from a rooftop solar panel

**Assumptions:** Output $P(t)=3.2\sin(\pi(t-6)/12)^{1.5}$ kW from 06:00 to 18:00, read hourly. Electricity is valued at 25 KES/kWh. The exact value is found with adaptive `quad`.

**Input data:** 13 hourly readings.

**Output:** Trapezoid 21.3445 kWh (error 0.0219); Simpson 21.3789 kWh (error 0.0125); `quad` 21.3664 kWh. Value **KES 534.47/day** (about 16,034 per 30 days). With readings every 3 h: Simpson 21.62, trapezoid 21.02 kWh.

![Task 16: terminal output of the modified program](screenshots/Task_16_modified_terminal.png)

![Task 16: graph from the modified program](screenshots/Task_16_modified_plot.png)

**Interpretation:** The panel produces about 21.4 kWh a day. Hourly readings are accurate to within 0.1 %, but 3-hourly readings make errors of about 1–2 %. More frequent measurements give a more reliable energy bill.


# Task 17: Ordinary Differential Equations – Euler Method

**Code:** `code/Task_17_euler_ode.py` (original + parameter change) and `code/Task_17_modified.py` (modified case)

## B. Input values and mathematical operation

**Input values:** ambient 25 °C, initial 90 °C, $k=0.12$/min, step $h=1$ min, 30 min.

**Mathematical operation:** Euler's update $T_{n+1}=T_n+h(-k(T_n-T_a))$ compared with the exact solution $T=T_a+(T_0-T_a)e^{-kt}$.

### Original program output

![Task 17: terminal output of the original program and the parameter change](screenshots/Task_17_euler_ode_terminal.png)

![Task 17: graph from the original program](screenshots/Task_17_original_plot.png)

## C. Parameter change

**Change 1:** step size 5, 2, 1, 0.5, 0.1 min gave final errors 1.51, 0.72, 0.37, 0.19, 0.038 °C. The error halves when $h$ halves (first-order method).

**Change 2:** $k=0.4$ with $h=6$ ($hk=2.4>2$): Euler gives 90, −66, 152, −153, ... which oscillates and grows although the real temperature settles at 25 °C. This is numerical **instability**.

## D. Meaning of the output and graph

After 30 minutes Euler gives 26.40 °C and the exact value is 26.78 °C (error 0.37 °C). The graph shows the Euler points staying slightly below the exact curve because the method uses the slope at the start of each step, which is steeper than the average slope.

## E. Modified case: SIR epidemic among 1,000 learners

**Assumptions:** $dS/dt=-\beta SI/N$, $dI/dt=\beta SI/N-\gamma I$, $dR/dt=\gamma I$ with $\beta=0.35$/day, $\gamma=0.10$/day ($R_0=3.5$), start 990 S, 10 I, 0 R, $h=0.5$ day.

**Input data:** N = 1000, 100 days.

**Output:** Peak **367 infected on day 23**; total ever infected 968 (96.8 %); S+I+R stays 1000. With $\beta=0.20$ (distancing): peak 160 and total 779. With $h=0.05$ the peak is 360 (close to 367).

![Task 17: terminal output of the modified program](screenshots/Task_17_modified_terminal.png)

![Task 17: graph from the modified program](screenshots/Task_17_modified_plot.png)

**Interpretation:** With $R_0=3.5$ almost everyone is eventually infected, but reducing contact (β = 0.35 → 0.20) cuts the peak by more than half (367 → 160) and so reduces the pressure on health services. The small change when $h$ is reduced shows that the Euler results are reliable enough.


# Task 18: Numerical Solution of ODEs Using SciPy

**Code:** `code/Task_18_scipy_ode.py` (original + parameter change) and `code/Task_18_modified.py` (modified case)

## B. Input values and mathematical operation

**Input values:** $V_s=5$ V, $R=1000\,\Omega$, $C=1$ mF ($\tau=RC=1$ s), $V_0=0$, time 0–5 s, tolerances $10^{-9}$.

**Mathematical operation:** `solve_ivp` solves $dV/dt=(V_s-V)/(RC)$ with an adaptive Runge–Kutta method; compared with $V=V_s(1-e^{-t/RC})$.

### Original program output

![Task 18: terminal output of the original program and the parameter change](screenshots/Task_18_scipy_ode_terminal.png)

![Task 18: graph from the original program](screenshots/Task_18_original_plot.png)

## C. Parameter change

**Change made:** $R=3000\,\Omega$ ($\tau=3$ s) and $V_0=2$ V.

**Result:** the capacitor charges more slowly: at 5 s it reaches 4.433 V instead of 4.966 V. The time to cover 63.2 % of the remaining rise is 2.99 s, matching $\tau=3$ s. A larger resistance means a slower circuit.

## D. Meaning of the output and graph

The capacitor voltage at 5 s is 4.9663 V, 99.3 % of the supply, and the numerical and exact solutions differ by only 9.3×10⁻¹⁰ V. In the graph the curve rises quickly then flattens, reaching the supply voltage after about 5 time constants.

## E. Modified case: Series RLC circuit step response (second-order ODE)

**Assumptions:** $L=0.5$ H, $C=0.05$ F, 10 V step, zero initial conditions; states $[v_C,i]$ with $\dot v_C=i/C$ and $\dot i=(V_s-Ri-v_C)/L$. Critical resistance $2\sqrt{L/C}$.

**Input data:** R = 1, 4, 6.325 (critical) and 12 Ω.

**Output:** R = 1 Ω: peak 16.05 V (60.5 % overshoot, settles in 2.66 s); R = 4 Ω: overshoot 7.7 % (0.80 s); critical R = 6.325 Ω: no overshoot, settles fastest in 0.74 s; R = 12 Ω (over-damped): no overshoot but slower, 1.70 s.

![Task 18: terminal output of the modified program](screenshots/Task_18_modified_terminal.png)

![Task 18: graph from the modified program](screenshots/Task_18_modified_plot.png)

**Interpretation:** Low resistance gives oscillation and a dangerous voltage overshoot; high resistance gives a slow response. The best design is near critical damping.


# Task 19: Numerical Optimization

**Code:** `code/Task_19_optimization.py` (original + parameter change) and `code/Task_19_modified.py` (modified case)

## B. Input values and mathematical operation

**Input values:** cost A = 12, B = 18; capacity A = 100, B = 170; required capacity ≥ 1000; start (5, 3).

**Mathematical operation:** minimise $12x+18y$ subject to $100x+170y\ge1000$, $x,y\ge0$ (SLSQP), then a whole-number search.

### Original program output

![Task 19: terminal output of the original program and the parameter change](screenshots/Task_19_optimization_terminal.png)


## C. Parameter change

**Change 1:** the cost of Type B 18 → 25. The optimum switches to **10 Type A** (cost 120) since A now costs 0.12 per capacity unit and B 0.147.

**Change 2:** required capacity 1000 → 2500. The optimum becomes 14.706 B units (cost 264.71) and the whole-number solution 15 B (cost 270).

## D. Meaning of the output and graph

The cheapest plan uses only Type B: 5.882 units, cost **105.88**, capacity exactly 1000. Type B is cheaper per unit of capacity (0.1059 versus 0.12). Because servers must be whole, the real plan is 6 Type B servers for **108** with capacity 1020.

## E. Modified case: Least-cost feed mix for a dairy cow

**Assumptions:** Napier grass (6 KES/kg, energy 2 units/kg, protein 0.02 kg/kg, at most 40 kg) and dairy meal (45 KES/kg, 12 units/kg, 0.18 kg/kg). Each cow needs ≥ 90 energy units and ≥ 1.8 kg protein and can eat ≤ 45 kg/day.

**Input data:** Prices, nutrient values and requirements above (illustrative).

**Output:** Grass 0 kg, dairy meal 10 kg, cost **KES 450/day** (linprog confirms). Energy 120 (needs 90) and protein exactly 1.8 kg, so protein is the binding constraint. If meal costs 70/kg: grass 39.375 kg, meal 5.625 kg, KES 630/day. Herd of 25 cows: KES 11,250/day.

![Task 19: terminal output of the modified program](screenshots/Task_19_modified_terminal.png)


**Interpretation:** Protein costs 250 KES/kg from meal but 300 KES/kg from grass, so meal is preferred; once meal rises to 70 KES/kg (389 per kg protein) grass becomes cheaper, so the optimum switches. Optimisation shows how the best plan changes with prices.


# Task 20: Random Numbers and Monte Carlo Methods

**Code:** `code/Task_20_monte_carlo.py` (original + parameter change) and `code/Task_20_modified.py` (modified case)

## B. Input values and mathematical operation

**Input values:** capacity 1000, demand $\sim N(850,120)$, $N=100{,}000$ simulations, seed 42.

**Mathematical operation:** generate random demand, count the fraction above capacity: $p\approx\text{overloaded}/N$.

### Original program output

![Task 20: terminal output of the original program and the parameter change](screenshots/Task_20_monte_carlo_terminal.png)

![Task 20: graph from the original program](screenshots/Task_20_original_plot.png)

## C. Parameter change

**Change 1:** capacity 1000 → 1100. The overload probability fell from 10.6 % to **1.91 %** (exact 1.86 %).

**Change 2:** $N=1{,}000$, $100{,}000$ and $1{,}000{,}000$ gave 10.40 %, 10.59 %, 10.61 % (exact 10.565 %). The random error shrinks like $1/\sqrt N$; for 1,000 runs the error is typically a few tenths of a percentage point.

## D. Meaning of the output and graph

The estimated overload probability is **10.62 %** (10,616 of 100,000), against an exact 10.565 %, so about 1 day in 10 the server is overloaded. In the graph the estimate fluctuates strongly with few simulations and settles near the red exact line as the number of simulations increases.

## E. Modified case: Probability that a construction project finishes within 30 days

**Assumptions:** Three sequential tasks with triangular durations: foundation (6, 8, 14), walls (9, 12, 20), roofing (5, 7, 12) days.

**Input data:** 200,000 simulations; deadline 30 days; plus a π estimate with 1,000,000 points.

**Output:** Mean 30.98 days (the sum of most-likely durations is 27); std 3.23; **P(finish ≤ 30 days) = 40.14 %**; 90th percentile 35.29 days; 95 % range 25.2–37.6 days; π ≈ 3.14034.

![Task 20: terminal output of the modified program](screenshots/Task_20_modified_terminal.png)

![Task 20: graph from the modified program](screenshots/Task_20_modified_plot.png)

**Interpretation:** A plan built only from most-likely durations (27 days) is over-optimistic because the long right tails push the mean to 31 days. There is only a 40 % chance to meet the 30-day deadline, and about 35 days must be planned for 90 % confidence.
