# Tutorial: Perturbation of the Neoclassical Growth Model

Consider the deterministic neoclassical growth model

- Transition Equation
$$\begin{eqnarray}
k_t & = & (1-\delta) k_{t-1} + i_{t-1} \\\\
z_t & = & \rho z_{t-1}
\end{eqnarray}
$$

- Definition:
$$c_t = \exp(z_t) k_t^\alpha - i_t$$

- Control $i_t\in[-(1-\delta) k_t,k_t^\alpha[$
  - or equivalently $c_t \in [0, k_t^{\alpha}]$

- Objective:
$$\max_{i_t} \sum_{t\geq0} \beta^t U(c_t)$$



- Optimality Condition:
$$\beta  \left[ \frac{\left(c_{t+1}\right)^{-\gamma}}{\left(c_t\right)^{-\gamma}} \left( (1-\delta + \alpha k_t^{\alpha -1}) \right)\right] = 1$$
  - Takes into account the fact that $c_t>0$.
- Definition:
$$c_t = k_t^\alpha - i_t$$


- Calibration:
  - $\beta = 0.96$
  - $\delta = 0.1$
  - $\gamma = 4.0$
  - $\alpha = 0.3$
  - $U(x)=\frac{x^{1-\gamma}}{1-\gamma}$
  

---

Create a structure `Calibration` to hold the model parameters.

Vector of states: s
Vector of controls: x

Define two functions: 
- `transition(z::Float64, k::Float64, p::Calibration)::Tuple{Float64}` which returns productivity and capital at date `t+1` as a function of productivity, capital and investment at date `t`
- `arbitrage(z::Float64, k::Float64, i::Float64, Z::Float64, K::Float64, I::Float64, p::Calibration)::Float64` which returns the residual of the euler equation (lower case variable for date t, upper case for date t+1)

Using multiple dispatch, define two variants of the same function, that take vectors as input and output arguments:
- `arbitrage(s::Vector{Float64}, x::Vector{Float64}, S::Vector{Float64}, X::Vector{Float64}, p::Calibration)
- `transition(s::Vector{Float64}, x::Vector{Float64}, p::Calibration)


Write a function `steady_state(p::Calibration)::Tuple{Vector,Vector}` which computes the steady-state of the model computed by hand. It returns two vectors, one for the states, one for the controls. Check that the steady-state satisfies the model equations.


The first order system satisfies:
$$\begin{eqnarray}A s_t + B x_t + C S_{t+1} + D x_{t+1} & = & 0 \\\\ 
s_{t+1} & = & E s_t + F x_t
 \end{eqnarray}$$
$$

Define a structure `PerturbedModel` to hold matrices A,B,C,D,E,F.

Write a function `first_order_model(s::Vector, x::Vector, p::Calibration)::PerturbedModel`, which returns the first order model, given the steady-state and the calibration.

Use `jacobian` function from library `ForwardDiff`  to compute the first order model. Check that it coincides with the version computed by hand.

We look for a linear solution $x_t = X s_t$ . Write the matrix equation which `X` must satisfy. Write a function `residual(X::Array, M::PerturbedModel)` which computes the residual of this equation for a given `X`.

Write a function `T(X, M::PerturbedModel)`  which implements the time iteration step.

Write function `linear_time_iteration(X_0::Matrix, PerturbedModel)::Matrix` which implements the time iteration algorithm. Apply it to `X0 = rand(1,2)` and check that the result satisfies the first order model.

Define two linear operators `L_S(S::Matrix, X_0::Matrix, m::PerturbedModel)::Matrix` and `L_T(S::Matrix, X_0::Matrix, m::PerturbedModel)::Matrix` which implement the derivatives of the simulation and the time-iteration operator respectively.

Implement a function `spectral_radius(f::Function)::Float64` which implements the power iteration method to compute the biggest eigenvalues of the two previously defined operators. Check that Blamnchard-Kahn conditions are met.

Write a function `simulate(s0::Vector, X::Matrix, p::Calibration, T::Int64)::Tuple{Matrix, Matrix}` to simulate the model over $T$ periods (by using the formula $\Delta s_t = (E + F X) s_{t-11}$. Return a matrix for the states (one line per date) and another matrix for the controls. Bonus: add a keyword option to compute variables levels or log-deviations. If possible, return a DataFrame object.

Make some nice plots.