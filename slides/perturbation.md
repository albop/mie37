
# Perturbation Analysis (1)

----

## Today

Study Neoclassical Model of Growth with Deterministic Productivity Shocks

1. Derive First Order Conditions
2. Computing Derivatives Numerically
3. Solution Method
   - Linear Time Iteration
4. Implementation

---

## Deriving First Order Conditions

----

## Neoclassical Growth Model

<div class="container">

<div class="col">

- Transition Equation
$$k_t = (1-\delta) k_{t-1} + i_{t-1}$$
$$z_t = \rho z_{t-1}$$
- Definition:
$$c_t = \exp(z_t) k_t^\alpha - i_t$$

- Control $i_t\in[-(1-\delta) k_t,k_t^\alpha[$
  - or equivalently $c_t \in [0, k_t^{\alpha}]$

- Objective:
$$\max_{i_t} \sum_{t\geq0} \beta^t U(c_t)$$


</div>
<div class="col">

- Calibration:
  - $\beta = 0.96$
  - $\delta = 0.1$
  - $\gamma = 4.0$
  - $\alpha = 0.3$
  - $U(x)=\frac{x^{1-\gamma}}{1-\gamma}$
  
</div>

----

### Deriving First Order Conditions

- Two methods:
    1. Bellman:
       1. Optimality Condition
       2. Enveloppe Condition
    2. Lagrangian:
- We will use the lagrangian version

----

### Lagrangian

- Initial Conditions (predetermined states):
    - $z_0$, $k_0$
- Problem:
$$V(z_0, k_0) = \max_{i_0, i_1, ..., c_0, c1, ...., k_1, k_2, ...  } \sum_{t \geq 0}\beta^t U(c_t)$$
s.t. $\forall t\geq 0$
$$\begin{eqnarray}
\mu_t:\quad &  0 & \leq & i_t + (1-\delta) k_t \\\\
\nu_t:\quad &  i_t & \leq & \exp(z_t) k_t^{\alpha} \\\\
\lambda_t:\quad &  i_t & = & \exp(z_t) k_t^{\alpha} - c_t\\\\
q_t:\quad &  k_{t+1} & = & (1-\delta) k_{t} + i_{t}
\end{eqnarray}$$
- Lagrangian:
$$\mathcal{L(z_0, k_0)} =   \sum_{t \geq 0} \beta^t\left\\{ U(c_t) + \mu_t \left( i (1-\delta) k_t + i_t \right) + \nu_t \left(\exp(z_t)k_t^{\alpha} - i_t \right) + \lambda_t \left(\exp(z_t) k_t^{\alpha}  - i_t -c_t \right)  + q_t \left( (1-\delta) k_{t} + i_{t} - k_{t+1} \right) \right\\}$$

----

### Lagrangian

<div class="container">
<div class="col">

- We maximize the lagrangian to get:

$$\begin{eqnarray}
\forall t\geq0 & \frac{\partial \mathcal{L}}{\partial i_t} & = & 0 \\\\
& \frac{\partial \mathcal{L}}{\partial c_t} & = & 0 \\\\
 & \frac{\partial \mathcal{L}}{\partial k_{t+1}} & = & 0 
\end{eqnarray}$$

- It is important to note that we don't differentiate with respect to a predetermined state
  - check that you don't differentiate w.r.t. $k_0$
- It looks like the first order condition added four new variables $\mu_t$,$\nu_t$, $\lambda_t$, $q_t$

</div>

<div class="col">

- Luckily these variables are associated to slackness conditions.

|                    |                                             |
| ------------------ | ------------------------------------------- |
| $\mu_t \geq 0$     | $(1-\delta)k_t + i_t \geq 0$                |
| $\nu_t \geq 0$     | $\exp(z_t) k_t^{\alpha}-i_t \geq 0$         |
| $q_t \geq 0$       | $(1-\delta) k_{t} + i_{t} - k_{t+1} \geq 0$ |
| $\lambda_t \geq 0$ | $\exp(z_t) k_t^{\alpha}  - i_t -c_t = 0$    |

- The Karush-Kuhn-Tucker states, that for each slackness condition, at any time, either
  - the lagrangian is 0 and it disappears from the F.O.C.s
  - or it is not 0 and the associated constraint adds another condition

</div>
</div>

----

### Eliminating constraints

<div class="container">
<div class="col">

|                    |                                             |
| ------------------ | ------------------------------------------- |
| $\mu_t \geq 0$     | $(1-\delta)k_t + i_t \geq 0$                |
| $\nu_t \geq 0$     | $\exp(z_t) k_t^{\alpha}-i_t \geq 0$         |
| $q_t$       | $(1-\delta) k_{t} + i_{t} - k_{t+1} = 0$ |
| $\lambda_t$ | $\exp(z_t) k_t^{\alpha}  - i_t -c_t = 0$    |

- In general slackness conditions can be occasionally binding
- For perturbation analysis, we need constraints to be always (or never binding)

</div>
<div class="col">

Let's review them:
- $\nu_t$: it is equivalent to $c_t\geq 0$
  - we necessarily have $c_t>0$ since $U^{\prime}(0)=\infty$
  - hence $\nu_t=0$
- $\mu_t$: it states $k_{t+1}\geq 0$
  - if $k_{t+1}=0$, then $c_{t+1}$. We can conclude $k_{t+1}>0$
  - hence $\mu_t=0$
- for multipliers associated to an equality constraint, we always keep the system
  - multiplier can have any sign
- but sometimes, there is an implicit inequality:
  -  $c_t \leq \exp(z_t) k_t^{\alpha}  - i_t$ ( you can destroy production insead of eating or investing it)
  - $k_{t+1} \leq (1-\delta) k_{t} + i_{t}$ (you can destroy capital instead of investing it)

</div>


----

### First order model

- Optimality Condition:
$$\beta  \left[ \frac{\left(c_{t+1}\right)^{-\gamma}}{\left(c_t\right)^{-\gamma}} \left( (1-\delta + \alpha k_t^{\alpha -1}) \right)\right] = 1$$
  - Takes into account the fact that $c_t>0$.
- Definition:
$$c_t = k_t^\alpha - i_t$$

- Transition:
$$k_t (1-\delta) k_{t-1} + i_{t-1}$$
$$z_t = \rho z_{t-1}$$

----

### Steady-State

- Steady-State: $\overline{i}, \overline{k}, \overline{z}$ such that:
  - $z_{t+1}=k_t=\overline{z}$
  - $k_{t+1}=k_t=\overline{k}$
  - $i_{t+1}=i_t=\overline{i}$
- ...satisfy the first order conditions
- ...i.e.
$$\beta   \left( (1-\delta + \alpha {\overline{k}}^{\alpha -1}) \right) = 1$$
$$\overline{k} (1-\delta) \overline{k} + \overline{i}$$
$$\overline{z} = \rho \overline{z}$$
- Solution?
  - closed-form
  - numerical

----

### Perturbation Analysis

<div class="container">

<div class="col">

- Write all variables in deviation to the steady-state:
$$z_{t}=\overline{z} + \Delta z_t$$
$$k_{t}=\overline{k} + \Delta k_t$$
$$i_{t}=\overline{i} + \Delta i_t$$
$$c_{t}=\overline{c} + \Delta c_t$$
  - Remark: some smart economists use log-deviations (i.e. $x_t = \overline{x} \hat{x}_t$ to make computations easier)

</div>
<div class="col">

- Replace in the system
$$\beta  \left[ \frac{\left(\overline{c}+ \Delta c_{t+1}\right)^{-\gamma}}{\left(\overline{c} + \Delta c_t\right)^{-\gamma}} \left( (1-\delta + \alpha (\overline{k} + \Delta k_{t+1})^{\alpha -1}) \right)\right] = 1$$
$$\overline{c} + \Delta c_t = (\overline{k}+ \Delta k_t)^\alpha - \overline{i} - \Delta i_t$$
$$\overline{k} + \Delta k_t = (1-\delta) (\overline{k}+ \Delta k_{t-1}) + \overline{i }+ \Delta i_{t-1}$$
$$\overline{z }+ \Delta z_t = \overline{z}+ \Delta \rho z_{t-1}$$
- Differentiate...
- (if we want to limit the number of equations, we can replace $c_t$ by its value)

</div>

----

### Result:

- Optimality Condition
$$\begin{bmatrix} . & . & . \\ \end{bmatrix} \begin{bmatrix} \Delta i_t \\\\ \Delta k_t \\\\ \Delta z_t \end{bmatrix} = \begin{bmatrix} . & . & . \\ \end{bmatrix} \begin{bmatrix} \Delta i_{t+t} \\\\ \Delta k_{t+1} \\\\ \Delta z_{t+1} \end{bmatrix} $$

- Transition
$$ \begin{bmatrix} \Delta k_t \\\\ \Delta z_t \end{bmatrix} = \begin{bmatrix} . & .  \\\\ . & . \end{bmatrix} \begin{bmatrix} \Delta k_{t-1} \\\\ \Delta z_{t-1} \end{bmatrix}  + \begin{bmatrix} . \end{bmatrix} \begin{bmatrix}\Delta i_{t-1}\end{bmatrix}$$

---

## Computing Derivatives

----

- Could we use Julia to make all the hard work?
  - yes, with the right library !
- Let's:
  1. introduce the julia console and package management system
  2. discuss which package to use for differentiation
  
----

### Julia Console

- Accessible:
  - from a good terminal
  - from VSCode panel

- Four modes:
  - ``: REPL (read-eval-print)
  - `?`: Help
  - `]`: Package Management
  - `;`: System Console

----

### Julia package ecosystem

- Large package ecosystem
- Fairly good quality native code
- Wrappers to low-level / foreign language libraries
  - C: `ccall`
  - Fortran: `fcall`
  - Python: `PyCall`

----

### How do you install packages?

- Short and wrong answer: `] add PackageName`
- Better answer:
  - a project environment specifies all dependencies for a project
    - informations are contained in `Project.toml`
  - change directory to the right project
    - `; cd path_to_the_right_project`
    - you can check where you are `; pwd` (print working directory)
  - activate environment:
    - `] activate .`  (`.` universally means current director)
  - add desired package:
    - `] add PackageName`
  - when you restart work on a project activate, it again, to ensure you have the right dependencies

----

### Main approaches

- Back to our problem, how to we differentiate the model?
- Main approaches:
    
    1. Manual
    2. Finite Differences
    3. Symbolic Differentiation
    4. Automatic Differentiation

----

### Manual Differentiation

- Trick:
    - never use $\frac{d}{dx} \frac{u(x)}{v(x)} = \frac{u'(x)v(x)-u(x)v'(x)}{v(x)^2}$
      - too error prone
    - use instead $$\frac{d}{dx} {u(x)v(x)} = {u'(x)v(x)+u(x)v'(x)}$$ and $$\frac{d}{dx} u(x) = -\frac{u^{\prime}}{u(x)^2}$$
- You can get easier calculations (in some cases) by using log-deviation rules

----

### Finite Differences

- Choose small $\epsilon>0$, typically $\sqrt{ \textit{machine eps}}$

- Forward Difference scheme:
    -  $f'(x) \approx \frac{f(x+\epsilon) - f(x)}{\epsilon}$
    - precision: $o(\epsilon)$
    - bonus: if $f(x+\epsilon)$ can compute $f(x)-f(x-\epsilon)$ instead (Backward)

- Central Difference scheme:
    -  $f'(x) \approx \frac{f(x+\epsilon) - f(x-\epsilon)}{2\epsilon}$
    - average of forward and backward
    - precision: $o(\epsilon^2)$

----

### Finite Differences: Higher order

- Central formula:
$$\begin{aligned}
f''(x) & \approx & \frac{f'(x)-f'(x-\epsilon)}{\epsilon} \approx \frac{(f(x+\epsilon))-f(x))-(f(x)-f(x-\epsilon))}{\epsilon^2} \\ & = & \frac{f(x+\epsilon)-2f(x)+f(x-\epsilon)}{\epsilon^2}
\end{aligned}$$
    - precision: $o(\epsilon)$
- Generalizes to higher order but becomes more and more innacurate

----

### Symbolic Differentiation

- manipulate the tree of algebraic expressions
    - implements various simplification rules
- requires mathematical expression
- can produce mathematical insights
- sometimes inaccurate:
  - cf: $\left(\frac{1+u(x)}{1+v(x)}\right)^{100}$

----

### Julia Packages:



- *FiniteDiff.jl*, *FiniteDifferences.jl*, *SparseDiffTools.jl*
    - careful implementation of finite diff
- *Calculus.jl*:
    - pure julia
    - finite difference
    - symbolic calculation
- *SymEngine.jl*
    - fast symbolic calculation

----

### Automatic Differentiation

- does not provide mathematical insights but solves the other problems

- can differentiate any piece of code
- two flavours
    - forward accumulation
    - reverse accumulation

----

### Simple example:

```julia
function f(x::Float64, y::Float64)
    a = x + 1
    b = y^2
    c = sin(a) + a + b
end
```

----

### Automatic rewrite: source code transform

```julia
function f(x::Float64)

    # x is an argument
    x_dx = 1.0

    a = x + 1
    a_dx = x_dx

    b = x^2
    b_dx = 2*x*x_dx

    t = sin(a)
    t_x = cos(a)*a_dx

    c = t + b
    c_x = t_dx + b_dx

    return (c, c_x)
end
```

----

### Dual numbers: operator overloading

```julia
struct DN
    x::Float64
    dx::Float64
end

+(a::DN,b::DN) = DN(a.x+b.x, a.dx+b.dx)
-(a::DN,b::DN) = DN(a.x-b.x, a.dx-b.dx)
*(a::DN,b::DN) = DN(a.x*b.x, a.x*b.dx+a.dx*b.x)
/(a::DN,b::DN) = DN(a.x/b.x, (a.dx*b.x-a.x*b.dx)/b.dx^2)

...
```

----

### Compatible with control flow

```julia
import ForwardDiff: Dual

x = Dual(1.0, 1.0)
a = 0.5*x
b = sum([(x)^i/i*(-1)^(i+1) for i=1:5000])
# compare with log(1+x)
```
  - generalizes nicely to gradient computations

```julia
x = Dual(1.0, 1.0, 0.0)
y = Dual(1.0, 0.0, 1.0)
exp(x) + log(y)
```

----

### Forward Accumulation Mode

- Forward Accumulation mode: isomorphic to dual number calculation
  - compute tree with values and derivatives at the same time
  - efficient for $f: R^n\rightarrow R^m$, with $n<<m$
    - (keeps lots of empty gradients when $n>>m$)

----

### Reverse Accumulation Mode

- Reverse Accumulation / Back Propagation
    - efficient for $f: R^n\rightarrow R^m$, with $m<<n$
    - requires data storage (to keep intermediate values)
    - graph / example

- Very good for machine learning:
   - $\nabla_{\theta} F(x;\theta)$ where $F$ can be an objective

----

### Libraries for AutoDiff

- See JuliaDiff: http://www.juliadiff.org/
  - *ForwardDiff.jl*
  - *ReverseDiff.jl*
- *Zygote.jl*
- Deep learning framework:
  - higher order diff w.r.t. any vector -> tensor operations
  - Flux.jl, MXNet.jl, Tensorflow.jl

---

## Solution using Linear Time iteration

----

### Our problem

<div class="container">

<div class="col">

$$ A s_t + B x_t + C s_{t+1} + D x_{t+1} = 0$$
$$ s_{t+1} = E s_t + F x_t $$

where:

$$s_t=(\Delta z_t, \Delta k_t)$$
$$x_t = (\Delta i_t)$$

</div>

<div class="col">
<div>

- What is the solution of our problem?
- At date $t$ what are the predetermined variables?
  - $k_t$ and $z_t$: the states
- The control $i_t$ must be a function of the states
  - there is a decision rule $i_t$ such that $i_t = \varphi(z_t, k_t)$
  - in the linearized model: $i_t = X \begin{bmatrix} z_t \\\\ k_t \end{bmatrix}$

</div>
</div>

----

### Optimality condition:


<div class="container">

<div class="col">

- Replacing in the system:

$$ x_t = X s_t $$
$$ s_{t+1} = E s_t + F X s_t $$
$$ x_{t+1} = X s_{t+1} $$
$$ A s_t + B x_t + C s_{t+1} + D x_{t+1} = 0$$

- If we make the full substitution:

$$( (A + B X) + ( D X + C) ( E  + F X ) ) s_t = 0$$

</div>
<div class="col">

- This must be true for all $s_t$. We get the special Ricatti equation:

$$(A + B X) + ( D X + C) ( E  + F X ) ) = 0 $$

