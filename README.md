# Time Series Forecasting of Solar Radiation using LSTM

M.Tech project (Energy Science and Technology, IIT Hyderabad) that forecasts
solar irradiance using LSTM-based time series models trained on surface
radiation data from multiple US monitoring stations.

**Authors:** Shantanu Gupta (ET21MTECH11004)

---

## Overview

Solar power generation is growing rapidly worldwide, which makes short- and
long-term forecasting of solar irradiance important for planning, monitoring,
and distributing electricity generation. This project applies time-series
forecasting — primarily LSTM (Long Short-Term Memory) recurrent neural
networks — to predict future solar irradiance values based on the previous
year's data.

Solar radiation is seasonal and periodic on a daily basis, so the project
focuses on **short-term forecasts**, ranging from a few hours up to 10 days
ahead, using one year of historical data per location.

## Motivation

There was no readily usable pipeline for turning publicly available solar
irradiance data into inputs for photovoltaic output-power prediction. This
project builds that pipeline end to end — automated data acquisition,
cleaning, feature selection, and LSTM-based modelling — to demonstrate how
such forecasts could support power generation planning and grid management.

## Data

- **Source:** Daily surface radiation data from **7 stations** across the
  United States, collected from the [NOAA Global Monitoring Laboratory
  (GML) SURFRAD network](https://gml.noaa.gov/grad/surfrad/).
- **Stations used:** Bondville (IL), Boulder (CO), Desert Rock (NV), Fort
  Peck (MT), Goodwin Creek (MS), Penn State (PA), Sioux Falls (SD).
- **Granularity:** Raw records are available roughly every ~17 seconds
  (~520,000 observations/location/year). The project resamples this to
  **hourly** and **daily** averages for modelling.
- **Features:** 28+ features are available per record; this project focuses
  on **`net solar` radiation** as the primary univariate target for
  forecasting, since it is seasonal and well suited to sequence models.
- **Automation:** A small download script fetches the raw daily files per
  location/year directly from NOAA GML, avoiding tedious manual downloads.

![Sample of the raw downloaded data](assets/fig01_raw_data_sample.png)

## Approach

1. **Acquire** daily SURFRAD data per station/year via an automated
   download script.
2. **Clean & resample** to hourly/daily frequency; interpolate missing
   values; build a proper UTC `DatetimeIndex`.
3. **Explore** seasonality (yearly hump in radiation around summer) and
   daily periodicity in net solar radiation.
4. **Model** with LSTM networks (RNNs well suited to sequential/periodic
   data), iterating across a few architectures and lag configurations
   (e.g. 10-day and 30-day lag windows).
5. **Forecast** 24-hour, 10-day, and 30-day horizons and evaluate against
   held-out test data, per station and across all stations.

![Net solar radiation over one year](assets/fig02_net_solar_radiation_year.png)

## Results

- **24-hour and 10-day forecasts** performed reasonably well across
  stations.
- **30-day forecasts** did not perform well — expected, given only one
  year of training data was used per location.

| Forecast horizon | Result |
|---|---|
| 24 hours | Good |
| 10 days | Reasonably good |
| 30 days | Poor (limited by only 1 year of training data) |

A few example outputs:

![Test prediction across all models](assets/fig09_test_prediction_all_models.png)
![Ten days ahead forecast](assets/fig10_ten_days_ahead_forecast.png)
![Future forecast of solar irradiance](assets/fig11_future_forecast_solar_irradiation.png)
![Forecast for Boulder, Colorado](assets/fig15_forecast_boulder_colorado.png)

More figures are available in [`assets/`](assets).

## Conclusion

The project shows that LSTM models trained on a single year of SURFRAD data
can produce useful short-term (up to 10-day) solar irradiance forecasts.
Longer-horizon (30-day) forecasting would need substantially more training
data to become reliable.

## Repository contents

```
.
├── README.md
├── requirements.txt
├── assets/                     Figures referenced in this README (extracted from the report)
├── docs/
│   ├── project_report.pdf      Original project report (as submitted)
│   └── code_pages/             Page-by-page screenshots of the original notebook
└── notebook/
    └── LSTM_Modelling_OCR_DRAFT.py   OCR-transcribed draft of the notebook code (see note below)
```

### ⚠️ A note on the code in this repo

The original submission was a Jupyter notebook, but only the **PDF report**
(with the notebook printed as flat page images) was available when this
repository was put together — the source `.ipynb`/`.py` files were not
recoverable. `notebook/LSTM_Modelling_OCR_DRAFT.py` was produced by running
OCR over those page images, in page order, as a head start on retyping the
real thing.

**Treat it as a rough draft, not working code.** OCR on code is unreliable —
expect issues like `0`/`O`/`@` confusion, smart quotes instead of straight
quotes, dropped `In [ ]:` cell markers, and garbled output blocks. Before
relying on any line, cross-check it against the corresponding page image in
`docs/code_pages/` (page `NNN` in that folder lines up with the section
labelled `notebook page NNN` in the draft file). If you still have the
original notebook anywhere (old laptop, Google Drive, email attachments,
IIT-H Google Classroom submission, etc.), replacing this draft with the real
file is strongly recommended.

## Suggested requirements.txt

Reconstructed from the libraries visibly used in the notebook printouts —
verify/pin versions once you're running it again:

```
pandas
numpy
matplotlib
scikit-learn
tensorflow
requests
```

## References

1. Zhang, G. P. — *Time series forecasting using a hybrid ARIMA and neural
   network model*, Neurocomputing.
2. De Gooijer, J. G. & Hyndman, R. J. — *25 years of time series
   forecasting*, International Journal of Forecasting.
3. Ahmed, N. K. & Atiya, A. F. — *An Empirical Comparison of Machine
   Learning Models for Time Series Forecasting*, Econometric Reviews.
4. Hochreiter, S. & Schmidhuber, J. — *Long Short-Term Memory*, IEEE
   Journals.
5. Van Houdt, G., Mosquera, C. & Nápoles, G. — *A review on the long
   short-term memory model*, Artificial Intelligence Review.
6. Lindemann, B., Müller, T., Vietz, H., Jazdi, N. & Weyrich, M. — *A survey
   on long short-term memory networks for time series prediction*, Procedia
   CIRP.
7. Peng, L., Zhu, Q., Lv, S.-X. & Wang, L. — *Effective long short-term
   memory with fruit fly optimization algorithm for time series
   forecasting*, Methodologies and Applications.

## License

Add a license of your choice (e.g. MIT) if you intend for others to reuse
this work — see `LICENSE`.
