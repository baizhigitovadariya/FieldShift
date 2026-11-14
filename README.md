# FieldShift

**What could grow well on your land?** A North Dakota prototype for the NASA Space Apps Challenge 2026, "Adapting Farms with NASA Data".

Pick a county, pick a season (dry, typical, wet), say what equipment, water and markets you have, and get a ranked list of crops with the numbers behind each one.

## What is in here

| Path | What it is |
|---|---|
| `docs/index.html` | The web app. One page, no build step, no server. |
| `docs/data.js` | The app's data, written by the notebook. |
| `FieldShift_multicrop.ipynb` | The full pipeline: data, models, honest testing, scenarios, export. |
| `data/raw/` | Cached downloads (USDA yields and prices, NASA POWER weather, county outlines). |
| `data/processed/` | Tables the notebook writes (training table, test results, scenarios). |

## See the app

Open `docs/index.html` in a browser. To put it online with GitHub Pages: repo Settings, Pages, Source "Deploy from a branch", Branch `main`, folder `/docs`.

## Run the notebook

Local: `pip install pandas numpy scikit-learn matplotlib requests jupyter`, then open the notebook and run all cells from the repo folder. With the cached files it takes about 7 minutes.

Colab: upload the notebook, paste this repo's URL into `REPO_URL` in the first code cell, run all. With `REPO_URL` empty it downloads everything fresh.

## Data

* **NASA POWER**, one request per county center, 2000 to 2025: daily weather (MERRA-2, CERES), satellite rain (GPM IMERG) and modeled soil wetness (MERRA-2), rolled up to months.
* **USDA NASS Quick Stats** county yields for 8 crops (2000 to 2025), and North Dakota prices received.
* **US Census Bureau** county centers. County outlines from the public plotly/datasets GeoJSON.

## What the model can and cannot do

The model is a random forest per crop. It uses month by month weather, soil wetness in April and May, winter rain and satellite rain, and it predicts yield relative to the statewide trend line. It trains on 2001 onward.

* It is tested only on years it never saw: train on earlier years, predict a later one (2016 to 2024), then 2025 once at the end.
* In the 2016 to 2024 tests its error averages 14.6% of yield over the 8 crops. Using each county's own average gives 18.1%. The first version of the model gave 16.9%. It beats the county average for all 8 crops.
* On 2025 it is far better than the first version for spring wheat, barley, corn and canola, and worse for soybeans, dry peas and durum. Averaged over the crops the two tie (11.9% and 11.8%).
* In drought years the error is still about 20%.
* It needs the whole season's weather, so it is not a forecast. In the app it answers "what if the season is dry, typical or wet".
* The app's typical number is real reported data (the county's middle yield for 2020 to 2025). Only the dry and wet change comes from the model, and the wet change is the least reliable part.
* County averages are not field numbers. Sales per acre is not profit.
* Soil wetness here is NASA's modeled value. SMAP satellite soil moisture starts in 2015, too short to train on.

## Credits

Built by the FieldShift team for Space Apps 2026. AI assistance (Claude) was used to write code for the notebook and the web page.
