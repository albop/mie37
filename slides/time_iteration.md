# Time Iteration

## Advanced Macro: Numerical Methods,  2021 (MIE37)

----

### Plan for today

- Optimization: complementarity conditions
- Introduce iterative methods for functional iterations
  - math: __interpolation__
- Expose the Time-Iteration algorithms
- Mention variants
  - improved time-iteration algorithm
  - parameterized expectations

----

### Functional iterations

- So far we have dealt with
  - discrete models
  - discretized versions of a model
- But for many models, the natural mathematical formulation of model is a *functional* equation

----

### General formulation

- Vector of states $s$
- Vector of controls $x$

- Random shock: $\epsilon$
- Transition:
$$s_{t+1} = g(s_t, x_t, \epsilon_{t+1})$$
- Utility with discount rate $\beta \in [0,1]$
$$U(s_t, x_t)$$
- Value function: $V(s) \in R$

- Solution is a function $V()$ (value) which is a fixed point of the Bellman-operator:

$$\forall s, V(s) = \max_x U(s,x) + \beta E \left[ V(g(s,x,\epsilon)) \right]$$

- The argmax, defines a decision rule *function*: $x = \varphi(s)$

----

### Discretization of the state

