# Rossmann-Forecasting-Sales

Datasets referred to [Kaggle](https://www.kaggle.com/competitions/rossmann-store-sales/overview)
There are 2 versions of forecasts. TUE G3 was finished by 20 Oct 2023. Refined version is ongoing.

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






