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

Local: `pip install pandas numpy scikit-learn matplotlib requests jupyter`, then open the notebook and run all cells from the repo folder. With the cached files it takes about 3 minutes.

Colab: upload the notebook, paste this repo's URL into `REPO_URL` in the first code cell, run all. With `REPO_URL` empty it downloads everything fresh.

## Data

* **NASA POWER** daily weather (temperature, rain, sunlight, humidity, wind), April to September, 2010 to 2025, one cell per county.
* **USDA NASS Quick Stats** county yields for 8 crops, and North Dakota prices received.
* **US Census Bureau** county centers. County outlines from the public plotly/datasets GeoJSON.

## What the model can and cannot do

* It is tested only on years it never saw: train on earlier years, predict a later one (2016 to 2024), then 2025 once at the end.
* Across the 8 crops its error is about 6% lower than simply using each county's own average. It helps most for canola, durum and sunflower. It does not help for soybeans.
* Typical error is 14 to 21% of yield, depending on the crop.
* It needs the whole season's weather, so it is not a forecast. In the app it answers "what if the season is dry, typical or wet".
* The app's typical number is real reported data (the county's middle yield for 2020 to 2025). Only the dry and wet change comes from the model, and the wet change is the least reliable part.
* County averages are not field numbers. Sales per acre is not profit.

## Credits

Built by the FieldShift team for Space Apps 2026. AI assistance (Claude) was used to write code for the notebook and the web page.
