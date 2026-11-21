BIT 2118: SCIENTIFIC COMPUTING


Repository & Task Implementation Documentation
Comprehensive technical reference covering environment setup, repository structure, mathematical formulations, and numerical implementations for Tasks A through R (Task_02 through Task_19).

1. Environment Setup & Execution
Prerequisites & Dependencies
Ensure Python 3.8 or newer is installed along with the core scientific computing stack:
pip install numpy scipy matplotlib

•	NumPy: Provides contiguous N-dimensional array storage, vectorized arithmetic, finite differences, trapezoidal integration, and dense linear algebra routines.
•	SciPy: Supplies advanced numerical solvers including LU factorizations (scipy.linalg), root-finding & nonlinear system solvers (scipy.optimize), splines (scipy.interpolate), and ODE/quadrature routines (scipy.integrate).
•	Matplotlib: Generates convergence histories, interpolation curves, derivative/ODE trajectories, and mathematical function visualizations.
Running the Scripts
All implementation scripts are located inside the code/ directory and can be executed standalone from the repository root:
python code/Task_02_numpy_arrays.py


2. Repository Overview (Tasks A–R)
Task	Concept / Mathematical Topic	Program File Path	Core Libraries
A	NumPy Arrays and Numerical Computation	code/Task_02_numpy_arrays.py	numpy
B	Mathematical Functions and Vectorization	code/Task_03_vectorization.py	numpy, timeit
C	Floating-Point Arithmetic and Numerical Errors	code/Task_04_numerical_errors.py	numpy
D	Matrix Algebra and Matrix Operations	code/Task_05_matrix_algebra.py	numpy
E	Solution of Systems of Linear Equations	code/Task_06_linear_systems.py	numpy
F	Gaussian Elimination and LU Decomposition	code/Task_07_gaussian_lu.py	numpy, scipy.linalg
G	Iterative Methods for Linear Systems	code/Task_08_iterative_linear.py	numpy, matplotlib
H	Eigenvalues and Eigenvectors	code/Task_09_eigenvalues.py	numpy
I	Root Finding: Bisection and Fixed-Point Methods	code/Task_10_bisection_fixed_point.py	numpy, scipy.optimize, matplotlib
J	Root Finding: Newton-Raphson and Secant Methods	code/Task_11_newton_secant.py	scipy.optimize
K	Polynomial and Nonlinear Equation Solving	code/Task_12_nonlinear_equations.py	numpy, scipy.optimize
L	Interpolation	code/Task_13_interpolation.py	numpy, scipy.interpolate, matplotlib
M	Curve Fitting and Approximation	code/Task_14_curve_fitting.py	numpy, scipy.optimize, matplotlib
N	Numerical Differentiation	code/Task_15_numerical_differentiation.py	numpy, matplotlib
O	Numerical Integration	code/Task_16_numerical_integration.py	numpy, scipy.integrate
P	Ordinary Differential Equations - Euler Method	code/Task_17_euler_ode.py	numpy, matplotlib
Q	Numerical Solution of ODEs Using SciPy	code/Task_18_scipy_ode.py	numpy, scipy.integrate, matplotlib
R	Numerical Optimization	code/Task_19_optimization.py	numpy, scipy.optimize


