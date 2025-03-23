# Dantzig-Wolfe Decomposition
The Wolfe-Dantzig decomposition method (also known as Dantzig-Wolfe decomposition) was developed by George Dantzig and Philip Wolfe in 1960s and explained in their paper "Decomposition Principle for Linear Programs" as an extension of the Simplex method to handle structured optimization problems efficiently. It is an algorithm for solving large-scale linear programming (LP) problems that inherits a block-angular structure.

The original **large-scale linear program (LP)** problem is formulated as:

$$Maximize: c^T x$$

$$\text{subject to:} \quad  A x ≤ b,$$
$$\quad D_i x_i ≤ d_i  \quad  \forall i = 1, 2, ..., N,$$  
$$\quad x ≥ 0$$

Where:

$x$: Vector of decision variables.<br>
$c$: Vector of objective coefficients.<br>
$A$: Matrix of linking constraints (shared across all subproblems).<br>
$b$: Right-hand side vector for the linking constraints.<br>
$D_i$: Constraint matrix for the i-th subproblem.<br>
$d_i$: Right-hand side vector for the i-th subproblem.<br>
$N$: Number of subproblems.

**Decomposed Problem**<br>
The problem is decomposed into a master problem and subproblems.

**Master Problem**<br>
The master problem coordinates the subproblems and ensures the linking constraints are satisfied. It is formulated as:

$$Maximize: Σ_i Σ_j (c_i^T x_i^j) λ_i^j$$

$$\text{subject to:} \quad  Σ_i Σ_j (A_i x_i^j) λ_i^j ≤ b,$$   
$$\quad Σ_j λ_i^j = 1,  \quad \forall i = 1, 2, \dots, N,$$
$$\quad λ_i^j ≥ 0,     \quad \forall i, j$$

Where:<br>
$λ_i^j$: Weight (or coefficient) associated with the j-th extreme point of the i-th subproblem.<br>
$J_i$: Number of extreme points for the i-th subproblem.<br>
$c_i$: Portion of the objective coefficients corresponding to the i-th subproblem.<br>
$A_i$: Portion of the linking constraint matrix corresponding to the i-th subproblem.<br>

**Subproblems**<br>
Each subproblem corresponds to one of the independent blocks in the original problem. The $i-th$ subproblem is formulated as:

$$Maximize: (c_i - π^T A_i)^T x_i$$

$$
\text{subject to:} \quad D_i x_i ≤ d_i, 
\quad x_i ≥ 0
$$

Where:<br>
$π$: Vector of dual variables (prices) associated with the linking constraints in the master problem.<br>
$x_i$: Vector of decision variables for the i-th subproblem.<br>

**Column Generation**
The Dantzig-Wolfe decomposition uses column generation to iteratively add new extreme points (x_i^j) to the master problem. The steps are as follows:<br>
Solve the restricted master problem (RMP) with a subset of extreme points.<br>
Use the dual variables (π) from the RMP to solve the subproblems.<br>
Check if any subproblem can generate a new extreme point that improves the RMP's objective.<br>
If such a point exists, add it to the RMP and repeat the process. Otherwise, terminate.<br>

This method is particularly useful when:
- The constraint matrix has a decomposable structure with independent subproblems.
- The problem has multiple subproblems that share linking constraints.
- Directly solving the full LP is computationally expensive.
