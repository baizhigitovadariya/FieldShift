# FieldShift

**What could grow well on your land?** A North Dakota prototype for the NASA Space Apps Challenge 2026, "Adapting Farms with NASA Data".

A user friendly interface to estimate agricultural yield by crop type, where users can adjust their farm settings and define the season.

## What is in here

| Path | What it is |
|---|---|
| `docs/index.html` | The web app. |
| `docs/data.js` | The app's data, written by the notebook. |
| `FieldShift_multicrop.ipynb` | The full pipeline: data, models, honest testing, scenarios, export. |
| `data/raw/` | Cached downloads (USDA yields and prices, NASA POWER weather, county outlines). |
| `data/processed/` | Tables the notebook writes (training table, test results, scenarios). |

## See the app

Open `docs/index.html` in a browser.

## Run the notebook

Local: `pip install pandas numpy scikit-learn matplotlib requests jupyter`, then open the notebook and run all cells from the repo folder. With the cached files it takes about 7 minutes.

Colab: upload the notebook, paste this repo's URL into `REPO_URL` in the first code cell, run all. With `REPO_URL` empty it downloads everything fresh.

## Data

* **NASA POWER**, one request per county center, 2000 to 2025: daily weather (MERRA-2, CERES), satellite rain (GPM IMERG) and modeled soil wetness (MERRA-2), rolled up to months.
* **USDA NASS Quick Stats** county yields for 8 crops (2000 to 2025), and North Dakota prices received.
* **US Census Bureau** county centers. County outlines from the public plotly/datasets GeoJSON.

## What the model can and cannot do

We train one random forest per crop on county data from 2001 onward. Each model sees month by month weather, how wet the soil was in April and May, winter rain, and satellite rain. It predicts yield relative to the statewide trend, since yields keep climbing over time.

**How we tested it.** Every score comes from a year the model had never seen. We trained on earlier years and predicted the next one, for each year from 2016 to 2024. Then we ran 2025 once, at the very end.

**What we found.**

* Across the 8 crops, the model is off by 14.6% of yield on average. Just using each county's own average is off by 18.1%, and our first version was off by 16.9%. The model beats the county average for all 8 crops.
* 2025 was mixed. Compared with our first version, the model did much better on spring wheat, barley, corn and canola, and worse on soybeans, dry peas and durum. Averaged over all crops the two versions tie (11.9% and 11.8%).
* Drought years are still hard. The error there is about 20%.

**What to keep in mind.**

* This is not a forecast. The model needs the whole season's weather, so the app asks "what if the season is dry, typical or wet?" instead.
* The typical yield in the app is real reported data: each county's middle yield for 2020 to 2025. The model only supplies the change for a dry or wet season, and the wet change is the shakiest part.
* County averages are not field numbers, and sales per acre is not profit.
* The soil wetness is NASA's modeled value. SMAP satellite soil moisture only starts in 2015, which is too short to train on.

## Credits

Built by the FieldShift team for Space Apps 2026. AI assistance (Claude) was used to write code for the notebook and the web page.
