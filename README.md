<div align="center">

# Exchange Analysis: Optimizing a Stock-Trading Rule

### Python edition of the CEE 251L trading project, built on Multivarious

[![Duke](https://img.shields.io/badge/Duke-CEE%20251L-00539B?style=flat-square)](#about)
[![Python](https://img.shields.io/badge/Python-3776AB?style=flat-square&logo=python&logoColor=white)](#setup)
[![Multivarious](https://img.shields.io/badge/built%20on-Multivarious-555555?style=flat-square)](https://github.com/hpgavin/multivarious)

</div>

---

Design and optimize an automatic trading rule using 200 days of closing prices for 19 stocks, starting with $1,000. The goal is to maximize the value of cash plus holdings on day 200, then see how the rule holds up on the next 200 days.

This is the Python port of the course's MATLAB starter code, prepared while I was a teaching assistant for CEE 251L. It uses the optimizers in [Multivarious](https://github.com/hpgavin/multivarious) (Nelder-Mead, optimized random search, and SQP).

<div align="center">
<img src="example_images/exchange_analysis_fig12.png" width="58%" alt="Cash, investments, and shares owned over 200 trading days">
<img src="example_images/exchange_analysis_fig13.png" width="40%" alt="Buy and sell thresholds over 200 trading days">
<br><sub>Example run: portfolio value and holdings (left), adaptive buy and sell thresholds (right).</sub>
</div>

## The rule

Each stock's quality on day $d$ is a weighted sum of the velocity, acceleration, and volatility of its smoothed price,

$$
Q_{d,s} = q_1\,\dot{\bar p}_{d,s} + q_2\,\ddot{\bar p}_{d,s} + q_3\,v_{d,s},
$$

where the smoothed price is a running average with forgetting factor $\phi$. Stocks with quality above the buy threshold $B$ are bought with a fraction $f_c$ of available cash; stocks below the sell threshold $S$ are sold. The seven parameters $(q_1, q_2, q_3, f_c, \phi, B, S)$ are the design variables.

## Files

| File | Purpose |
|---|---|
| `exchange_opt.py` | Entry point: initial guess, bounds, and choice of optimizer (`nms`, `ors`, or `sqp`) |
| `exchange_analysis.py` | Trading simulation that returns the negative portfolio value after 200 days |
| `running_average.py` | Shows how the forgetting factor φ shapes smoothed price, velocity, and volatility |
| `stock_prices1.csv` | 200 days × 19 stocks of closing prices |
| `exchange-project.pdf` | Project assignment |
| `m_files/` | Original MATLAB source |

`exchange_analysis.py` marks five places where the rule can be changed: the quality equation, the sell decision, how cash is split among purchases, how thresholds evolve, and the minimum gap between buy and sell thresholds.

## Setup

```bash
pip install numpy matplotlib
pip install -e /path/to/multivarious
python exchange_opt.py
```

## About

Course materials for CEE 251L, Uncertainty, Design, and Optimization, at Duke University. The original MATLAB project is by Prof. Henri P. Gavin and Steve Lattanzio.
