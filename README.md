# econ5200-lab07-data-portfolio
# Diagnosing Broken Confidence Intervals

## Objective
I diagnosed three confidence intervals that looked fine but were wrong, and measured how often each one actually covered the true value.

## Methodology
- Bootstrapped the median of 32,561 integer ages from the UCI Adult census data. The bootstrap SE came out at 0, and BCa returned nan with a warning instead of an interval.
- Measured coverage of nominal 95% intervals on right-skewed simulated data (n = 50, 400 experiments), for both the mean and the median, using parametric and bootstrap intervals.
- Bootstrapped wages from 25 firms as if the workers were independent, then resampled whole firms instead, and compared the two intervals over 200 simulated panels.
- Used a `bootstrap_utils.py` module with percentile, i.i.d. and cluster bootstrap functions.
- Had an AI write a coverage function, revised my prompt once, and checked the function against the coverage I had measured for the firm wages.

## Key Findings
- The zero SE for median age happens because ages are whole numbers with many ties at the median, so the resampled median never moves. It does not mean the median is known exactly. BCa returned nan because the jackknife also never moves.
- For the mean, coverage was 92.5% for the parametric interval and 92.0% for the bootstrap. Both fall short of 95%, and the bootstrap did not fix the skew.
- For the median, coverage was 66.8% for the parametric interval and 92.8% for the bootstrap. The parametric interval is built around the mean, so it misses the median on skewed data.
- Bootstrapping workers as independent gave an interval 5.7x too narrow and 19.5% coverage. Resampling whole firms brought coverage to 91.0%.
- The AI's first coverage function reused one seed for every repetition, so it repeated the same trial. After I fixed the seeding, it matched my numbers exactly: 19.5% for the worker bootstrap and 91.0% for the firm bootstrap.
