# CME Futures Trading Competition

## Libraries Used
- *numpy/pandas*: For data processing, handling, and analysis
- *scipy/sklearn*: For linear models and feature pruning
- *arch*: For GARCH models
- *yfinance/pandas-datareader*: For data download
- *ploty*: For graphing information (preferred to matplotlib because it allows for hovering on info and zooming in natively)

## Methodology
- Bought continuous front month futures contract data for several tickers (Data from Kibot)
- Cleaned, spliced, and processed data into a useable format (including data from fred and yahoo finance)
- Developed various features for analysis including log prices, log volume, log returns, spreads, etc.
- Split data into 80/20 train/test
- Determined feature importance with random forest in a preprocessing step (reducing ~2400 features to ~600)
- Began iterating on a 2 stage model to separate drift from diffusion (General idea is to find a rough drift path and explain the more granular fluctations with diffusion)
- Tested various linear models and feature selection/regularization techniques including lasso, ridge, elastic net, pca, pls
- Tested training linear models on log squared residuals for diffusion estimation, later moved to GARCH models on residuals, finding APARCH to be the best fit
- Created backtesting functionality that emulates rules of the competition for each individual contract, later moved to vectorized version to trade on all contracts at once (and to significantly improve testing speed)
- Tried numerous portfolio allocation techniques including continuous Kelly criterion / Merton portfolio optimization (extends from Markowitz portfolio optimization in the multi-asset case)
- Adjusted drift in log returns for GBM adjustment (+ 1/2sigma^2 in the normal assumption)
- Graphed various metrics and displayed performance on out of sample testing
- Grid searched on hyperparameters in the test set (further confidence could have been gained by separating into a test set and validation set)
- Final result was a two stage drift/diffusion model that gave target positions for next day trading session

## Notes
- I ran out of time before the start of the competition to really refine the model, and there were a couple issues with the features and other areas, but the backtests showed promising results regardless so we will see how it turns out in its current state.
- I tried to add several more contracts to hopefully increase diversification and trading opportunities. The base version I made learned to essentially trade spreads between certain futures, and I was thinking adding more contracts would improve this result. However, my drift modeling wasn't capable enough to add so many more contracts. The quality of forecast decreased significantly to a simple average return essentially with little sensitivity (if any) to changes in macro and other features. A more sophisticated model would likely yield better results with more contracts, but I wasn't able to obtain that result with the time I had.
- I think after significant refinement is made to robustify these predictions, potentially including swapping the drift model for a Kalman filter and experimenting with Markov-Switching GARCH for diffusion, a next step would be to target intraday mean reversion within this strategy or implementing intraday price targets based on our long term trend, allowing us to capture alpha within the short term trends as well. I would also like to experiment with the Heston model rather than GARCH, since the Heston model the continuous time version and would probably mesh better with the rest of the model which is using continuous time principles.
