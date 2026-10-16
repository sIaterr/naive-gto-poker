# The clairvoyant game

**What this part does.** It applies the methods of [matrix games](matrix_games.md) to the smallest interesting poker game, the *clairvoyant game*, in which one player's hand is known. It solves the game three ways (by indifference, by the matrix formula, and by calculus with a boundary), shows why the best hand always bets, and certifies the answer against the *full* game rather than a restricted one. It then uses the game as a laboratory for the exact local form: since $D$ is a single number here, every claim about the geometry around the equilibrium can be checked by hand (Section 6).

Poker terms are defined in the [glossary](glossary.md) and linked at their first use in Section 1.1.

---

## 1. The game

### 1.1 Rules

- [Heads-up](glossary.md#heads-up), [river](glossary.md#river), [pot](glossary.md#pot) $=1$, [bet](glossary.md#bet) size $s \gt 0$.
- **A** holds one known hand, a [*bluff-catcher*](glossary.md#bluff-catcher): it beats [air](glossary.md#air) and loses to the [nuts](glossary.md#nuts).
- **B** holds the nuts with probability $\tfrac12$ and air with probability $\tfrac12$. B sees its own hand; A does not. (B effectively "sees through" A, since A's hand is fixed and known, which is why the game is called *clairvoyant*.)
- B acts first: [**bet**](glossary.md#bet) $s$ or [**check**](glossary.md#check).
- If B checks, there is a [showdown](glossary.md#showdown). (A has no option to bet.)
- If B bets, A [**calls**](glossary.md#call) or [**folds**](glossary.md#fold). There are no raises. If A calls there is a showdown.

### 1.2 Payoff convention

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

### 1.3 The full payoff matrix

A has two pure strategies: $\mathsf f$ (fold) and $\mathsf c$ (call), both used only when facing a bet. B's pure strategy specifies an action for each hand: a pair (action with nuts, action with air), where $\mathsf b$ is bet and $\mathsf k$ is check. This gives four pure strategies $\mathsf{bk},\mathsf{bb},\mathsf{kk},\mathsf{kb}$. For example $\mathsf{bk}$ means "bet the nuts, check air".

The entry for each pair is the average of the table above over B's hand, with weights $\tfrac12$ each. For instance, $(\mathsf c, \mathsf{bk})$: $\tfrac12(-s) + \tfrac12(1) = \tfrac{1-s}{2}$.

$$
M \ =\ 
\begin{array}{c|cccc}
 & \mathsf{bk} & \mathsf{bb} & \mathsf{kk} & \mathsf{kb} \cr  \hline
\mathsf f & \tfrac12 & 0 & \tfrac12 & 0 \cr 
\mathsf c & \tfrac{1-s}{2} & \tfrac12 & \tfrac12 & \tfrac{1+s}{2}
\end{array}
$$

Note that B has $2^2 = 4$ pure strategies for just two hands. For a range of $N$ hands with $k$ actions each the count is $k^N$, so the normal form will not scale; this is revisited in Section 7.

---

## 2. Dominance: why the nuts always bet

### 2.1 The principle

For the minimizer B, column $j$ **weakly dominates** column $j'$ if $M_{ij} \le M_{ij'}$ for every row $i$ (strictly for at least one $i$). Then B does at least as well with $j$ as with $j'$ *whatever A does*. For the maximizer A, row $i$ dominates row $i'$ if $M_{ij} \ge M_{i'j}$ for every column $j$.

### 2.2 The nuts always bet (the intuition)

Look at B holding the nuts, in the table of Section 1.2:

- If B **checks**, A receives $0$ whatever A does (A's bluff-catcher loses the showdown).
- If B **bets**, A receives $0$ if A folds and $-s$ if A calls.

Betting is therefore never worse for B and is strictly better whenever A calls with positive probability. The reason is simple: with the best hand, a bet can only add money to a pot you are going to win, while checking forgoes the chance of being paid off.

This comparison is *per hand*. Since the payoff is an average over B's two hands, the choice made with the nuts can be compared in isolation, whatever B does with air.

### 2.3 The same fact in the matrix

Compare columns that differ only in the action with the nuts:

- $\mathsf{bk}$ against $\mathsf{kk}$ (both check air): the rows are $(\tfrac12,\tfrac{1-s}{2})$ and $(\tfrac12,\tfrac12)$. Since $\tfrac{1-s}{2} \le \tfrac12$, **$\mathsf{bk}$ dominates $\mathsf{kk}$**, strictly in the row $\mathsf c$ when $s \gt 0$.
- $\mathsf{bb}$ against $\mathsf{kb}$ (both bet air): $(0,\tfrac12)$ against $(0,\tfrac{1+s}{2})$. **$\mathsf{bb}$ dominates $\mathsf{kb}$.**

So the columns $\mathsf{kk}$ and $\mathsf{kb}$ (the ones that check the nuts) can be removed.

### 2.4 Nothing else is dominated

- **Air.** Checking with air gives A a payoff of $1$ whatever A does. Betting gives $0$ if A folds and $1+s$ if A calls: better for B when A folds, worse when A calls. In the matrix, $\mathsf{bk}$ and $\mathsf{bb}$ are incomparable: row $\mathsf f$ prefers $\mathsf{bb}$ ($0 \lt \tfrac12$), row $\mathsf c$ prefers $\mathsf{bk}$ ($\tfrac{1-s}{2} \lt \tfrac12$). So whether to bluff is a genuine decision.
- **A.** Row $\mathsf c$ minus row $\mathsf f$ is $-\tfrac s2$ against $\mathsf{bk}$ and $+\tfrac12$ against $\mathsf{bb}$. Neither row dominates.

The reduced game (rows $\mathsf f,\mathsf c$; columns $\mathsf{bk},\mathsf{bb}$) has no dominance and no pure saddle point (no entry is both the minimum of its row and the maximum of its column when $s \gt 0$). So its equilibrium must be mixed.

### 2.5 Is removing a dominated column safe?

**Lemma 1.** Suppose column $j$ weakly dominates column $j'$ for B. If $(\sigma^\star,\tau^\star)$ is a saddle point of the game without column $j'$, then it is also a saddle point of the full game (with $\tau^\star$ extended by zero weight on $j'$), and the values agree.

*Proof.* Let $v$ be the value of the reduced game. The saddle conditions in the reduced game give $(\sigma^{\star\top}M)_k \ge v$ for every remaining column $k$ and $(M\tau^\star)_i \le v$ for every row $i$. The only new column to check is $j'$: $(\sigma^{\star\top}M)_{j'} \ge (\sigma^{\star\top}M)_j \ge v$ by dominance. So all saddle conditions of the full game hold. $\square$

**Caveat.** With *weak* dominance the converse can fail: the full game may have other equilibria that use the dominated column, and those would be lost. This is a real subtlety of dominance arguments. Here it does not happen, as Section 4 shows: A's optimal strategy is unique, and it makes both discarded columns strictly worse for B than the value.

### 2.6 What this argument depends on

The dominance of "bet with the nuts" is a property of **this tree**, not of poker in general. It relies on A having *no action after a check*. If A could bet after B checks, then checking the nuts could be better, because it can induce A to bet a weak hand into B (a "trap"). Dominance arguments must be re-examined whenever the tree changes.

---

## 3. Solving the reduced game

After Section 2 the game is $2\times 2$ with

$$
M_{\text{red}} = \begin{pmatrix} \tfrac12 & 0 \cr  \tfrac{1-s}{2} & \tfrac12 \end{pmatrix}
\qquad\text{rows: } \mathsf f,\mathsf c;\quad \text{columns: } \mathsf{bk},\mathsf{bb}.
$$

Let $x$ be the probability that A calls, and let $y$ be the probability that B bets (bluffs) with air; B always bets the nuts. Then $\sigma = (1-x,\ x)$ and $\tau = (1-y,\ y)$. We derive the solution three ways; they must agree.

### 3.1 By hand: two indifference conditions

**B makes A indifferent between folding and calling.** Facing a bet, A folds for payoff $0$. Calling, A loses $s$ against a value bet (weight $\tfrac12$) and wins $1+s$ against a bluff (weight $\tfrac12 y$). Up to the common factor $\tfrac12$,

$$
-s\cdot 1 + (1+s)\cdot y = 0 \quad\Longrightarrow\quad y^\star = \frac{s}{1+s}.
$$

**A makes B indifferent between bluffing and checking with air.** If B checks with air, A's payoff is $1$. If B bluffs, A's payoff is $x(1+s)$ (A folds with probability $1-x$ for payoff $0$, calls with probability $x$ for payoff $1+s$). Setting these equal,

$$
x(1+s) = 1 \quad\Longrightarrow\quad x^\star = \frac{1}{1+s}.
$$

**Value.** Use the pure strategy $\mathsf f$, whose payoff against $\tau^\star$ must equal $v$: $v = \tfrac12(1-y^\star) = \dfrac{1}{2(1+s)}$.

### 3.2 By the matrix formula (Proposition 2)

$\det M_{\text{red}} = \tfrac14$, and

$$
M_{\text{red}}^{-1} = \begin{pmatrix} 2 & 0 \cr  -2(1-s) & 2 \end{pmatrix},\qquad
M_{\text{red}}^{-1}\mathbf 1 = \begin{pmatrix} 2 \cr  2s \end{pmatrix},\qquad
\kappa = 2 + 2s .
$$

So
$$
\tau^\star = \Big(\tfrac{1}{1+s},\ \tfrac{s}{1+s}\Big),\qquad
\sigma^\star = \Big(\tfrac{s}{1+s},\ \tfrac{1}{1+s}\Big),\qquad
v = \frac{1}{2(1+s)},
$$
using $M_{\text{red}}^{-\top}\mathbf 1 = (2s,\ 2)^\top$. All entries are nonnegative for every $s \gt 0$, so [Proposition 2](matrix_games.md) applies.

### 3.3 By calculus, with a boundary

Work in the full game with **behavioral** coordinates: $x$ as before, $y_{\text{nuts}}$ and $y_{\text{air}}$ the probabilities that B bets with each hand, all in $[0,1]$. Reading the table of Section 1.2,

$$
P(x, y_{\text{nuts}}, y_{\text{air}}) \ =\  -\frac s2\  x\  y_{\text{nuts}} \ +\  \frac12\Big[(1+s)\ x\ y_{\text{air}} + 1 - y_{\text{air}}\Big].
$$

$P$ is affine in $x$ for fixed $(y_{\text{nuts}},y_{\text{air}})$ and affine in $(y_{\text{nuts}},y_{\text{air}})$ for fixed $x$. The partial derivatives are

$$
\frac{\partial P}{\partial x} = \frac12\big[-s\ y_{\text{nuts}} + (1+s)\ y_{\text{air}}\big],\qquad
\frac{\partial P}{\partial y_{\text{nuts}}} = -\frac s2\ x,\qquad
\frac{\partial P}{\partial y_{\text{air}}} = \frac12\big[(1+s)\ x - 1\big].
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

*Certifying it.* At $(x^\star,\ 1,\ y_{\text{air}}^\star)$: $\partial P/\partial x = 0$, $\partial P/\partial y_{\text{air}} = 0$, and $\partial P/\partial y_{\text{nuts}} = -\tfrac{s}{2(1+s)} \lt 0$ at the upper bound. For fixed $y^\star$, $P$ does not depend on $x$ at all, so every $x$ (including $x^\star$) is a best response. For fixed $x^\star$, $P$ is affine in $y$ with slope $(-\tfrac{s x^\star}{2},\ 0)$, so the minimum over $[0,1]^2$ is attained at $y_{\text{nuts}}=1$ with $y_{\text{air}}$ arbitrary, and $y^\star_{\text{air}}$ qualifies. Hence it is a saddle point of the full game, and again $v = P(x^\star,1,y^\star_{\text{air}}) = \frac{1}{2(1+s)}$.

---

## 4. Verification against the full game

The $2\times 4$ matrix of Section 1.3 contains the strategies we removed, so checking there is a check against the full game.

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

**Uniqueness of A's strategy (closing the dominance caveat).** For a general $x$ (A's call probability), the payoffs of A against the four columns are $\tfrac12-\tfrac{s}{2}x$, $\tfrac x2$, $\tfrac12$, $\tfrac{1+s}{2}x$. The last two are never below the first two, so A's guaranteed payoff is $\min\left(\tfrac12-\tfrac s2 x,\ \tfrac x2\right)$. The first term decreases in $x$ and the second increases, so the maximum is attained at a single point, $x^\star = \tfrac{1}{1+s}$. Since every equilibrium strategy of B must be a best response to A's (unique) optimal strategy ([Proposition 1](matrix_games.md)), and $\mathsf{kk},\mathsf{kb}$ are strictly worse than $v$ against it, **no equilibrium of the full game ever checks the nuts.** The restriction "B always bets value" used in the first derivation is therefore not an assumption but a consequence.

---

## 5. Reading the result

$$
\boxed{\ x^\star = \frac{1}{1+s},\qquad y^\star = \frac{s}{1+s},\qquad v = \frac{1}{2(1+s)}\ }
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

## 6. Sensitivity analysis

Section 4 certified that $(x^\star,y^\star)$ is an equilibrium. This section asks how the payoff and the exploitability behave *near* it, using the exact local form of [matrix games, Section 2](matrix_games.md). The game is small enough to do everything by hand, so this is a check of the machinery rather than a source of new phenomena (see 6.7 for what a scalar example cannot show).

### 6.1 The coefficients $a$, $b$, $c$, $D$

Work in the reduced game (rows $\mathsf f,\mathsf c$; columns $\mathsf{bk},\mathsf{bb}$), where the equilibrium has full support. The coordinates are $x=P(\text{A calls})$ and $y=P(\text{B bets air})$, and the base strategies are $\mathsf f$ and $\mathsf{bk}$. With

$$
M_{\text{red}}=\begin{pmatrix}\tfrac12 & 0\cr \tfrac{1-s}{2} & \tfrac12\end{pmatrix}
$$

the formulas of [matrix games, Section 2.3](matrix_games.md) give

$$
a=M_{11}=\tfrac12,\qquad
b=M_{21}-M_{11}=-\tfrac s2,\qquad
c=M_{12}-M_{11}=-\tfrac12,\qquad
D=M_{22}-M_{21}-M_{12}+M_{11}=\tfrac{1+s}{2},
$$

so that

$$
P(x,y)=\tfrac12-\tfrac s2\ x-\tfrac12\ y+\tfrac{1+s}{2}\ xy . \tag{1}
$$

The saddle conditions $b+Dy=0$ and $c+Dx=0$ (all quantities are scalars here) give $y^\star=-b/D=\tfrac{s}{1+s}$ and $x^\star=-c/D=\tfrac{1}{1+s}$, and the value is $v=a-bc/D=\tfrac{1}{2(1+s)}$. These are the results of Section 3.

The interaction $D$ has a poker reading: it is the extra gain from calling instead of folding when B bluffs, compared with when B does not, $\big(\tfrac12-0\big)-\big(\tfrac{1-s}{2}-\tfrac12\big)=\tfrac{1+s}{2}$.

### 6.2 The exact identity, by hand

Put $x=x^\star+u$ and $y=y^\star+w$ in (1). The product term expands as

$$
\tfrac{1+s}{2}(x^\star+u)(y^\star+w)=\tfrac{1+s}{2}x^\star y^\star+\underbrace{\tfrac{1+s}{2}x^\star}_{=\ 1/2}\ w+\underbrace{\tfrac{1+s}{2}y^\star}_{=\ s/2}\ u+\tfrac{1+s}{2}\ uw .
$$

Collect terms:

- the coefficient of $u$ is $-\tfrac s2+\tfrac s2=0$;
- the coefficient of $w$ is $-\tfrac12+\tfrac12=0$;
- the constant is $\tfrac12-\tfrac{s}{2(1+s)}-\tfrac{s}{2(1+s)}+\tfrac{s}{2(1+s)}=\tfrac{1}{2(1+s)}=v$.

Hence

$$
\boxed{\ P(x,y)=v+\tfrac{1+s}{2}\ (x-x^\star)(y-y^\star)\ }\tag{2}
$$

for every $(x,y)\in[0,1]^2$, not only for small deviations: the identity is exact because $P$ is biaffine.

### 6.3 What a deviation costs

Write $u=x-x^\star$ and $w=y-y^\star$.

**Fixed Nash opponent ($w=0$).** By (2), $P=v$ for every $u$. A can over-call or under-call at no cost, as long as B stays put. This is the indifference principle, and it gives no protection against an opponent who adapts.

**Best-responding opponent.** A's loss is the amount of value A gives away,

$$
\ell_A(x)=v-\min_j(\sigma^\top M)_j=\max_{y\in[0,1]}\ u\ D\ (y^\star-y).
$$

The right-hand side is linear in $y$, so the maximum is at $y=0$ or $y=1$:

| A's deviation | B's best reply | A's loss |
|---|---|---|
| over-call, $u\gt0$ | never bluff, $y=0$ | $D\ y^\star\ u=\tfrac s2\ u$ |
| under-call, $u\lt0$ | always bluff, $y=1$ | $D\ (1-y^\star)\ \lvert u\rvert=\tfrac12\ \lvert u\rvert$ |

Check against the matrix directly. In Section 4, A's guaranteed payoff at call probability $x$ is $\min\big(\tfrac12-\tfrac s2x,\ \tfrac x2\big)$. Subtracting it from $v$ gives slope $\tfrac s2$ for $x\gt x^\star$ and slope $\tfrac12$ for $x\lt x^\star$, as in the table.

In poker terms: if A calls too often, B stops bluffing and A pays for calls that now only lose to value bets. If A calls too rarely, B bluffs everything. The loss is **linear in the size of the mistake, with a kink at the equilibrium**, not quadratic. Example: $s=1$, where $v=\tfrac14$. If A calls $60\%$ instead of $50\%$, the guaranteed payoff drops from $0.25$ to $0.20$, a loss of $20\%$ of the value, for a $10$-point mistake.

**The same for B.** B's loss from a wrong bluffing frequency is $\ell_B(y)=\max_{x\in[0,1]}D\ (x-x^\star)\ w$. If B over-bluffs ($w\gt0$), A always calls and B loses $D(1-x^\star)\ w=\tfrac s2\ w$. If B under-bluffs ($w\lt0$), A always folds and B loses $D\ x^\star\lvert w\rvert=\tfrac12\lvert w\rvert$.

### 6.4 Exploitability on the whole square

By Proposition 4 of [matrix games](matrix_games.md), $\varepsilon=\ell_A+\ell_B$:

$$
\varepsilon(x,y)=\begin{cases}\tfrac s2\ u & u\gt0\cr \tfrac12\lvert u\rvert & u\lt0\end{cases}\ +\ \begin{cases}\tfrac s2\ w & w\gt0\cr \tfrac12\lvert w\rvert & w\lt0\end{cases}
$$

for pairs in which B always bets the nuts. (The two columns $\mathsf{kk},\mathsf{kb}$ never attain the minimum in $\ell_A$, so the reduced game already gives the full-game exploitability, as in Section 4.) This function is piecewise linear with a single kink at $(x^\star,y^\star)$. Its sublevel sets $\lbrace\varepsilon\le\delta\rbrace$ are skewed diamonds with vertices at $u=2\delta/s$, $u=-2\delta$, $w=2\delta/s$, $w=-2\delta$ (before cutting by the square). Two consequences:

- The minimum is **sharp**: $\varepsilon$ grows linearly with the distance, so there is no quadratic bowl and a Hessian at the equilibrium would say nothing.
- **An observation from this one example.** The over-call slope is $D\ y^\star=\tfrac s2$. For small bets $s$ it tends to $0$, although $D\to\tfrac12$ stays far from zero. The slope is small because $y^\star$ is close to the vertex $y=0$: B's best reply "never bluff" is almost the equilibrium strategy. So the slope is governed by $D$ *together with the position of the equilibrium inside the simplex*, not by the singular values of $D$ alone. The hypothesis that the slope is set by the smallest singular value of $D$ is therefore too simple, and should be restated.

### 6.5 Both players err

By (2), $P-v=D\ uw$. Over a box $\lvert u\rvert\le\varepsilon_A$, $\lvert w\rvert\le\varepsilon_B$ its maximum is $D\ \varepsilon_A\varepsilon_B$, attained when $u$ and $w$ have the same sign (A over-calls while B over-bluffs, so A's calls catch more bluffs), and its minimum is $-D\ \varepsilon_A\varepsilon_B$. This is the range of the payoff surface. For $s=1$ and $\varepsilon_A=\varepsilon_B=0.1$ it is $0.01$, while A's loss for the single over-call of $0.1$ against a best response is $0.05$. The two quantities answer different questions: exploitability is first order in the mistake, and the cross term is second order.

### 6.6 The full game: the boundary term

In the full $2\times4$ game the equilibrium is on the boundary for B: the strategies $\mathsf{kk}$ and $\mathsf{kb}$ (check the nuts) have weight zero. A has full support. By Section 4 their regrets are

$$
r^B_{\mathsf{kk}}=r^B_{\mathsf{kb}}=\tfrac12-v=\tfrac{s}{2(1+s)} .
$$

Let A deviate by $\delta\sigma=(-u,\ u)$ and B by $\delta\tau=(\delta\tau_{\mathsf{bk}},\delta\tau_{\mathsf{bb}},\delta\tau_{\mathsf{kk}},\delta\tau_{\mathsf{kb}})$ with entries summing to zero and $\delta\tau_{\mathsf{kk}},\delta\tau_{\mathsf{kb}}\ge0$. Proposition 3 of [matrix games](matrix_games.md) gives, with $\delta\sigma^\top M\ \delta\tau=u\ (\text{row }\mathsf c-\text{row }\mathsf f)\cdot\delta\tau$,

$$
P=v+\tfrac{s}{2(1+s)}\big(\delta\tau_{\mathsf{kk}}+\delta\tau_{\mathsf{kb}}\big)+u\Big(-\tfrac s2\ \delta\tau_{\mathsf{bk}}+\tfrac12\ \delta\tau_{\mathsf{bb}}+\tfrac{1+s}{2}\ \delta\tau_{\mathsf{kb}}\Big).
$$

With $\delta\tau_{\mathsf{kk}}=\delta\tau_{\mathsf{kb}}=0$, $\delta\tau_{\mathsf{bk}}=-w$, $\delta\tau_{\mathsf{bb}}=w$ this reduces to (2). The new term is a **first-order penalty of $\tfrac{s}{2(1+s)}$ per unit of probability that B puts on checking the nuts**, and it does not involve $D$: it is the regret of the unused strategies. For $s=1$ every $1\%$ of probability on checking the nuts costs B $0.0025$ against A's equilibrium strategy, which is $1\%$ of the value $\tfrac14$. This is the quantitative form of "the nuts always bet".

### 6.7 What the scalar case cannot show

- **No spectrum.** $D$ is one number, $\tfrac{1+s}{2}\neq0$ for every $s\gt0$. There are no singular values to compare and no kernel, which is consistent with the uniqueness of the equilibrium found in Section 4.
- **The norm still sets the constants.** In the coordinates $(x,y)$ the sensitivity is $D=\tfrac{1+s}{2}$. In the Euclidean norm on the simplex, $\delta\sigma=(-u,u)$ has length $\sqrt2\ \lvert u\rvert$, so with an orthonormal tangent basis the singular value is $D/2=\tfrac{1+s}{4}$. The shape of a ball does not matter in one dimension, but its size does, so each result must state its convention (see [matrix games, Section 2.4](matrix_games.md)).
- **A single reply of B.** Only two vertices of B's simplex enter $\ell_A$, so the "worst case over vertices" step of Question 2 is trivial here. With many strategies it is a vertex enumeration or a linear program.

### 6.8 Numerical status

The identity (2), the two slopes, the separation $\varepsilon=\ell_A+\ell_B$ and the boundary formula of 6.6 were checked numerically for $s\in\lbrace0.5,\ 1,\ 2,\ 3.7\rbrace$ on random feasible deviations (agreement to $10^{-12}$). The check script is not yet part of the repository; it belongs in `notebooks/01_clairvoyant.ipynb`.


---

## 7. Limits of this part, and what comes next

**Assumptions that did hidden work here:**
1. **A has no action after a check.** This is what makes "bet the nuts" dominant (Section 2.6).
2. **No raises** and a **single bet size.**
3. **A holds one known hand.** Real [ranges](glossary.md#range) have many hands on both sides.
4. **Fixed prior** $\tfrac12/\tfrac12$ on B's hand.
5. **Dominance was weak.** Safe here only thanks to the uniqueness argument of Section 4.

**Structural limits of the method as developed so far:**
- **Normal form does not scale.** With $N$ hands and $k$ actions per hand a player has $k^N$ pure strategies, so $M$ cannot even be written down. The general framework has to be the **sequence form**, where a strategy is the vector of probabilities of reaching each action sequence, $P = r_A^\top M\  r_B$ is exactly bilinear, and the constraints are linear. Its size is linear in $N$.
- **Interior solutions are not guaranteed.** The saddle of the reduced game was interior, but already the full game had a boundary component ($y_{\text{nuts}}=1$). In general, finding the *support* is the difficult step, not the algebra.
- **River structure may help.** Hands are totally ordered by showdown strength and equilibria are essentially threshold strategies, so the unknowns may collapse to a few thresholds and mixing frequencies determined by indifference conditions. That is the topic of the next parts.

**Next:** the sequence form, where $M$ is no longer a small table and $D$ becomes a genuine matrix with structure. The first continuous-range spot (hand strengths uniform on $[0,1]$, one bet size, no raises) has a known closed form, so the local form of [matrix games](matrix_games.md) can be tested against it before the discretized $N$-hand spot. CFR is only an optional cross-check at the end.

---

## References

- W. Chen and J. Ankenman, *The Mathematics of Poker* (2006): the clairvoyant game and related toy models.
