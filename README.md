# 2025 Infodengue-Mosqlimate dengue Forecast Sprint

<p align="center">
  <img src="img/logo_lacid.png" alt="Logo LaCiD" width="" height="">
</p>

[LaCiD](https://lacid.ccet.ufrn.br/) (Laboratório de Ciência de Dados - Data Science Lab) is the first research laboratory specializing in data science in Natal, Rio Grande do Norte, Brazil. It is a dedicated environment for developing research and innovation projects through data analysis, inference, and machine learning.

Our team collaborated together to participate in the 2025 Infodengue-Mosqlimate dengue Forecast Sprint.


## Team and Contributors

### Team Name

LaCiD/UFRN

### Names of all contributors

* [Marcus A. Nunes](https://lacid.ccet.ufrn.br/author/marcus-a.-nunes/) - Federal University of Rio Grande do Norte
* [Eliardo G. Costa](https://lacid.ccet.ufrn.br/author/eliardo-g.-costa/) - Federal University of Rio Grande do Norte
* [Marcelo Bourguignon](https://lacid.ccet.ufrn.br/author/marcelo-bourguignon/) - Federal University of Rio Grande do Norte
* [Thiago Valentim Marques](https://lacid.ccet.ufrn.br/author/thiago-valentim-marques/) - Federal University of Rio Grande do Norte
* [Thiago Zaqueu Lima](https://lacid.ccet.ufrn.br/author/thiago-zaqueu-lima/) - Federal University of Rio Grande do Norte


## Repository Structure

The code used for the model fit are located in the folder `resultados`. The training and predictions for the validation sets 1, 2, and 3 are in files `Thiago-Marcelo_treino1.R`, `Thiago-Marcelo_treino2.R`, and `Thiago-Marcelo_treino3.R`. 

The files `01_setup.R`, `02_download.R`, and `03_eda.R` located in the root were used for exploratory data analysis.

File `99_sumissao.R` has the code used for the final predictions and for the submission of our results, without our private keys.

Due to github limitations, files larger than 100MB are not supplied in this repository. Therefore, the `data` folder must be filled with the files `dengue.csv` and `climate.csv`, available at [https://sprint.mosqlimate.org/data/](https://sprint.mosqlimate.org/data/).


## Libraries and Dependencies

Software: R version 4.5.1

Libraries: `tidyverse`, `forecast`, `data.table` and `patchwork`


## Data and Variables

### Which datasets and variables were used?

Only the datasets `dengue.csv` and `climate.csv`, available at the Sprint's FTP Server, were used.

The following variables were used:

* `casos`
* `temp_med`

### How was the data pre-processed?

Due to the data nature, the Box-Cox were used to transform the response variable. Since this transformation can only be applied to non-zero data, `casos+1` was used instead of `casos` if there were no dengue cases in any week for a particular state and each validation set. Moreover, a particular $\lambda$ was estimated for each state and each validation set.

Besides that, we used lagged observations of the transformed `casos` variable from 1 up to 3 lags.

We also used `temp_med` lagged by 1.

For target 3, it was need to report our forecasts for the epidemiological weeks from 202441 to 202540. However, the data set only covers 202441 to 202522. This means we need to input 18 values into the matrix that will generate our forecasts for the lagged `casos` and 22 for the lagged `temp_med`. We chose to fill the missing data using the rolling averages of the 12 weeks most recent weeks available (which is approximately 3 months).







### How were the variables selected? Please point to the relevant part of the code.

The variables and $p$, $q$, $P$, and $Q$ polynomial degrees were selected using stepwise selection. This was done through argument `setpwise` in `auto.arima` function.



## Model Training

### Description of how the model was trained

We used a ARIMAX model based on Xavier et al. (2025). We used as covariate lagged variables for the number of cases (1, 2, and 3 lags) and mean temperature (lag 1). The model was trained in the entire validation sets for each case.

### If applicable, describe any hyperparameter optimization techniques used

No hyperparameters were optimized.

### Indicate where the training code is located

The code used for the model fit are located in the folder `resultados`. The training and predictions for the validation sets 1, 2, and 3 are in files `Thiago-Marcelo_treino1.R`, `Thiago-Marcelo_treino2.R`, and `Thiago-Marcelo_treino3.R`. 


## References

Xavier, L. L., Pessanha, J. F. M., Honório, N. A., Ribeiro, M. S., Moreira, D. M., & Peiter, P. C. (2025). A incidência da dengue explicada por variáveis climáticas em municípios da Região Metropolitana do Rio de Janeiro. _Trends in Computational and Applied Mathematics_, 26, e01476. [https://doi.org/10.5540/tcam.2025.026.e01476](https://doi.org/10.5540/tcam.2025.026.e01476)

## Data Usage Restriction

Our model was fitted up to EW 25 of the current year and we extrapolated the prediction to more steps ahead when compared to the other validation datasets.


## Predictive Uncertainty

The prediction intervals were calculated though simulation. 1000 responses were simulated for each fitted model and the median and respective prediction interval quantiles were calculated.
















