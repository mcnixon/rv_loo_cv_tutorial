# LOO-CV for radial velocity analysis

[![DOI](https://zenodo.org/badge/DOI/10.5281/zenodo.22779652.svg)](https://doi.org/10.5281/zenodo.22779652)

Tutorial notebooks on applying
leave-one-out cross-validation (LOO-CV) to radial velocity data.

LOO-CV scores a model by how well it predicts each measurement when that measurement is withheld. The notebooks work through
the calculation on two of the paper's case studies, using the Pareto-smoothed importance sampling
(PSIS) approximation to avoid refitting the model once per data point.

## The notebooks

### `gj4276_loo_cv.ipynb`

LOO-CV for the GJ 4276 system (Nagel et al. 2019), comparing a single eccentric planet against two
planets on circular orbits. This covers the whole procedure for an
uncorrelated noise model: forming the pointwise log-likelihood array, running `arviz.loo`, reading
the Pareto $k$ diagnostic, handling the two points where $k > 0.7$ by exact refitting, and
comparing the two models with a properly paired standard error.

### `gj357_gp_loo_cv.ipynb`

The same method extended to models with a Gaussian process, for the GJ 357 system
(Luque et al. 2019), comparing a three-planet model against a four-planet model that adds a
candidate signal at 87.3 d.

A GP couples every measurement to every other, so the likelihood no longer factorises and the
pointwise quantity LOO-CV needs becomes *conditional* predictive
density of each measurement given all the others. The notebook derives that array from the
published posterior samples, and then applies PSIS as in the uncorrelated case.

**Note: no fitting is performed.** The notebooks use pre-computed published posterior samples throughout.

## Data

The notebooks need an `auxiliary_data/` directory that is too large for this repository (about
310 MB). It is archived on Zenodo:

https://doi.org/10.5281/zenodo.22779652

Download the files from that record into a directory named `auxiliary_data/` alongside the
notebooks. It should contain:

| File | Used by |
|---|---|
| `loglikelihood_per_datapoint_single_planet.npy`, `..._two_planet.npy` | GJ 4276 |
| `loglikelihood_idx_4_left_out.npy`, `loglikelihood_idx_89_left_out.npy` | GJ 4276, exact refits |
| `single_planet_best_fit.txt`, `two_planet_best_fit.txt`, `CARMENES_RV_GJ4276.dat` | GJ 4276, figure |
| `GJ357_3P_GP_posterior.npz`, `GJ357_4P_GP_posterior.npz` | GJ 357 |
| `gj357_rvs.dat` | GJ 357 |

## Requirements

`numpy`, `scipy`, `matplotlib` and `arviz`. Both notebooks run under `arviz` 0.23 and `arviz` 1.1,
whose interfaces differ, and detect which is installed.

The stored outputs were produced with `arviz` 0.23.4.
