# webscraping_project
# 🎬 IMDb Movie List Web Scraper & EDA

A Python project that scrapes movie data from a public **IMDb list** (50 pages, ~1000 titles) using `requests` + `BeautifulSoup`, cleans the resulting dataset with `pandas`/`numpy`, and performs basic exploratory data analysis with `matplotlib`/`seaborn`.

---

## 📌 Overview

This notebook scrapes an IMDb custom list page-by-page, extracts key movie attributes (rank, title, year, type, runtime, rating, metascore), consolidates everything into a CSV, and then cleans and explores the data — handling missing values and inspecting outliers.

---

## 🛠️ Tech Stack

- **Python 3.12**
- `requests` — HTTP requests to IMDb
- `BeautifulSoup` (bs4) — HTML parsing
- `pandas` / `numpy` — data cleaning & manipulation
- `matplotlib` / `seaborn` — visualization
- `csv` — writing scraped data to disk

---

## 🔎 What Gets Scraped

For every movie on the list, the scraper extracts:

| Field | Description |
|---|---|
| `Rank` | Position in the IMDb list |
| `Name` | Movie title |
| `Year` | Release year |
| `Movie_type` | Type/format (e.g., Movie, TV Series) — optional field, not always present |
| `Time` | Runtime |
| `Rating` | IMDb rating |
| `Metascore` | Critic metascore (optional — not all titles have one) |

**Source:** `https://www.imdb.com/list/ls063676189/` — iterated across pages `1` to `50` via the `page` query parameter.

A custom `User-Agent` header is sent with every request to mimic a real browser and reduce the chance of being blocked.

---

## 🧹 Data Cleaning Steps

1. **Loaded** the scraped CSV into a pandas DataFrame.
2. **Fixed a parsing edge case:** for rows where `Movie_type` and `Time` were accidentally scraped as the same value (i.e., the optional "type" span was missing from the HTML and `Time` got misread), `Movie_type` was set to `NaN`.
3. **Imputed missing `Movie_type`** values using the **mode** (most frequent category).
4. **Imputed missing `Metascore`** values using the **column mean** (rounded to 1 decimal) — since not every title on IMDb has a metascore.
5. Verified null counts and dtypes at each step with `df.info()` / `.isna().sum()`.

---

## 📊 Exploratory Data Analysis

- Used `seaborn.boxplot()` across the numeric columns to visually inspect the spread and outliers in ratings/metascores/runtime.

---

## 🚀 How to Run

1. Clone this repo and install dependencies:
   ```bash
   pip install requests beautifulsoup4 pandas numpy matplotlib seaborn
   ```
2. Open `practice_imdb.ipynb` in Jupyter Notebook / JupyterLab.
3. Run all cells sequentially:
   - Cells 1–7 scrape the data and write it to `ajay.csv`
   - Cells 8 onward load, clean, and analyze the CSV
4. The scraped dataset will be saved locally as `ajay.csv` in the project directory.

> ⚠️ **Note:** Since this scrapes live HTML from IMDb, the CSS class names used (`lister-item-content`, `ipl-rating-star__rating`, etc.) are tied to IMDb's page structure at the time of writing. If IMDb changes its layout, the scraper's selectors will need to be updated.

---

## 📁 Repository Structure

```
├── practice_imdb.ipynb     # Main notebook: scraping + cleaning + EDA
├── ajay.csv                # Scraped dataset (generated on run)
└── README.md
```

---

## ⚖️ Disclaimer

This project is for **educational purposes only**. Please review IMDb's [Terms of Service](https://www.imdb.com/conditions) / `robots.txt` before scraping, use reasonable request rates, and avoid scraping at scale or for commercial use without permission.

---

## 👤 Author

**Ajay Verma**
MCA (AI/ML & Data Science) — S-VYASA Deemed University, Bengaluru

---

## 📄 License

This project is open-sourced for educational purposes.
