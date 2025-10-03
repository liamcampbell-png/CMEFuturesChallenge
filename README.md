# CME Futures Trading Competition

## Libraries Used
- *Numpy/Pandas*: For data processing, handling, and analysis
- *Scipy/Sklearn*: For linear models and feature pruning
- *ARCH*: For GARCH models
- *yfinance/Pandas-Datareader*: For data download
- *ploty*: For graphing information (preferred to matplotlib because it allows for hovering on info and zooming in natively)

## Methodology
- Bought continuous front month futures contract data for several tickers (Data from Kibot)
- Cleaned, spliced, and processed data into a useable format
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
