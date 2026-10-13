# Naive Nash: A Naive Approach to GTO Poker

> Can heads-up poker spots be solved analytically by exploiting the structure of the
> payoff space, instead of running a numerical solver?

## Contents

1. [Key results](#key-results)
2. [Background: poker and game theory in five minutes](#background)
3. [The idea: bilinear payoffs](#the-idea)
4. [Case study 1: the clairvoyant game](#case-study-1-the-clairvoyant-game)
5. [Ground truth: CFR comparison](#ground-truth-cfr-comparison)
6. [Where the method breaks](#where-the-method-breaks)
7. [Next spots](#next-spots)
8. [Repository guide and reproduction](#repository-guide)

## Key results

_TODO: headline findings and one figure._

## Background

_TODO: heads-up, bluff, bluff-catcher, nuts, air, GTO / Nash equilibrium. See also [docs/glossary.md](docs/glossary.md)._

## The idea

_TODO: payoff `P(x, y) = a + b x + c y + d x y`, corner evaluation, saddle point._

## Case study 1: the clairvoyant game

_TODO: setup, derivation, result. Code: [notebooks/01_clairvoyant.ipynb](notebooks/01_clairvoyant.ipynb)._

## Ground truth: CFR comparison

_TODO: Code: [notebooks/02_cfr_baseline.ipynb](notebooks/02_cfr_baseline.ipynb)._

## Where the method breaks

_TODO: when bilinearity holds, hidden assumptions (e.g. "always bet value")._

## Next spots

_TODO._

## Repository guide

| Path | Contents |
|---|---|
| `notebooks/` | The code, one notebook per case study, with commentary |
| `figures/` | Images used in this README |
| `docs/glossary.md` | Poker terms for readers without a poker background |

To reproduce:

```bash
python3 -m venv .venv
source .venv/bin/activate
pip install -r requirements.txt
jupyter notebook
```