- this is a __quadratic__, __matrix__ ( $X$ is 1 by 2 ) equation:
  - requires special solution method
  - there are multiple solutions: which should we choose?
    - next time: eigenvalues analysis

</div>
</div>

----

### Linear Time Iteration

- Let's be more subtle: define
  - $X$: decision rule today and
  - $\tilde{X}$ is decision rule tomorrow.
$$ x_t = X s_t$$
$$ s_{t+1} = E s_t + F X s_t $$
$$ x_{t+1} = \tilde{X} s_{t+1} $$
$$ A s_t + B x_t + C s_{t+1} + D x_{t+1} = 0$$
- We get the equation: 
$$F(X, \tilde{X}) = (A + B X) + ( D \tilde{X} + C) ( E  + F X ) ) = 0 $$

----

### Linear Time Iteration (2)

- We get the equation: 
$F(X, \tilde{X}) = (A + B X) + ( D \tilde{X} + C) ( E  + F X ) ) = 0 $

- Now consider the following algorithm:
  - choose stopping criteria: $\epsilon_0$ and $\eta_0$
  - choose random $X_0$
  - given $X_n$:
    - compute $X_{n+1}$ such that $F(X_{n+1}, X_{n}) = 0$
      - $$X_{n+1} = (...)$$
    - compute:
      - $\eta_n = |X_{n+1} - X_n|$
      - $\epsilon_n = F(X_{n+1}, X_{n+1})$
    - if $\eta_n<\eta_0$ and $\epsilon_n<\epsilon_0$
      - stop and return $X_{n+1}$
      - otherwise iterate with $X_{n+1}$

- When the model is well-specified this is guaranteed to converge to the right solution.




