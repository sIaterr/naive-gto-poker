# Matrix games

**What this part does.** It sets up the methods used throughout the project. Section 1 shows how to find the equilibrium of a zero-sum matrix game and how to certify that an answer really is one: best responses, the minimax theorem, indifference, a closed form, and exploitability. Section 2 then asks what the payoff looks like *around* the equilibrium. Because the payoff is bilinear, the answer is an exact formula governed by a single matrix $D$, and this matrix is the object the rest of the project studies. The [clairvoyant game](clairvoyant_game.md) is the first worked example.

---

## 1. Zero-sum matrix games

### 1.1 Setup

Player A (the **maximizer**) has pure strategies $1,\dots,m$. Player B (the **minimizer**) has pure strategies $1,\dots,n$. A *pure strategy* is a complete plan: an action at every decision point (in poker, for every hand held). The payoff matrix is

$$
M_{ij} = \text{payoff to A when A plays } i \text{ and B plays } j .
$$

(In a game with chance moves, $M_{ij}$ is the expectation over chance.) B's payoff is $-M_{ij}$.

A *mixed strategy* is a probability vector. The set of mixed strategies of A is the **probability simplex**

$$
\Delta^{m-1} = \left\lbrace \sigma \in \mathbb{R}^m \ :\  \sigma_i \ge 0,\ \sum_{i=1}^m \sigma_i = 1 \right\rbrace,
$$

where the exponent is its dimension (the sum-to-one constraint removes one degree of freedom). Its vertices are the pure strategies $e_1,\dots,e_m$. Likewise $\tau \in \Delta^{n-1}$ for B. If the players randomize independently, the expected payoff is

$$
\boxed{P(\sigma,\tau) = \sigma^\top M \tau = \sum_{i,j} \sigma_i M_{ij} \tau_j }
$$

which is **bilinear**: linear in $\sigma$ for fixed $\tau$, and linear in $\tau$ for fixed $\sigma$.

### 1.2 Best responses, minimax, saddle points

For fixed $\tau$, the map $\sigma \mapsto \sigma^\top M\tau$ is linear, and a linear function on a simplex is maximized at a vertex. Hence

$$
\max_{\sigma} \sigma^\top M\tau = \max_i (M\tau)_i, \qquad \min_{\tau} \sigma^\top M\tau = \min_j (\sigma^\top M)_j. \tag{1}
$$

A best response can always be taken pure. This is the reason evaluating payoffs at the corners of the strategy space captures everything.

**Minimax theorem (von Neumann, 1928).** For every real matrix $M$,

$$
v := \max_{\sigma}\min_{\tau}\ \sigma^\top M\tau \ =\  \min_{\tau}\max_{\sigma}\ \sigma^\top M\tau .
$$

A **saddle point** (equilibrium) is a pair $(\sigma^\star,\tau^\star)$ with

$$
\sigma^\top M\tau^\star \ \le\  \sigma^{\star\top} M\tau^\star \ \le\  \sigma^{\star\top} M\tau \qquad \text{for all } \sigma,\tau .
$$

It always exists, and $\sigma^{\star\top} M\tau^\star = v$. The **value** $v$ is unique, although the saddle point need not be.

### 1.3 The indifference principle

**Proposition 1.** Let $(\sigma^\star,\tau^\star)$ be a saddle point with value $v$. Then
- $(M\tau^\star)_i \le v$ for all $i$, with equality whenever $\sigma^\star_i \gt 0$;
- $(\sigma^{\star\top}M)_j \ge v$ for all $j$, with equality whenever $\tau^\star_j \gt 0$.

*Proof.* Take $\sigma = e_i$ in the left saddle inequality: $(M\tau^\star)_i \le v$. Now
$v = \sigma^{\star\top} M \tau^\star = \sum_i \sigma^\star_i (M\tau^\star)_i$ is a weighted average of numbers that are each $\le v$. It can only equal $v$ if every number with positive weight equals $v$. The statement for B is symmetric. $\square$

