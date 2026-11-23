# A shape-conditioned regime brake

A rule that turns a market regime label into a daily position size between 0 and 1, built from five-minute returns of many assets and explained from the ground up in one notebook, [shape_conditioned_regime_brake.ipynb](shape_conditioned_regime_brake.ipynb).

It extends the sliced Wasserstein labeler of [sliced-wasserstein-regimes](https://github.com/FuratGoat/sliced-wasserstein-regimes), which sorts days into calm, transition and stress, with three additions.

- Projection directions from a scrambled Sobol sequence instead of random ones, and a measurement of how much that reduces the error of the sliced distance.
- A second fingerprint, taken after each asset's volatility is removed inside the window. The variance of a projection then becomes $\theta^\top R\, \theta$, and its spread over directions is set by the eigenvalues of the correlation matrix. Its most concentrated cluster is the crash shape.
- A release rule that cuts the re-entry wait short when volatility is falling, unless the market still has the crash shape.

The notebook tests each part on a synthetic market with planted breaks, including a crash in two legs with a bounce between them, and then shows the brake on real five-minute data for 23 ETFs and two spreads from 2018 to 2026.

The short version of the findings is that the parts do what they are built for on synthetic data, and the volatility state is stable on real data, but the crash-shape label is the weak point. It is named by comparing cluster averages that twice differed by less than 0.002, it moves with the choice of directions, and on real events it sometimes held the brake through a second leg (COVID, early March 2020) and sometimes missed one (Q4 2018).

The real market data cannot be redistributed, so the real-data section holds computed results only. Everything else runs from scratch with numpy, pandas, scipy, scikit-learn and matplotlib, in a few minutes, and check cells stop the run if a claim in the text does not hold.