3. Detailed Task Documentation (Tasks A–R)
Task A: NumPy Arrays and Numerical Computation
Script Path: code/Task_02_numpy_arrays.py
Scenario: Daily meteorological telemetry processing using 24 hourly temperature readings stored in a 1D NumPy array (temperatures_c).
T_F = 1.8 * T_C + 32
Operations & Numerical Methods:
•	Descriptive Statistics: Computes daily summary metrics across the array using np.mean(), np.min(), and np.max().
•	Vectorized Unit Conversion: Applies an element-wise affine transformation to convert all Celsius readings to Fahrenheit simultaneously without explicit loops.
Outputs: Prints the daily mean, minimum, and maximum temperatures in Celsius formatted to two decimal places, followed by the first six Fahrenheit values via array slicing (temperatures_f[:6]).
Task B: Mathematical Functions and Vectorization
Script Path: code/Task_03_vectorization.py
Scenario: Execution speed benchmarking comparing pure Python loops against NumPy vectorization across 1,000,000 simulated network latency readings uniformly distributed between 5 ms and 250 ms.
Operations & Numerical Methods:
•	Reproducible Data Generation: Initializes a random number generator via np.random.default_rng(42) to draw one million uniform samples.
•	Loop vs. Broadcasted Computation: Compares iterative element appending (loop_version) against C-backed array arithmetic (vectorized_version) to compute squared latency in seconds: (t / 1000)^2.
•	Performance Timing: Measures average execution runtime using timeit.timeit and calculates the relative speed-up factor.
Implementation Note: In loop_version(), the return squared statement is indented inside the for loop, causing the function to return after a single iteration. Dedenting return squared to align with the for statement enables a full 1,000,000-element benchmark comparison.
Task C: Floating-Point Arithmetic and Numerical Errors
Script Path: code/Task_04_numerical_errors.py
Scenario: Investigation of IEEE 754 binary floating-point representation limits, error quantification metrics, and mitigation of catastrophic cancellation.
f(x) = (sqrt(1 + x) - 1) / x   ==>   f_stable(x) = 1 / (sqrt(1 + x) + 1)
Operations & Numerical Methods:
•	Tolerance-Based Equality: Contrasts direct floating-point comparison (0.1 + 0.2 == 0.3, evaluating to False) against relative/absolute tolerance checking via np.isclose() (True).
•	Error Metrics: Computes absolute error (|x - x_approx|) and relative error (|x - x_approx| / |x|) comparing np.sqrt(2) with the truncated approximation 1.414.
•	Algebraic Stabilization: Evaluates f(x) = (sqrt(1 + x) - 1) / x for small magnitudes (1e-4, 1e-8, 1e-12) and eliminates subtractive cancellation by rationalizing the numerator.
Outputs: Displays precision loss in the direct formula as x approaches machine epsilon while the stabilized form maintains full accuracy.
Task D: Matrix Algebra and Matrix Operations
Script Path: code/Task_05_matrix_algebra.py
Scenario: 2D linear geometric transformations (rotation combined with anisotropic scaling) applied to the four vertices of a unit square.
Operations & Numerical Methods:
•	Operator Construction: Builds a 2x2 counter-clockwise rotation matrix R(45 deg) and an anisotropic scaling matrix S = diag(2.0, 1.5).
•	Matrix Composition: Combines transformations into a single operator T = R @ S using the matrix multiplication operator.
•	Coordinate Mapping & Determinant: Transforms row-vector vertex coordinates via P' = P @ T.T and computes the area scaling factor using np.linalg.det(T) (yielding det(T) = 3.0).
Outputs: Prints the original coordinates, composite transformation matrix, transformed vertex coordinates, and matrix determinant.
Task E: Solution of Systems of Linear Equations
Script Path: code/Task_06_linear_systems.py
Scenario: Infrastructure capacity planning solving for the exact counts of Type A (x) and Type B (y) servers required to match observed CPU and memory consumption.
Operations & Numerical Methods:
•	Direct Linear Solver: Formulates the 2x2 linear system Ax = b (4x + 2y = 20 CPU units; 8x + 12y = 72 GB memory) and solves it using np.linalg.solve(A, b).
•	Residual Verification: Validates solution accuracy by computing the residual vector r = b - A @ x and its Euclidean norm ||r||_2 via np.linalg.norm().
Outputs: Reports the exact server counts (Type A = 3, Type B = 4), reconstructed resource usage A @ x, and near-zero residual norm.
Task F: Gaussian Elimination and LU Decomposition
Script Path: code/Task_07_gaussian_lu.py
Scenario: Efficient solution of multiple linear systems Ax_i = b_i sharing an invariant 3x3 tridiagonal coefficient matrix A across three separate right-hand side measurement vectors.
Operations & Numerical Methods:
•	One-Time LU Factorization: Decomposes A = PLU once using scipy.linalg.lu_factor(A), incurring O(n^3) computational cost a single time.
•	Repeated Substitution Solves: Solves each measurement vector in O(n^2) operations via forward and backward substitution using scipy.linalg.lu_solve((lu, piv), b).
Verification: Computes the residual norm for every measurement set and cross-checks the first solution against np.linalg.solve(A, b_1).
Task G: Iterative Methods for Linear Systems
Script Path: code/Task_08_iterative_linear.py
Scenario: 1D steady-state heat conduction model across five interior grid nodes bounded by fixed boundary temperatures (100 C on the left boundary, 20 C on the right boundary).
Operations & Numerical Methods:
•	Jacobi Iteration: Implements simultaneous updates x^(k+1) = D^(-1)(b - R x^(k)) using exclusively values from the previous iteration.
•	Gauss-Seidel Iteration: Implements sequential in-place updates so newly computed nodal temperatures immediately inform subsequent nodes within the same sweep.
•	Convergence Criteria & Plotting: Iterates until ||x^(k+1) - x^(k)||_inf < 1e-8 and plots semilogarithmic error curves (plt.semilogy) comparing convergence rates.
Outputs: Verifies both iterative methods against the direct np.linalg.solve benchmark and visualizes Gauss-Seidel's faster convergence.
Task H: Eigenvalues and Eigenvectors
Script Path: code/Task_09_eigenvalues.py
Scenario: Network centrality analysis computing eigenvector importance scores for a 4-node undirected graph (nodes A, B, C, D) represented by a symmetric adjacency matrix A.
Operations & Numerical Methods:
•	Spectral Decomposition: Computes all eigenvalues and eigenvectors via np.linalg.eig(A) and identifies the dominant eigenvalue lambda_max.
•	Eigenvector Centrality: Extracts the principal eigenvector v, takes its absolute real components, and normalizes by its L1 sum to obtain relative node centrality scores.
•	Eigenpair Validation: Confirms numerical accuracy by measuring the residual norm ||A @ v - lambda_max * v||_2.
Outputs: Displays the dominant eigenvalue, normalized importance ranking (identifying hub nodes B and C as highest), and verification error.
Task I: Root Finding — Bisection and Fixed-Point Methods
Script Path: code/Task_10_bisection_fixed_point.py
Scenario: Nonlinear scalar root finding to determine the thermal equilibrium temperature T where radiative/convective heat loss balances internal heat generation.
f(T) = T - 25 - 10 * exp(-T / 20) = 0
Operations & Numerical Methods:
•	Bisection Method: Brackets the sign change of f(T) = T - 25 - 10 * exp(-T/20) on [20, 40] and converges to xtol=1e-10 via scipy.optimize.root_scalar(..., method='bisect').
•	Fixed-Point Iteration: Rearranges f(T) = 0 into the contraction mapping T_(k+1) = g(T_k) = 25 + 10 * exp(-T_k/20) starting at T_0 = 30.0 until |T_(k+1) - T_k| < 1e-10.
•	Graphical Root Verification: Plots f(T) across [20, 40] with a horizontal zero-axis line and a vertical dashed indicator at the equilibrium root.
Outputs: Prints the equilibrium temperature from both methods alongside the fixed-point iteration count and renders the root-finding plot.
Task J: Root Finding — Newton-Raphson and Secant Methods
Script Path: code/Task_11_newton_secant.py
Scenario: Financial capital budgeting analysis computing the Internal Rate of Return (IRR) for a 4-year investment project with an initial outlay of -10,000 followed by annual cash inflows [3000, 3500, 4000, 2500].
NPV(r) = sum_{t=0}^{4} CF_t / (1 + r)^t = 0
Operations & Numerical Methods:
•	Newton-Raphson Method: Uses analytical first derivatives d(NPV)/dr with initial guess r_0 = 0.10 via scipy.optimize.root_scalar(..., method='newton', xtol=1e-12).
•	Secant Method: Approximates the derivative using two initial guesses (r_0 = 0.05, r_1 = 0.20) via scipy.optimize.root_scalar(..., method='secant', xtol=1e-12) without requiring analytical derivatives.
Outputs: Prints the converged IRR percentage from both methods and verifies that NPV(IRR) evaluates to zero within machine tolerance.
Task K: Polynomial and Nonlinear Equation Solving
Script Path: code/Task_12_nonlinear_equations.py
Scenario: Solving a coupled 2D nonlinear algebraic system to locate the geometric intersection points between a circle of radius 2 and an upward-opening parabola.
x^2 + y^2 = 4   and   y = x^2 - 1
Operations & Numerical Methods:
•	Multivariate Root Solver: Defines the residual vector F(x, y) = [x^2 + y^2 - 4, y - (x^2 - 1)] and solves F(x, y) = 0 using scipy.optimize.root().
•	Multi-Start Root Discovery: Executes the solver from symmetric initial guesses ([1.0, 1.0] and [-1.0, 1.0]) to capture both distinct intersection points.
Outputs: Displays the converged (x, y) intersection coordinates, residual vectors, and solver convergence flags for each initial guess.
Task L: Interpolation
Script Path: code/Task_13_interpolation.py
Scenario: Sub-hourly temperature estimation reconstructing 10-minute interval telemetry (1/6 hour) from discrete hourly measurements collected between 08:00 and 14:00.
Operations & Numerical Methods:
•	Piecewise Linear Interpolation: Constructs a C^0 continuous interpolant using scipy.interpolate.interp1d(..., kind='linear').
•	Cubic Spline Interpolation: Constructs a smooth C^2 continuous piecewise cubic polynomial using scipy.interpolate.CubicSpline().
Outputs: Compares interpolated temperature estimates at 10:30 (t = 10.5) and plots both curves against the raw scatter measurements.
Task M: Curve Fitting and Approximation
Script Path: code/Task_14_curve_fitting.py
Scenario: Empirical performance modeling of web server response times (ms) as concurrent user load scales from 50 to 350 users, with extrapolation to 400 users.
y(x) = a * x^2 + b * x + c
Operations & Numerical Methods:
•	Nonlinear Least-Squares Fitting: Fits a second-degree polynomial y = ax^2 + bx + c to noisy empirical telemetry using scipy.optimize.curve_fit().
•	Goodness-of-Fit & Extrapolation: Quantifies model accuracy via Root Mean Square Error (RMSE) and predicts latency at 400 concurrent users.
Outputs: Prints fitted quadratic parameters, RMSE in milliseconds, extrapolated 400-user latency, and plots the fitted curve over observed data.
Task N: Numerical Differentiation
Script Path: code/Task_15_numerical_differentiation.py
Scenario: Instantaneous velocity estimation from discrete 1-second GPS position samples (metres) recorded over a 6-second interval.
Operations & Numerical Methods:
•	Boundary Finite Differences: Applies a first-order forward difference at t = 0 s and a first-order backward difference at t = 6 s.
•	Interior Central Differences: Applies second-order accurate central differences v_i = (x_(i+1) - x_(i-1)) / (t_(i+1) - t_(i-1)) across all interior sample points.
Outputs: Tabulates timestamp, position, and estimated speed (m/s) at every sample point and plots the resulting velocity profile.
Task O: Numerical Integration
Script Path: code/Task_16_numerical_integration.py
Scenario: Cumulative electrical energy consumption (kWh) calculated from smart-meter power readings (kW) sampled at 2-hour intervals over a 12-hour window.
E = integral_{0}^{12} P(t) dt
Operations & Numerical Methods:
•	Composite Trapezoidal Rule: Approximates the definite integral of power over time using piecewise linear panels via np.trapezoid(power_kw, time_hours).
•	Composite Simpson's Rule: Approximates the integral using piecewise quadratic polynomials via scipy.integrate.simpson(power_kw, x=time_hours).
Outputs: Reports total energy estimates in kWh from both quadrature rules alongside their absolute discrepancy.
Task P: Ordinary Differential Equations — Euler Method
Script Path: code/Task_17_euler_ode.py
Scenario: Thermal cooling simulation modeling the temperature decay of a hot object (T_0 = 90 C) in an ambient environment (T_amb = 25 C) over 30 minutes under Newton's Law of Cooling.
dT / dt = -k * (T - T_ambient)
Operations & Numerical Methods:
•	Explicit Forward Euler Integration: Advances the state with uniform step size h = 1.0 min via T_(i+1) = T_i + h * (-k * (T_i - T_amb)) for cooling constant k = 0.12.
•	Analytical Validation: Benchmarks numerical trajectory against the exact closed-form exponential solution T(t) = T_amb + (T_0 - T_amb) * exp(-kt).
Outputs: Reports simulated vs. exact temperature at t = 30 min, final absolute truncation error, and overlays both trajectories.
Task Q: Numerical Solution of ODEs Using SciPy
Script Path: code/Task_18_scipy_ode.py
Scenario: Transient voltage simulation across a charging RC electrical circuit (Vs = 5.0 V, R = 1000 ohms, C = 0.001 F, time constant tau = 1.0 s) over 5 seconds.
dV / dt = (Vs - V) / (R * C)
Operations & Numerical Methods:
•	Adaptive Runge-Kutta Integration: Integrates dV/dt = (Vs - V) / (RC) from V(0) = 0 V using scipy.integrate.solve_ivp() with tight tolerances (rtol=1e-9, atol=1e-11) evaluated across 200 points.
•	Exact Transient Verification: Compares solver output against V(t) = Vs * (1 - exp(-t / (RC))) to quantify maximum global error.
Outputs: Prints solver status, final capacitor voltage at t = 5 s, maximum error across the time grid, and plots numerical vs. exact charging curves.
Task R: Numerical Optimization
Script Path: code/Task_19_optimization.py
Scenario: Constrained cost minimization for cloud server provisioning, selecting allocations of Type A (cost 12, capacity 100) and Type B (cost 18, capacity 170) units to meet a minimum capacity threshold of 1,000.
min 12x + 18y   subject to   100x + 170y >= 1000,  x, y >= 0
Operations & Numerical Methods:
•	Continuous Constrained Optimization: Minimizes linear cost f(x, y) = 12x + 18y subject to inequality constraint 100x + 170y >= 1000 and non-negativity bounds via scipy.optimize.minimize(..., method='SLSQP').
•	Discrete Integer Search: Performs an exhaustive grid search over integer server counts (0 <= a, b < 20) to identify the optimal whole-server deployment.
Outputs: Reports continuous SLSQP allocation, total capacity, minimum continuous cost, and the optimal discrete integer combination.


4. Remaining Repository Modules (Task S)
Scope Note — Final Module Defined in Repository Manifest
Only one module from the repository README remains undocumented because its source file was not included in the uploaded batch:
• Task S — Random Numbers and Monte Carlo Methods (code/Task_20_monte_carlo.py): Stochastic simulation, random sampling, and Monte Carlo integration/estimation.
• Note on Task_02_numpy_arrays_2.py: Confirmed as an identical duplicate of Task_02_numpy_arrays.py (Task A).

