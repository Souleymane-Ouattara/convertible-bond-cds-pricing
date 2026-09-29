# Hybrid Credit–Equity Pricing of a CDS on a Convertible Bond

This project develops a quantitative framework for pricing credit protection linked to a convertible bond. The model accounts for the interaction between issuer default and the bondholder's optimal conversion decision.

The central idea is that CDS protection is only economically relevant when the issuer defaults before the convertible bond has been converted into equity.

## Project overview

A convertible bond combines a traditional bond component with an embedded equity conversion option. As a result, its credit exposure may disappear before maturity if the investor optimally converts the bond into shares.

The model introduces:

- $\tau$: the issuer's default time;
- $\tau_c$: the optimal conversion time;
- $p^*$: the probability that default occurs before conversion.

The key probability is therefore

$$
p^* = \mathbb{P}(\tau < \tau_c).
$$

The convertible-bond CDS spread is approximated by

$$
s_{\mathrm{CB}} \approx \lambda(1-R)p^*,
$$

where $\lambda$ is the default intensity and $R$ is the recovery rate.

## Methodology

The project combines the following methods:

1. estimation of historical equity volatility using four estimators:
   - close-to-close;
   - EWMA;
   - Parkinson;
   - Garman–Klass;
2. simulation of BNP Paribas share-price paths under a geometric Brownian motion;
3. estimation of the optimal conversion strategy with the Longstaff–Schwartz Least-Squares Monte Carlo method;
4. simulation of default times under a constant-intensity reduced-form model;
5. Monte Carlo estimation of $p^*$;
6. computation of the corresponding convertible-bond CDS spread;
7. sensitivity analysis with respect to spot price, volatility, interest rate, maturity, default intensity and conversion price.

## Case study

The numerical application uses a stylised five-year BNP Paribas convertible bond.

| Parameter | Base-case value |
|---|---:|
| Initial share price $S_0$ | EUR 89.23 |
| Nominal value | EUR 100 |
| Maturity $T$ | 5 years |
| Conversion price | EUR 111.54 |
| Conversion ratio | 0.8966 |
| Risk-free rate | 2.132% |
| Recovery rate $R$ | 40% |
| Default intensity $\lambda$ | 0.6933% |
| Historical volatility $\sigma$ | 25% |
| Monte Carlo paths | 100,000 |

## Main results

The base-case simulation produces:

$$
\widehat{p^*} = 0.02866
$$

and an estimated convertible-bond CDS spread of approximately

$$
s_{\mathrm{CB}} = 1.19 \text{ bps}.
$$

The spread is substantially below the standard five-year CDS spread used for the calibration. Within the model, this difference reflects the possibility that the bond is converted into equity before the issuer defaults.

The sensitivity analysis shows that:

- default intensity is the most influential parameter;
- maturity has a strong positive effect on the spread;
- a higher share price reduces the remaining credit exposure;
- volatility has a limited local effect in the selected base case;
- the conversion price has a moderate positive effect.

## Repository structure

```text
.
├── README.md
├── requirements.txt
├── notebooks/
│   ├── 01_volatility_estimation.ipynb
│   └── 02_convertible_cds_pricer.ipynb
├── data/
│   └── README.md
├── figures/
│   ├── volatility_estimators.png
│   └── sensitivity_tornado.png
└── report/
    └── convertible_bond_cds_pricing_report.pdf
```

## Running the notebooks

The notebooks were developed for Google Colab.

### Volatility estimation

Open [`01_volatility_estimation.ipynb`](notebooks/01_volatility_estimation.ipynb) and upload the required OHLC Excel file when prompted.

The notebook:

- cleans and sorts the market data;
- computes log returns;
- estimates annualised volatility;
- performs variance-stability tests;
- produces the volatility figures used in the report.

### CDS pricing

Open [`02_convertible_cds_pricer.ipynb`](notebooks/02_convertible_cds_pricer.ipynb).

The notebook:

- simulates equity-price paths;
- determines optimal conversion dates with Least-Squares Monte Carlo;
- simulates issuer default times;
- estimates $p^*$;
- computes the convertible-bond CDS spread;
- performs the sensitivity analysis.

## Report

The complete academic report, including the theoretical framework, implementation details, results and limitations, is available here:

[Read the full report](report/convertible_bond_cds_pricing_report.pdf)

## Model limitations

This is a stylised academic model. In particular:

- the convertible bond does not include coupons, issuer calls, investor puts or soft-call clauses;
- the default intensity is constant;
- default intensity and equity price are modelled independently;
- the CDS term structure is not bootstrapped;
- Monte Carlo estimates retain a small amount of numerical noise.

The results are intended for educational and research purposes and should not be interpreted as investment advice or market quotations.

## Authors

- Christ Ange Dylan Kouame
- Aboubakar Ouattara
- Souleymane Ouattara

Academic project completed as part of the M2 Actuarial Science programme at ISFA.

Supervisor: Prof. Ying Jiao.