In words: **each player randomizes only among strategies that are equally good against the opponent's equilibrium strategy.** The equilibrium of a player is determined by what makes the *opponent* indifferent, not by that player's own payoffs.

### 1.4 Closed form for full-support equilibria

**Proposition 2.** Let $M$ be $n\times n$ and invertible with $\kappa := \mathbf 1^\top M^{-1}\mathbf 1 \neq 0$. Define

$$
\tau^\star = \frac{M^{-1}\mathbf 1}{\kappa}, \qquad \sigma^\star = \frac{M^{-\top}\mathbf 1}{\kappa}, \qquad v = \frac{1}{\kappa}.
$$

If $\sigma^\star \ge 0$ and $\tau^\star \ge 0$ entrywise, then $(\sigma^\star,\tau^\star)$ is a saddle point with value $v$. Conversely, any saddle point with full support on both sides is of this form.

*Proof.* The vectors sum to one by construction. We have $M\tau^\star = \frac1\kappa \mathbf 1 = v\mathbf 1$ and $M^\top\sigma^\star = v\mathbf 1$. So for any $\sigma$, $\sigma^\top M\tau^\star = v\ \sigma^\top\mathbf 1 = v$, and for any $\tau$, $\sigma^{\star\top}M\tau = v$. Both saddle inequalities hold with equality. Conversely, by Proposition 1 a full-support equilibrium satisfies $M\tau^\star = v\mathbf 1$, so $\tau^\star = vM^{-1}\mathbf 1$, and $\mathbf 1^\top\tau^\star=1$ fixes $v = 1/\kappa$. $\square$

**When this fails.** If some entry of $\sigma^\star$ or $\tau^\star$ is negative, no full-support equilibrium exists, and the equilibrium lives on a smaller *support* (a subset of the pure strategies). Then one applies the same formula to square submatrices of $M$ and checks the inequalities of Proposition 1 outside the support. If $M$ is not square, equilibria also live on supports. Finding the right support is the hard part in general; the algebra afterwards is a linear solve.

### 1.5 Certifying an answer: exploitability

For any pair $(\sigma,\tau)$ (not necessarily an equilibrium), define

$$
\varepsilon(\sigma,\tau) := \max_i (M\tau)_i - \min_j (\sigma^\top M)_j \ \ge\  0 .
$$

