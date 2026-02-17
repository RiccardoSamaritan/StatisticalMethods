# Bike-Sharing Demand Forecasting: Statistical Methods Final Project

**Final Project for Statistical Methods Exam**  
*Master's Degree in Data Science and Artificial Intelligence, University of Trieste*

## Overview

This repository contains the final project analysis for the Statistical Methods course. The work implements a comparative study of parametric, semi-parametric, and machine learning approaches for modeling count-data response variables in hourly bike-sharing rental prediction, spanning Generalized Linear Models (GLM), Generalized Additive Models (GAM), and Multivariate Adaptive Regression Splines (MARS).

## Dataset

**Source**: Hourly bike-sharing rental data (2011-2012), **n = 17,379** observations

**Response Variable**: cnt (total hourly rentals)
- Right-skewed count distribution
- Over-dispersion: Var[cnt] > E[cnt]
- Temporal patterns across hours and seasons

**Predictors**:
- Continuous: temperature (temp), humidity (hum), windspeed
- Categorical: season (4 levels), weather (3 levels), workday (binary), hour (24 levels), year

**Data Quality**: 22 missing observations retained (weather-induced station closures).

## Methodology

### 1. Exploratory Data Analysis
- Univariate analysis: distributional characterization (skewness, kurtosis, Shapiro-Wilk tests)
- Bivariate analysis: correlation matrices, non-linear associations, heteroscedasticity assessment

### 2. Parametric GLMs
**Poisson GLM**: Baseline approach with log-link. Diagnostics reveal overdispersion (scaled deviance >> 1), necessitating alternative distributions.

**Negative Binomial GLM**: Var[cnt_i] = μ_i + φμ_i². Model selection via AIC. Optimal specification includes hour × workday interaction, yielding scaled deviance ≈ 1.0.

**Quasi-Poisson**: Robust variance specification treating dispersion parameter as nuisance.

### 3. Semi-Parametric GAM
Smooth functions via penalized regression splines:
- Cyclic cubic splines for hour effects (k=24) stratified by workday
- Smooth temperature effects
- Smoothing parameter selection via Generalized Cross-Validation

### 4. Machine Learning
**MARS**: Piecewise-linear basis expansions with forward-backward selection. Identifies interaction patterns and threshold effects through hinge functions.

**Tree-Based Methods**: Pruned regression trees and random forests. Variable importance via mean decrease in impurity.

## Model Evaluation

**Metrics**: RMSE, MAE, AIC/BIC on 25% test set (n=4,344)

**Cross-Validation**: 5-fold CV on training data (75%, n=13,035)

**Diagnostics**:
- GLM: Deviance/Pearson residuals, Q-Q plots, scale-location plots, influence diagnostics
- GAM: Partial dependence plots, concurvity assessment, EDF allocation

## Key Findings

- **Overdispersion**: Poisson assumptions violated; negative binomial substantially improves fit
- **Non-linearity**: GAM and machine learning outperform parametric GLM (~5-15% error reduction)
- **Parsimony vs. Fit**: Negative binomial GLM achieves optimal balance between interpretability and predictive performance
- Hour-of-day effects require smooth functions; categorical factors insufficient
- Tree-based methods identify non-smooth decision boundaries suggesting threshold phenomena

## Structure

```
├── data/hour.csv
├── univariate.Rmd              # EDA - univariate distributions
├── BivariateEDA.Rmd            # EDA - bivariate associations
├── BikeSharing_PoisModel.Rmd   # Poisson GLM
├── BikeSharing_NBModel.Rmd     # Negative Binomial GLM
├── gam_mars.Rmd                # GAM and MARS
├── BikeSharing_Trees.Rmd       # Tree-based methods
└── comparison_final.Rmd        # Model comparison
```

## Authors

* [Giovanni Oro](https://github.com/GiovanniOro666)
* [Riccardo Samaritan](https://github.com/RiccardoSamaritan)
* [Lorenzo Tonet](https://github.com/LorenzoTonet)


