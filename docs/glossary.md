# Glossary

Terms are defined here the first time they are needed, for readers with no poker background. Poker terms come first, followed by a few game-theory terms. Entries are in alphabetical order within each group.

## Poker terms

## Air
A hand that loses at [showdown](#showdown) to everything the opponent can plausibly hold. Air can only win if the opponent folds, which is why betting with it is a [bluff](#bluff).

## Bet
Putting chips into the middle. Its size is measured as a fraction of the [pot](#pot); in the examples the bet size is $s$.

## Bluff
A [bet](#bet) made with a hand that would lose at [showdown](#showdown) (typically [air](#air)), hoping the opponent [folds](#fold).

## Bluff-catcher
A hand that beats bluffs but loses to [value bets](#value-bet). It can only win by [calling](#call) a bettor who sometimes bluffs.

## Call
Matching the opponent's [bet](#bet) to stay in the hand, which leads to a [showdown](#showdown).

## Check
Declining to [bet](#bet) when no bet has been made. If both players check on the [river](#river), there is a showdown.

## Fold
Giving up the hand and the [pot](#pot) rather than matching a [bet](#bet).

## GTO (game-theory optimal)
A Nash equilibrium strategy; it cannot be exploited.

## Heads-up
A game with exactly two players.

## Nuts
The best possible hand. It cannot lose at [showdown](#showdown).

## Polarized range
A [range](#range) consisting only of very strong hands (such as the [nuts](#nuts)) and very weak hands (such as [air](#air)), with little in between.

## Pot
The chips already in the middle before the betting round in question starts. In the examples it is normalized to $1$.

## Range
The set of hands a player may hold in a given situation, together with their probabilities.

## River
The last betting round. All cards have been dealt and nothing more will be revealed, so there are no chance events left after the deal.

## Showdown
Both players reveal their hands and the better hand wins the [pot](#pot).

## Value bet
A [bet](#bet) made with a hand that is likely to win if [called](#call).

## Game-theory terms

## Exploitability
How much a pair of strategies loses against best responses, summed over both players. It is zero exactly at an equilibrium. Defined formally in Part 1, Section 2.5.

## Minimum defense frequency
The fraction of the time a player must [call](#call) so that the opponent's [bluffs](#bluff) just break even. For a bet of size $s$ into a pot of $1$ it equals $\frac{1}{1+s}$, that is, $\frac{\text{pot}}{\text{pot}+\text{bet}}$.
