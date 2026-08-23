# AT-LAST
### Asteroid Time-domain Lightcurve Analysis for Spin Tracking

**AT-LAST** is an automated pipeline for measuring asteroid rotation periods from the sparse, multiband time-domain photometry produced by the **Vera C. Rubin Observatory**. Rather than assigning a period from a single period-search method, AT-LAST combines several independent approaches and reports high-confidence solutions only when they agree and pass a series of data-quality and statistical checks. The pipeline is designed for the scale of the Legacy Survey of Space and Time (LSST), where millions of Solar System objects will accumulate irregularly sampled observations that cannot realistically be inspected by hand.

You can view the full pipeline results here: https://atlast.monitormyplanet.com/

## Pipeline

The current implementation processes Rubin asteroid photometry end-to-end. It first reduces the observed magnitudes by correcting for changing Sun–asteroid and observer–asteroid distances, light-travel time, and solar phase angle. Observing geometry is obtained from **NASA/JPL HORIZONS**, and the IAU `H,G` phase function is used to produce reduced absolute magnitudes that can be compared across observing epochs. 

AT-LAST then searches for periodicity using three complementary techniques:

- **Multiband Lomb–Scargle periodogram** — searches for periodic signals while using observations obtained through multiple Rubin filters.
- **Phase Dispersion Minimization (PDM)** — a model-independent method that tests how tightly the observations align when folded at a candidate period.
- **Higher-order Fourier fitting** — models more complex and asymmetric rotational light curves using multiple harmonics.

Each method returns its strongest candidate periods together with quantities such as amplitude, uncertainty, periodogram strength, phase coverage, dispersion, and goodness of fit. The pipeline accounts for common harmonic ambiguities, including half- and double-period solutions, before comparing the methods. 

## Confidence and Quality Control

A period is promoted to a high-confidence solution only when the independent searches agree within the pipeline's tolerance or an accepted harmonic relationship and the solution passes multiple quality gates. These tests evaluate factors including the number of observations per filter, phase coverage, Lomb–Scargle strength, Fourier significance, PDM dispersion, amplitude signal-to-noise, and period uncertainty. The pipeline additionally applies **Bayesian Information Criterion (BIC)** comparisons and a permutation-based **false-alarm probability (FAP)** test to assess whether the periodic model is meaningfully preferred over noise.

## Additional Characterization

For each asteroid, AT-LAST records the strongest candidate periods from all three methods along with amplitudes, uncertainties, observing baseline, number of nights, and filter coverage. It also fits an **IAU H,G solar phase curve** after accounting for rotational variability and enriches the results with information from the **JPL Small-Body Database**, including dynamical class, NEO/PHA status, and previously published rotation periods where available. 

## Rubin First-Look Results

Applied to **5,052 asteroids** from Rubin's early datasets, AT-LAST identified **523 high-confidence rotation periods**. Among 33 objects for which earlier work reported a single preferred period, the pipeline reproduced 32 corresponding to **97% agreement**. It also recovered previously reported ultra-fast rotators and identified additional high-confidence candidates for independent confirmation.

The broader goal of AT-LAST is to provide a scalable and reproducible way to turn Rubin's growing archive of sparse asteroid photometry into reliable measurements of asteroid spin, light-curve amplitude, and population-level physical properties without requiring manual analysis of every object.
