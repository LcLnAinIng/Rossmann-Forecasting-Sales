# Rossmann-Forecasting-Sales

Datasets referred to [Kaggle](https://www.kaggle.com/competitions/rossmann-store-sales/overview)
There are 2 versions of forecasts. TUE G3 was finished with Python by 20 Oct 2023. Refined version is ongoing using R.

# TUE G3 ipynb

## 1. Exploratory Analysis
Examined correlations between Assortment, Promotions, Holiday effects, Competition, and core time-series components.
Selected features based on _correlation_ strength.
Limited the training set to the 80th- 100th sales percentile to reduce the influence of extreme values.

## 2. Pre-processing
Scaled numeric features with _MaxAbsScaler_ to retain sparsity while controlling magnitude.
Added _Ridge_ regularisation to mitigate multicollinearity and overfitting.

## 3. Models Evaluated
| Model                       |  Train R² |  Test R²  |
| --------------------------- | :-------: | :-------: |
| Linear Regression           | **0.714** | **0.711** |
| Decision-Tree Regressor     | **0.999** | **0.847** |
| **Random-Forest Regressor** | **0.988** | **0.915** |

Random Forest offered the best bias–variance trade-off and is the recommended model for this version.

To see further detail, please refer to the [report](https://github.com/LcLnAinIng/Rossmann-Forecasting-Sales/blob/LcLnAinIng-refined-R-v1/2023.11.19%20ADS1002%20Tue%20Gp3%20Forecasting%20Sales%20for%20Rossmann%20Stores.pdf) and the [ipynb](https://github.com/LcLnAinIng/Rossmann-Forecasting-Sales/blob/LcLnAinIng-refined-R-v1/ADS1002%20TUE%20G3%20NoteBook%20FinAl.ipynb) file


# The refined version
> Goal – Re-implement the project in R ( fpp3 ecosystem) and build store-level forecasts with ARIMA and Dynamic Harmonic Regression (DHR), validated by store-wise cross-validation.

This project follows the methodology in [Forecasting: Principles and Practice (Hyndman & Athanasopoulos)](https://otexts.com/fpp3/dynamic.html).

## Preperation Steps
- Feature simplification
  - Converted Promotion- and Competition-related variables to binary flags (0 / 1) after confirming this retained the relevant signal while reducing model complexity.

* Handling closed days
 - Observed that stores are closed on Sundays; whenever Open == 0, set Sales = 0.

- Time-series exploration
  - Applied STL decomposition and uncovered two strong seasonal patterns:
    - Annual seasonality.
    - Weekly seasonality.
  - Noted clear holiday effects (e.g., Christmas).

- Variance stabilisation & diagnostics
  - Performed a _Box–Cox transformation_ to flatten the data.
  - Ran _Ljung-Box_ and _unit-root tests_ to check white-noise behaviour and the need for ordinary/seasonal differencing.
  - Inspected _ACF_ plots—significant spikes violate the 1-in-20 rule, confirming seasonal structure.


 ## Modelling Workflow
 - Model-tunning store
   - Selected one representative store to experiment with parameter choices before scaling up to all stores.

 - Baseline ARIMA
  - Fitted an ARIMA to the Box-Cox-transformed series. Achieved ***<ins> AIC = 7273.14 </ins>***
    
- Dynamic Harmonic Regression (DHR)
  - in order to obtain a more sophisticated model, DHR is applied
  - Added 2 Fourier Terms to capture the seasonal cycles:
    ```{r}
    ARIMA(box_cox(Sales, λ) ~
      fourier("year",  K = 9) +   # yearly seasonality
      fourier("week",  K = 3))    # weekly seasonality
    ```
  - AIC dropped to 5341, a ***26.6% improvement*** over the plain ARIMA
  - Omitted trend() because the underlying trend is non-linear.
  - Attempts to introduce additional regressors (Promotion, Competition, etc.) unexpectedly produced null models and are under investigation. 🫠

- Two-phase "gateway" logic
     1.  if `Open == 0` => predict `Sales == 0` ;
     2.  else => apply the DHR model
  - This structure also returned a null model, suggesting a deeper issue with the data or specification that is being debugged. 🤡


## Cross-Validation Plan - will do
- Fold definition - one fold per store
- Objective - ensure forecasting performance is robust across the diverse Rossmann store network


## Next Steps
To diagnose why DHR with exogenous regressors and the gateway architecture return null fits (check data sparsity, factor levels, or optimisation constraints).
A Neural Network may also be applied for forecasting

Please check the refined version [here](https://github.com/LcLnAinIng/Rossmann-Forecasting-Sales/blob/LcLnAinIng-refined-R-v1/Refined%20Rossmann%20Sales%20Forecasting.qmd). (It is still very messy. I will organise it once it is done.)



