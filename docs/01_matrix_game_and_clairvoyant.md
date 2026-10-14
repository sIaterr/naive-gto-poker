# Matrix games and the clairvoyant game

**What this part does.** It derives the equilibrium of a zero-sum matrix game, then applies it to the smallest interesting poker game, the [*clairvoyant game*](glossary.md#clairvoyant-game). Along the way it shows (i) how to find an equilibrium by indifference, (ii) how dominance explains why the best hand should always bet, and (iii) how to certify an answer against the *full* game rather than a restricted one.

**Result in one line.** Pot $=1$, bet size $s$: the bettor bluffs with a fraction $\frac{s}{1+s}$ of its weak hands, the caller calls with probability $\frac{1}{1+s}$, and the value of the game to the caller is $\frac{1}{2(1+s)}$.

---

## 1. Poker vocabulary

Poker terms are defined in the [glossary](glossary.md). The terms used by the clairvoyant game are linked at their first use in Section 3.

---

## 2. Zero-sum matrix games

### 2.1 Setup

Player A (the **maximizer**) has pure strategies $1,\dots,m$. Player B (the **minimizer**) has pure strategies $1,\dots,n$. A *pure strategy* is a complete plan: an action at every decision point (in poker, for every hand held). The payoff matrix is

$$
M_{ij} = \text{payoff to A when A plays } i \text{ and B plays } j .
$$

(In a game with chance moves, $M_{ij}$ is the expectation over chance.) B's payoff is $-M_{ij}$.

A *mixed strategy* is a probability vector. The set of mixed strategies of A is the **probability simplex**

$$
\Delta^{m-1} = \Big\{ \sigma \in \mathbb{R}^m \;:\; \sigma_i \ge 0,\ \sum_{i=1}^m \sigma_i = 1 \Big\},
$$

where the exponent is its dimension (the sum-to-one constraint removes one degree of freedom). Its vertices are the pure strategies $e_1,\dots,e_m$. Likewise $\tau \in \Delta^{n-1}$ for B. If the players randomize independently, the expected payoff is

$$
\boxed{P(\sigma,\tau) = \sigma^\top M \tau = \sum_{i,j} \sigma_i M_{ij} \tau_j }
$$

which is **bilinear**: linear in $\sigma$ for fixed $\tau$, and linear in $\tau$ for fixed $\sigma$.

### 2.2 Best responses, minimax, saddle points

For fixed $\tau$, the map $\sigma \mapsto \sigma^\top M\tau$ is linear, and a linear function on a simplex is maximized at a vertex. Hence

$$
\max_{\sigma} \sigma^\top M\tau = \max_i (M\tau)_i, \qquad \min_{\tau} \sigma^\top M\tau = \min_j (\sigma^\top M)_j. \tag{1}
$$

A best response can always be taken pure. This is the reason evaluating payoffs at the corners of the strategy space captures everything.

**Minimax theorem (von Neumann, 1928).** For every real matrix $M$,

$$
v := \max_{\sigma}\min_{\tau}\ \sigma^\top M\tau \;=\; \min_{\tau}\max_{\sigma}\ \sigma^\top M\tau .
$$

A **saddle point** (equilibrium) is a pair $(\sigma^\star,\tau^\star)$ with

$$
\sigma^\top M\tau^\star \;\le\; \sigma^{\star\top} M\tau^\star \;\le\; \sigma^{\star\top} M\tau \qquad \text{for all } \sigma,\tau .
$$

It always exists, and $\sigma^{\star\top} M\tau^\star = v$. The **value** $v$ is unique, although the saddle point need not be.

### 2.3 The indifference principle

**Proposition 1.** Let $(\sigma^\star,\tau^\star)$ be a saddle point with value $v$. Then
- $(M\tau^\star)_i \le v$ for all $i$, with equality whenever $\sigma^\star_i \gt 0$;
- $(\sigma^{\star\top}M)_j \ge v$ for all $j$, with equality whenever $\tau^\star_j \gt 0$.

*Proof.* Take $\sigma = e_i$ in the left saddle inequality: $(M\tau^\star)_i \le v$. Now
$v = \sigma^{\star\top} M \tau^\star = \sum_i \sigma^\star_i (M\tau^\star)_i$ is a weighted average of numbers that are each $\le v$. It can only equal $v$ if every number with positive weight equals $v$. The statement for B is symmetric. $\square$

In words: **each player randomizes only among strategies that are equally good against the opponent's equilibrium strategy.** The equilibrium of a player is determined by what makes the *opponent* indifferent, not by that player's own payoffs.

### 2.4 Closed form for full-support equilibria

**Proposition 2.** Let $M$ be $n\times n$ and invertible with $\kappa := \mathbf 1^\top M^{-1}\mathbf 1 \neq 0$. Define

$$
\tau^\star = \frac{M^{-1}\mathbf 1}{\kappa}, \qquad \sigma^\star = \frac{M^{-\top}\mathbf 1}{\kappa}, \qquad v = \frac{1}{\kappa}.
$$

If $\sigma^\star \ge 0$ and $\tau^\star \ge 0$ entrywise, then $(\sigma^\star,\tau^\star)$ is a saddle point with value $v$. Conversely, any saddle point with full support on both sides is of this form.

*Proof.* The vectors sum to one by construction. We have $M\tau^\star = \frac1\kappa \mathbf 1 = v\mathbf 1$ and $M^\top\sigma^\star = v\mathbf 1$. So for any $\sigma$, $\sigma^\top M\tau^\star = v\,\sigma^\top\mathbf 1 = v$, and for any $\tau$, $\sigma^{\star\top}M\tau = v$. Both saddle inequalities hold with equality. Conversely, by Proposition 1 a full-support equilibrium satisfies $M\tau^\star = v\mathbf 1$, so $\tau^\star = vM^{-1}\mathbf 1$, and $\mathbf 1^\top\tau^\star=1$ fixes $v = 1/\kappa$. $\square$

**When this fails.** If some entry of $\sigma^\star$ or $\tau^\star$ is negative, no full-support equilibrium exists, and the equilibrium lives on a smaller *support* (a subset of the pure strategies). Then one applies the same formula to square submatrices of $M$ and checks the inequalities of Proposition 1 outside the support. If $M$ is not square, equilibria also live on supports. Finding the right support is the hard part in general; the algebra afterwards is a linear solve.

### 2.5 Certifying an answer: exploitability

For any pair $(\sigma,\tau)$ (not necessarily an equilibrium), define

$$
\varepsilon(\sigma,\tau) := \max_i (M\tau)_i - \min_j (\sigma^\top M)_j \;\ge\; 0 .
$$

By (1) the first term is what A earns by best-responding to $\tau$, and the second is what A earns when B best-responds to $\sigma$. We have $\max_i (M\tau)_i \ge v \ge \min_j(\sigma^\top M)_j$, so $\varepsilon \ge 0$, and $\varepsilon = 0$ exactly when $(\sigma,\tau)$ is a saddle point. This number, [**exploitability**](glossary.md#exploitability), is how much a pair of strategies loses to best responses, summed over both players (some authors divide by two). It is the accuracy measure used throughout this project, and it must always be computed in the **full** game, not a restricted version of it.

---

## 3. The clairvoyant game

### 3.1 Rules

- [Heads-up](glossary.md#heads-up), [river](glossary.md#river), [pot](glossary.md#pot) $=1$, [bet](glossary.md#bet) size $s \gt 0$.
- **A** holds one known hand, a [*bluff-catcher*](glossary.md#bluff-catcher): it beats [air](glossary.md#air) and loses to the [nuts](glossary.md#nuts).
- **B** holds the nuts with probability $\tfrac12$ and air with probability $\tfrac12$. B sees its own hand; A does not. (B effectively "sees through" A, since A's hand is fixed and known, which is why the game is called *clairvoyant*.)
- B acts first: [**bet**](glossary.md#bet) $s$ or [**check**](glossary.md#check).
- If B checks, there is a [showdown](glossary.md#showdown). (A has no option to bet.)
- If B bets, A [**calls**](glossary.md#call) or [**folds**](glossary.md#fold). There are no raises. If A calls there is a showdown.

### 3.2 Payoff convention

A's payoff is the chips A receives from the pot at the end of the street minus the chips A puts in on this street. Chips put in earlier are sunk and ignored. This gives

| B's hand | B's action | A's action | Outcome | Payoff to A |
|---|---|---|---|---|
| nuts | bet | fold | A gets nothing, pays nothing | $0$ |
| nuts | bet | call | A loses the bet | $-s$ |
| nuts | check | (none) | showdown, A loses | $0$ |
| air | bet | fold | A gets nothing, pays nothing | $0$ |
| air | bet | call | A wins pot $1$ and B's bet $s$ | $1+s$ |
| air | check | (none) | showdown, A wins pot | $1$ |

**Remark (constant-sum).** With this accounting the two players' payoffs sum to the dead money $1$ in the pot, so $u_A + u_B = 1$ rather than $0$. This is strategically identical to a zero-sum game: setting $u_B = 1 - P$, B maximizing $u_B$ is the same as B minimizing $P$, and adding a constant changes no best response. We therefore write $P := u_A$ and treat B as the minimizer of $P$.

### 3.3 The full payoff matrix

A has two pure strategies: $\mathsf f$ (fold) and $\mathsf c$ (call), both used only when facing a bet. B's pure strategy specifies an action for each hand: a pair (action with nuts, action with air), where $\mathsf b$ is bet and $\mathsf k$ is check. This gives four pure strategies $\mathsf{bk},\mathsf{bb},\mathsf{kk},\mathsf{kb}$. For example $\mathsf{bk}$ means "bet the nuts, check air".

The entry for each pair is the average of the table above over B's hand, with weights $\tfrac12$ each. For instance, $(\mathsf c, \mathsf{bk})$: $\tfrac12(-s) + \tfrac12(1) = \tfrac{1-s}{2}$.

$$
M \;=\;
\begin{array}{c|cccc}
 & \mathsf{bk} & \mathsf{bb} & \mathsf{kk} & \mathsf{kb} \\ \hline
\mathsf f & \tfrac12 & 0 & \tfrac12 & 0 \\[3pt]
\mathsf c & \tfrac{1-s}{2} & \tfrac12 & \tfrac12 & \tfrac{1+s}{2}
\end{array}
$$

Note that B has $2^2 = 4$ pure strategies for just two hands. For a range of $N$ hands with $k$ actions each the count is $k^N$, so the normal form will not scale; this is revisited in Section 8.

---

## 4. Dominance: why the nuts always bet

### 4.1 The principle

For the minimizer B, column $j$ **weakly dominates** column $j'$ if $M_{ij} \le M_{ij'}$ for every row $i$ (strictly for at least one $i$). Then B does at least as well with $j$ as with $j'$ *whatever A does*. For the maximizer A, row $i$ dominates row $i'$ if $M_{ij} \ge M_{i'j}$ for every column $j$.

### 4.2 The nuts always bet (the intuition)

Look at B holding the nuts, in the table of Section 3.2:

- If B **checks**, A receives $0$ whatever A does (A's bluff-catcher loses the showdown).
- If B **bets**, A receives $0$ if A folds and $-s$ if A calls.

Betting is therefore never worse for B and is strictly better whenever A calls with positive probability. The reason is simple: with the best hand, a bet can only add money to a pot you are going to win, while checking forgoes the chance of being paid off.

This comparison is *per hand*. Since the payoff is an average over B's two hands, the choice made with the nuts can be compared in isolation, whatever B does with air.

### 4.3 The same fact in the matrix

Compare columns that differ only in the action with the nuts:

- $\mathsf{bk}$ against $\mathsf{kk}$ (both check air): the rows are $(\tfrac12,\tfrac{1-s}{2})$ and $(\tfrac12,\tfrac12)$. Since $\tfrac{1-s}{2} \le \tfrac12$, **$\mathsf{bk}$ dominates $\mathsf{kk}$**, strictly in the row $\mathsf c$ when $s \gt 0$.
- $\mathsf{bb}$ against $\mathsf{kb}$ (both bet air): $(0,\tfrac12)$ against $(0,\tfrac{1+s}{2})$. **$\mathsf{bb}$ dominates $\mathsf{kb}$.**

So the columns $\mathsf{kk}$ and $\mathsf{kb}$ (the ones that check the nuts) can be removed.

### 4.4 Nothing else is dominated

- **Air.** Checking with air gives A a payoff of $1$ whatever A does. Betting gives $0$ if A folds and $1+s$ if A calls: better for B when A folds, worse when A calls. In the matrix, $\mathsf{bk}$ and $\mathsf{bb}$ are incomparable: row $\mathsf f$ prefers $\mathsf{bb}$ ($0 \lt \tfrac12$), row $\mathsf c$ prefers $\mathsf{bk}$ ($\tfrac{1-s}{2} \lt \tfrac12$). So whether to bluff is a genuine decision.
- **A.** Row $\mathsf c$ minus row $\mathsf f$ is $-\tfrac s2$ against $\mathsf{bk}$ and $+\tfrac12$ against $\mathsf{bb}$. Neither row dominates.

The reduced game (rows $\mathsf f,\mathsf c$; columns $\mathsf{bk},\mathsf{bb}$) has no dominance and no pure saddle point (no entry is both the minimum of its row and the maximum of its column when $s \gt 0$). So its equilibrium must be mixed.

### 4.5 Is removing a dominated column safe?

**Lemma 1.** Suppose column $j$ weakly dominates column $j'$ for B. If $(\sigma^\star,\tau^\star)$ is a saddle point of the game without column $j'$, then it is also a saddle point of the full game (with $\tau^\star$ extended by zero weight on $j'$), and the values agree.

*Proof.* Let $v$ be the value of the reduced game. The saddle conditions in the reduced game give $(\sigma^{\star\top}M)_k \ge v$ for every remaining column $k$ and $(M\tau^\star)_i \le v$ for every row $i$. The only new column to check is $j'$: $(\sigma^{\star\top}M)_{j'} \ge (\sigma^{\star\top}M)_j \ge v$ by dominance. So all saddle conditions of the full game hold. $\square$

**Caveat.** With *weak* dominance the converse can fail: the full game may have other equilibria that use the dominated column, and those would be lost. This is a real subtlety of dominance arguments. Here it does not happen, as Section 6 shows: A's optimal strategy is unique, and it makes both discarded columns strictly worse for B than the value.

### 4.6 What this argument depends on

The dominance of "bet with the nuts" is a property of **this tree**, not of poker in general. It relies on A having *no action after a check*. If A could bet after B checks, then checking the nuts could be better, because it can induce A to bet a weak hand into B (a "trap"). Dominance arguments must be re-examined whenever the tree changes.

---

## 5. Solving the reduced game

After Section 4 the game is $2\times 2$ with

$$
M_{\text{red}} = \begin{pmatrix} \tfrac12 & 0 \\[2pt] \tfrac{1-s}{2} & \tfrac12 \end{pmatrix}
\qquad\text{rows: } \mathsf f,\mathsf c;\quad \text{columns: } \mathsf{bk},\mathsf{bb}.
$$

Let $x$ be the probability that A calls, and let $y$ be the probability that B bets (bluffs) with air; B always bets the nuts. Then $\sigma = (1-x,\,x)$ and $\tau = (1-y,\,y)$. We derive the solution three ways; they must agree.

### 5.1 By hand: two indifference conditions

**B makes A indifferent between folding and calling.** Facing a bet, A folds for payoff $0$. Calling, A loses $s$ against a value bet (weight $\tfrac12$) and wins $1+s$ against a bluff (weight $\tfrac12 y$). Up to the common factor $\tfrac12$,

$$
-s\cdot 1 + (1+s)\cdot y = 0 \quad\Longrightarrow\quad y^\star = \frac{s}{1+s}.
$$

**A makes B indifferent between bluffing and checking with air.** If B checks with air, A's payoff is $1$. If B bluffs, A's payoff is $x(1+s)$ (A folds with probability $1-x$ for payoff $0$, calls with probability $x$ for payoff $1+s$). Setting these equal,

$$
x(1+s) = 1 \quad\Longrightarrow\quad x^\star = \frac{1}{1+s}.
$$

**Value.** Use the pure strategy $\mathsf f$, whose payoff against $\tau^\star$ must equal $v$: $v = \tfrac12(1-y^\star) = \dfrac{1}{2(1+s)}$.

### 5.2 By the matrix formula (Proposition 2)

$\det M_{\text{red}} = \tfrac14$, and

$$
M_{\text{red}}^{-1} = \begin{pmatrix} 2 & 0 \\ -2(1-s) & 2 \end{pmatrix},\qquad
M_{\text{red}}^{-1}\mathbf 1 = \begin{pmatrix} 2 \\ 2s \end{pmatrix},\qquad
\kappa = 2 + 2s .
$$

So
$$
\tau^\star = \Big(\tfrac{1}{1+s},\ \tfrac{s}{1+s}\Big),\qquad
\sigma^\star = \Big(\tfrac{s}{1+s},\ \tfrac{1}{1+s}\Big),\qquad
v = \frac{1}{2(1+s)},
$$
using $M_{\text{red}}^{-\top}\mathbf 1 = (2s,\,2)^\top$. All entries are nonnegative for every $s \gt 0$, so Proposition 2 applies.

### 5.3 By calculus, with a boundary

Work in the full game with **behavioral** coordinates: $x$ as before, $y_{\text{nuts}}$ and $y_{\text{air}}$ the probabilities that B bets with each hand, all in $[0,1]$. Reading the table of Section 3.2,

$$
P(x, y_{\text{nuts}}, y_{\text{air}}) \;=\; -\frac s2\, x\, y_{\text{nuts}} \;+\; \frac12\Big[(1+s)\,x\,y_{\text{air}} + 1 - y_{\text{air}}\Big].
$$

$P$ is affine in $x$ for fixed $(y_{\text{nuts}},y_{\text{air}})$ and affine in $(y_{\text{nuts}},y_{\text{air}})$ for fixed $x$. The partial derivatives are

$$
\frac{\partial P}{\partial x} = \frac12\big[-s\,y_{\text{nuts}} + (1+s)\,y_{\text{air}}\big],\qquad
\frac{\partial P}{\partial y_{\text{nuts}}} = -\frac s2\,x,\qquad
\frac{\partial P}{\partial y_{\text{air}}} = \frac12\big[(1+s)\,x - 1\big].
$$

The optimality conditions for a maximizer (A) and a minimizer (B) on $[0,1]$ are:

| Variable's position | A (maximizes over $x$) | B (minimizes over $y$) |
|---|---|---|
| interior | derivative $=0$ | derivative $=0$ |
| at $0$ | derivative $\le 0$ | derivative $\ge 0$ |
| at $1$ | derivative $\ge 0$ | derivative $\le 0$ |

*Finding the candidate.* If $x \gt 0$, then $\partial P/\partial y_{\text{nuts}} \lt 0$ strictly, so B pushes $y_{\text{nuts}}$ to the boundary $1$ with a **nonzero** derivative. This is exactly where "set the gradient to zero" fails, and where the boundary conditions in the table take over. (The case $x = 0$ is impossible: it would force $y_{\text{air}} = 1$, which makes $\partial P/\partial x \ge \tfrac12 \gt 0$, so A would want to increase $x$.) With $y_{\text{nuts}}=1$, the interior conditions $\partial P/\partial x = 0$ and $\partial P/\partial y_{\text{air}} = 0$ give

$$
y_{\text{air}}^\star = \frac{s}{1+s},\qquad x^\star = \frac{1}{1+s}.
$$

*Certifying it.* At $(x^\star,\,1,\,y_{\text{air}}^\star)$: $\partial P/\partial x = 0$, $\partial P/\partial y_{\text{air}} = 0$, and $\partial P/\partial y_{\text{nuts}} = -\tfrac{s}{2(1+s)} \lt 0$ at the upper bound. For fixed $y^\star$, $P$ does not depend on $x$ at all, so every $x$ (including $x^\star$) is a best response. For fixed $x^\star$, $P$ is affine in $y$ with slope $(-\tfrac{s x^\star}{2},\,0)$, so the minimum over $[0,1]^2$ is attained at $y_{\text{nuts}}=1$ with $y_{\text{air}}$ arbitrary, and $y^\star_{\text{air}}$ qualifies. Hence it is a saddle point of the full game, and again $v = P(x^\star,1,y^\star_{\text{air}}) = \frac{1}{2(1+s)}$.

---

## 6. Verification against the full game

The $2\times 4$ matrix of Section 3.3 contains the strategies we removed, so checking there is a check against the full game.

**A's strategy $\sigma^\star = \big(\tfrac{s}{1+s},\tfrac{1}{1+s}\big)$ against every column** (what B could achieve by any pure response):

| Column | $\sigma^{\star\top}M$ | Compared with $v=\frac{1}{2(1+s)}$ |
|---|---|---|
| $\mathsf{bk}$ | $\dfrac{1}{2(1+s)}$ | $= v$ |
| $\mathsf{bb}$ | $\dfrac{1}{2(1+s)}$ | $= v$ |
| $\mathsf{kk}$ | $\dfrac12$ | $\gt v$ |
| $\mathsf{kb}$ | $\dfrac12$ | $\gt v$ |

So B's best response to $\sigma^\star$ earns exactly $v$ (it can do no better), and the two columns that check the nuts are strictly worse.

**B's strategy $\tau^\star$ (weight $\tfrac{1}{1+s}$ on $\mathsf{bk}$, $\tfrac{s}{1+s}$ on $\mathsf{bb}$) against both rows:**

$$
(M\tau^\star)_{\mathsf f} = \tfrac12\cdot\tfrac{1}{1+s} = v,\qquad
(M\tau^\star)_{\mathsf c} = \tfrac{1-s}{2}\cdot\tfrac{1}{1+s} + \tfrac12\cdot\tfrac{s}{1+s} = \tfrac{1}{2(1+s)} = v .
$$

So A's best response to $\tau^\star$ earns exactly $v$. Therefore

$$
\varepsilon(\sigma^\star,\tau^\star) = v - v = 0 .
$$

**Numerical sanity check ($s=1$).** $\sigma^\star = (\tfrac12,\tfrac12)$, $\tau^\star=(\tfrac12,\tfrac12,0,0)$, $v=\tfrac14$. The columns give $(\tfrac14,\tfrac14,\tfrac12,\tfrac12)$ and the rows give $(\tfrac14,\tfrac14)$. ✓

**Uniqueness of A's strategy (closing the dominance caveat).** For a general $x$ (A's call probability), the payoffs of A against the four columns are $\tfrac12-\tfrac{s}{2}x$, $\tfrac x2$, $\tfrac12$, $\tfrac{1+s}{2}x$. The last two are never below the first two, so A's guaranteed payoff is $\min\{\tfrac12-\tfrac s2 x,\ \tfrac x2\}$. The first term decreases in $x$ and the second increases, so the maximum is attained at a single point, $x^\star = \tfrac{1}{1+s}$. Since every equilibrium strategy of B must be a best response to A's (unique) optimal strategy (Proposition 1), and $\mathsf{kk},\mathsf{kb}$ are strictly worse than $v$ against it, **no equilibrium of the full game ever checks the nuts.** The restriction "B always bets value" used in the first derivation is therefore not an assumption but a consequence.

---

## 7. Reading the result

$$
\boxed{\;x^\star = \frac{1}{1+s},\qquad y^\star = \frac{s}{1+s},\qquad v = \frac{1}{2(1+s)}\;}
$$

- **A's calling frequency** $x^\star = \frac{\text{pot}}{\text{pot}+\text{bet}}$ is the [*minimum defense frequency*](glossary.md#minimum-defense-frequency): A must call often enough that bluffing is exactly break-even for B (a bluff risks $s$ to win $1$, so it breaks even when A folds with probability $\frac{s}{1+s}$).
- **B's bluffing frequency.** $y^\star$ is the fraction of air hands that bluff. Because nuts and air have equal weight, it is also the ratio of [bluffs](glossary.md#bluff) to [value bets](glossary.md#value-bet) in B's betting range, $\text{bluff}:\text{value} = s:(1+s)$. Bluffs therefore make up a fraction $\frac{s}{1+2s}$ of all of B's bets.
- **The bet is worth something.** If B never bet, A would win the pot half the time, so $v=\tfrac12$. Equilibrium betting lowers A's value by $\tfrac12 - \tfrac{1}{2(1+s)} = \tfrac{s}{2(1+s)}$.

| $s$ | $x^\star$ | $y^\star$ | $v$ | bluff share of B's bets |
|---|---|---|---|---|
| $\tfrac12$ | $\tfrac23$ | $\tfrac13$ | $\tfrac13$ | $\tfrac14$ |
| $1$ | $\tfrac12$ | $\tfrac12$ | $\tfrac14$ | $\tfrac13$ |
| $2$ | $\tfrac13$ | $\tfrac23$ | $\tfrac16$ | $\tfrac25$ |

Limits: as $s\to 0$, A always calls, B never bluffs, and $v\to\tfrac12$. As $s\to\infty$, A almost never calls, B bluffs with nearly all air, and $v\to 0$.

---

## 8. Limits of this part, and what comes next

**Assumptions that did hidden work here:**
1. **A has no action after a check.** This is what makes "bet the nuts" dominant (Section 4.6).
2. **No raises** and a **single bet size.**
3. **A holds one known hand.** Real [ranges](glossary.md#range) have many hands on both sides.
4. **Fixed prior** $\tfrac12/\tfrac12$ on B's hand.
5. **Dominance was weak.** Safe here only thanks to the uniqueness argument of Section 6.

**Structural limits of the method as developed so far:**
- **Normal form does not scale.** With $N$ hands and $k$ actions per hand a player has $k^N$ pure strategies, so $M$ cannot even be written down. The general framework has to be the **sequence form**, where a strategy is the vector of probabilities of reaching each action sequence, $P = r_A^\top M\, r_B$ is exactly bilinear, and the constraints are linear. Its size is linear in $N$.
- **Interior solutions are not guaranteed.** The saddle of the reduced game was interior, but already the full game had a boundary component ($y_{\text{nuts}}=1$). In general, finding the *support* is the difficult step, not the algebra.
- **River structure may help.** Hands are totally ordered by showdown strength and equilibria are essentially threshold strategies, so the unknowns may collapse to a few thresholds and mixing frequencies determined by indifference conditions. That is the topic of the next parts.

**Next:** Part 2 introduces the sequence form and the first continuous-range spot (strengths uniform on $[0,1]$, one bet size, no raises), whose closed form is known, so the machinery can be validated before comparing with CFR and an exact LP.

---

## Appendix A: Reduced coordinates (optional)

This is a derivation device that makes the role of the interaction between the players explicit. The general solver does not use it.

**Elimination.** Drop the first component of each simplex: $x = (\sigma_2,\dots,\sigma_m)$, $y=(\tau_2,\dots,\tau_n)$, so that $\sigma_1 = 1-\mathbf 1^\top x$ and $\tau_1 = 1 - \mathbf 1^\top y$. Substituting into $\sigma^\top M\tau$ and collecting terms by type gives

$$
P(x,y) = a + b^\top x + c^\top y + x^\top D\, y,
$$

$$
a = M_{11},\qquad b_k = M_{k1}-M_{11},\qquad c_l = M_{1l}-M_{11},\qquad D_{kl} = M_{kl}-M_{k1}-M_{1l}+M_{11}.
$$

($b$: effect of A's choice alone; $c$: effect of B's choice alone; $D$: a mixed second difference measuring how much A's and B's choices **interact**. If $D=0$ there is no interior saddle, only dominance.)

**Biaffine, not bilinear.** For fixed $y$, $P$ is *affine* in $x$ (linear plus the constant $a + c^\top y$), not linear. The reason is that $x\mapsto\sigma = (1-\mathbf 1^\top x,\ x)$ is an affine map, not a linear one, and a bilinear form composed with affine maps is **biaffine**. Equivalently, with $\tilde x = (1,x)$ and $\tilde y=(1,y)$,

$$
P = \tilde x^\top \tilde M\,\tilde y,\qquad \tilde M = \begin{pmatrix} a & c^\top\\ b & D\end{pmatrix},
$$

which is bilinear in $(\tilde x,\tilde y)$, but $\tilde x,\tilde y$ are not free vectors (their first component is pinned to $1$).

**Interior saddle.** Setting $\nabla_x P = b + Dy = 0$ and $\nabla_y P = c + D^\top x = 0$, for square invertible $D$:

$$
x^\star = -D^{-\top}c,\qquad y^\star = -D^{-1}b,\qquad v = a - c^\top D^{-1}b .
$$

The Hessian $\begin{pmatrix}0 & D\\ D^\top & 0\end{pmatrix}$ has eigenvalues $\pm$ the singular values of $D$, so the critical point is a saddle (a maximum in $x$, a minimum in $y$).

**The clairvoyant game.** With base strategies $\mathsf f$ and $\mathsf{bk}$:

$$
a = \tfrac12,\qquad b = -\tfrac s2,\qquad c = -\tfrac12,\qquad d = \tfrac{1+s}{2}.
$$

The interaction $d$ is the extra benefit of calling over folding when B bluffs compared with when B does not: $\big(\tfrac12 - 0\big) - \big(\tfrac{1-s}{2}-\tfrac12\big) = \tfrac{1+s}{2}$. Then

$$
x^\star = -\frac cd = \frac{1}{1+s},\qquad y^\star = -\frac bd = \frac{s}{1+s},\qquad v = a - \frac{bc}{d} = \frac{1}{2(1+s)},
$$

agreeing with Section 5.

**Why this is not the main tool.** The coefficients depend on the arbitrary choice of base strategy (the saddle does not); the elimination does not generalize to the constraints $Er = e$, $r\ge 0$ of the sequence form, where Lagrange multipliers and LP duality are the natural tools; and plain stationarity covers only interior solutions.

---

## References

- J. von Neumann, "Zur Theorie der Gesellschaftsspiele", *Mathematische Annalen* 100 (1928): minimax theorem.
- W. Chen and J. Ankenman, *The Mathematics of Poker* (2006): the clairvoyant game and related toy models.