By (1) the first term is what A earns by best-responding to $\tau$, and the second is what A earns when B best-responds to $\sigma$. We have $\max_i (M\tau)_i \ge v \ge \min_j(\sigma^\top M)_j$, so $\varepsilon \ge 0$, and $\varepsilon = 0$ exactly when $(\sigma,\tau)$ is a saddle point. This number, [**exploitability**](glossary.md#exploitability), is how much a pair of strategies loses to best responses, summed over both players (some authors divide by two). It is the accuracy measure used throughout this project, and it must always be computed in the **full** game, not a restricted version of it.

---

## 2. The exact local form at a saddle

**What this section does.** Section 1 asked *where* the equilibrium is. This section asks what the payoff looks like *around* it. Because $P$ is bilinear, the answer is an exact formula and not an approximation.

### 2.1 Expanding around any point

Let

$$
T_m := \lbrace d\in\mathbb R^m \mid \mathbf 1^\top d = 0\rbrace
$$

be the **tangent space** of the simplex $\Delta^{m-1}$: the directions in which one can move without leaving the hyperplane $\mathbf 1^\top\sigma=1$. If $\sigma_0,\sigma\in\Delta^{m-1}$ then $\sigma-\sigma_0\in T_m$, because both sum to one. Write $\sigma=\sigma_0+\delta\sigma$ and $\tau=\tau_0+\delta\tau$ with $\delta\sigma\in T_m$ and $\delta\tau\in T_n$. Multiplying out the bilinear form gives

$$
P(\sigma_0+\delta\sigma,\ \tau_0+\delta\tau) \ =\  \underbrace{\sigma_0^\top M\tau_0}_{=\ P(\sigma_0,\tau_0)} \ +\ \delta\sigma^\top M\tau_0 \ +\ \sigma_0^\top M\ \delta\tau \ +\ \delta\sigma^\top M\ \delta\tau . \tag{2}
$$

This is an identity: no remainder, no smallness assumption. It holds for every pair of feasible deviations. The three terms are the first-order effect of A's move, the first-order effect of B's move, and an **interaction** term. For a general smooth function a Taylor expansion would continue with higher-order terms. Here it stops, because $P$ has degree at most one in each player's variable.

### 2.2 Around a saddle point

Let $(\sigma^\star,\tau^\star)$ be a saddle point with value $v$, and let $S_A=\lbrace i \mid \sigma^\star_i\gt 0\rbrace$ and $S_B=\lbrace j \mid \tau^\star_j\gt 0\rbrace$ be the supports. Define the **regrets**

$$
r^A_i := v-(M\tau^\star)_i \ \ge\ 0,\qquad r^B_j := (\sigma^{\star\top}M)_j - v\ \ge\ 0 .
$$

The regret of a pure strategy is how far it falls short of the value when the opponent plays the equilibrium strategy. By Proposition 1 the regrets are nonnegative and vanish on the supports.

Because $\mathbf 1^\top\delta\sigma=0$, we may subtract the constant $v$ inside the linear terms of (2):

$$
\delta\sigma^\top M\tau^\star=\sum_i\delta\sigma_i\big((M\tau^\star)_i-v\big)=-\sum_{i\notin S_A} r^A_i\ \delta\sigma_i,\qquad
\sigma^{\star\top}M\ \delta\tau=\sum_{j\notin S_B} r^B_j\ \delta\tau_j .
$$

**Proposition 3 (exact local form).** For every feasible deviation,

$$
P(\sigma^\star+\delta\sigma,\ \tau^\star+\delta\tau) \ =\ v\ -\sum_{i\notin S_A} r^A_i\ \delta\sigma_i\ +\sum_{j\notin S_B} r^B_j\ \delta\tau_j\ +\ \delta\sigma^\top M\ \delta\tau .
$$

Feasibility forces $\delta\sigma_i\ge 0$ for $i\notin S_A$ and $\delta\tau_j\ge 0$ for $j\notin S_B$ (an unused strategy cannot lose weight). So each of the two sums is a first-order penalty on the player who moves weight onto an unused strategy. If both equilibrium strategies have full support (an **interior saddle**), all regrets vanish and

$$
\boxed{\ P(\sigma^\star+\delta\sigma,\ \tau^\star+\delta\tau) \ =\ v+\delta\sigma^\top M\ \delta\tau\ }
$$

### 2.3 Coordinates: $a$, $b$, $c$, $D$

To compute, choose bases of the tangent spaces. Let

$$
Z_A=\big(e_2-e_1,\ \dots,\ e_m-e_1\big)\in\mathbb R^{m\times(m-1)},\qquad
Z_B=\big(e_2-e_1,\ \dots,\ e_n-e_1\big)\in\mathbb R^{n\times(n-1)} ,
$$

so column $k$ is $e_k-e_1$. Every $\delta\sigma\in T_m$ equals $Z_A\ dx$ for a unique $dx\in\mathbb R^{m-1}$, namely $dx_k=\delta\sigma_k$ for $k\ge 2$ (the first component is then forced by the sum-to-zero condition). So the points of the simplex are $\sigma=e_1+Z_Ax$ with $x=(\sigma_2,\dots,\sigma_m)$, and likewise $\tau=e_1+Z_By$. Take the base point $(\sigma_0,\tau_0)=(e_1,e_1)$ in (2):

$$
P(x,y)=a+b^\top x+c^\top y+x^\top D\ y,
$$

$$
a=e_1^\top Me_1,\quad b=Z_A^\top Me_1,\quad c=Z_B^\top M^\top e_1,\quad D=Z_A^\top MZ_B ,
$$

and entrywise

$$
a=M_{11},\qquad b_k=M_{k1}-M_{11},\qquad c_l=M_{1l}-M_{11},\qquad D_{kl}=M_{kl}-M_{k1}-M_{1l}+M_{11}.
$$

- $a$ is the payoff at the base corner, with both players on their base pure strategy.
- $b_k$ is the change in payoff when A moves from the base strategy to $k$ while B stays on the base strategy. Similarly $c_l$ for B.
- $D_{kl}$ is a mixed second difference: how much A's gain from switching to $k$ depends on which strategy $l$ B is playing. It is the only genuinely bilinear part, the **interaction**.

The function $P(x,y)$ is **biaffine**, not bilinear: for fixed $y$ it is affine in $x$ (linear plus a constant), because $x\mapsto\sigma=e_1+Z_Ax$ is an affine map and not a linear one.

**The saddle in coordinates.** The gradient in $x$ is $b+Dy=Z_A^\top M(e_1+Z_By)=Z_A^\top M\tau$, whose $k$-th entry is $(M\tau)_k-(M\tau)_1$. Likewise $c+D^\top x=Z_B^\top M^\top\sigma$ has entries $(\sigma^\top M)_l-(\sigma^\top M)_1$. So the stationarity conditions

$$
b+Dy^\star=0,\qquad c+D^\top x^\star=0
$$

are exactly the indifference conditions of Proposition 1, written in coordinates. When $D$ is square and invertible,

$$
x^\star=-D^{-\top}c,\qquad y^\star=-D^{-1}b,\qquad v=a-c^\top D^{-1}b ,
$$

which is Proposition 2 in other coordinates. Expanding around $(x^\star,y^\star)$ instead of the base corner, Proposition 3 reads

$$
P(x^\star+dx,\ y^\star+dy)=v+dx^\top D\ dy ,
$$

with the same matrix $D$ (the second-order part of a biaffine function does not depend on where it is expanded).

### 2.4 What is intrinsic and what is a convention

The numbers $a,b,c$ and the entries of $D$ depend on which strategy was chosen as base and on the basis $Z$. The intrinsic object is the bilinear form $(\delta\sigma,\delta\tau)\mapsto\delta\sigma^\top M\ \delta\tau$ on $T_m\times T_n$. If $Z_A$ is replaced by $Z_AG$ with $G$ invertible, then $D$ is replaced by $G^\top D$, and similarly on B's side. What survives such changes is the rank of $D$ and its kernels. The entries and the singular values do not survive.

Singular values need a choice of length on $T_m$ and $T_n$. With the Euclidean length of $\delta\sigma\in\mathbb R^m$ they are the singular values of $D=Z_A^\top MZ_B$ for **orthonormal** $Z_A,Z_B$. Equivalently, they are the nonzero singular values of $\Pi_AM\Pi_B$, with $\Pi_A=I-\tfrac1m\mathbf 1\mathbf 1^\top$ and $\Pi_B$ likewise. The columns $e_k-e_1$ used above are *not* orthonormal, since $\lVert Z_A\ dx\rVert^2=\lVert dx\rVert^2+(\mathbf 1^\top dx)^2$. So the singular values of $D$ in those coordinates measure a different norm. Both are legitimate, but a result must say which one it uses. This is the "choice of norm" caveat.

### 2.5 Non-uniqueness and the kernel of $D$

Take an interior saddle. A vector $\delta\tau=Z_B\ dy$ with $Dy=0$ satisfies $\delta\sigma^\top M\ \delta\tau=0$ for all $\delta\sigma\in T_m$. By Proposition 3, $P(\sigma,\tau^\star+\delta\tau)=v$ for every $\sigma$, so $\tau^\star+\delta\tau$ holds A to at most $v$: it is another optimal strategy for B (whenever it lies in the simplex). Conversely, if $\tau'$ is optimal for B, then $P(\sigma^\star,\tau')=v$ and $P(\sigma,\tau')\le v$ for all $\sigma$. Since $\sigma^\star$ has full support, $\sigma^\star+\delta\sigma$ is feasible for small $\delta\sigma$ of both signs, so $\delta\sigma^\top M\ \delta\tau'=0$ for all such $\delta\sigma$, hence $D\ dy'=0$. Near an interior saddle, then:

$$
\text{optimal strategies of B}=\tau^\star+Z_B\ker D\quad(\text{intersected with the simplex}),
$$

and symmetrically A's optimal strategies are $\sigma^\star+Z_A\ker D^\top$. The equilibrium is unique exactly when $\ker D$ and $\ker D^\top$ are trivial. These are the "indifferent hands" of the project notes.

### 2.6 What a deviation costs

**Proposition 4 (exploitability splits).** For every pair $(\sigma,\tau)$,

$$
\varepsilon(\sigma,\tau)=\ell_A(\sigma)+\ell_B(\tau),\qquad
\ell_A(\sigma):=v-\min_j(\sigma^\top M)_j\ \ge 0,\qquad
\ell_B(\tau):=\max_i(M\tau)_i-v\ \ge 0 .
$$

*Proof.* Add and subtract $v$ in the definition of $\varepsilon$. Nonnegativity is the inequality $\max_i(M\tau)_i\ge v\ge\min_j(\sigma^\top M)_j$ from Section 1.5. $\square$

So the exploitability is the sum of two terms, each depending on one player's own strategy only. $\ell_A(\sigma)$ is how much value A gives away against a best-responding opponent. The three questions of the project then read as follows (interior saddle).

1. **Fixed Nash opponent.** Take $\delta\tau=0$ in Proposition 3: $P=v$ exactly. Deviating within the support costs nothing. (With unused strategies, moving weight onto one costs its regret per unit.)

2. **Best-responding opponent.** With $\delta\sigma$ A's deviation,
$$
\ell_A(\sigma^\star+\delta\sigma)=v-\min_{\tau}P(\sigma^\star+\delta\sigma,\tau)=\max_{\tau\in\Delta^{n-1}}\ \delta\sigma^\top M(\tau^\star-\tau).
$$
This is a maximum of a function that is linear in $\tau$, so it is attained at a vertex $e_j$. In coordinates,
$$
\ell_A=\max_{j}\ dx^\top D\ (y^\star-y_j),
$$
where $y_j$ are the coordinates of $e_j$. It is piecewise linear and positively homogeneous of degree one in the deviation: **first order, with a kink at the equilibrium, not quadratic**. It is zero exactly when $\delta\sigma$ lies in the kernel of Section 2.5: if $\ell_A=0$ then $\delta\sigma^\top M(\tau-\tau^\star)\ge0$ for all $\tau$, and since $\tau^\star$ has full support the differences $\tau-\tau^\star$ fill a neighbourhood of $0$ in $T_n$, so a linear functional that is nonnegative there must vanish. How large the slope is in the worst direction is a quantitative question about $D$, and it is left for later.

3. **Both err.** The remaining term is $\delta\sigma^\top M\ \delta\tau=dx^\top D\ dy$. Its maximum over $\lVert dx\rVert\le\varepsilon_A$, $\lVert dy\rVert\le\varepsilon_B$ is $\varepsilon_A\varepsilon_B\ \sigma_{\max}(D)$ (in the chosen norm). This is the **range of the payoff surface**, and it is second order. It is not the exploitability.

### 2.7 Where assumptions do hidden work

- **Full support.** The clean form $v+\delta\sigma^\top M\delta\tau$ needs an interior saddle. On the boundary, the regret terms of Proposition 3 appear, and they are first order, independent of $D$.
- **Independent randomization.** Bilinearity needs $\sigma$ and $\tau$ to be independent. Correlated play breaks (2).
- **The norm.** Singular values and the shape of an $\varepsilon$-ball depend on the chosen length (Section 2.4).
- **The kernel.** Uniqueness and the strict positivity of $\ell_A$ outside a subspace both hinge on $\ker D$ and $\ker D^\top$ (Sections 2.5, 2.6).

---

## References

- J. von Neumann, "Zur Theorie der Gesellschaftsspiele", *Mathematische Annalen* 100 (1928): minimax theorem.
