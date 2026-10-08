# Wuzzuf Job Scraper with Selenium

Scrape job listings from Wuzzuf Egypt with Python + Selenium: search 4 roles, paginate, clean text, dedupe, and export a UTF-8 CSV.

## Target

- Site: https://wuzzuf.net/jobs/egypt
- Searches: `Data Engineer`, `Data Science`, `AI Developer`, `Machine Learning Engineer`
- Pages: up to 3 per search with polite random sleeps (5-10s)
- Result: 174 rows in `wuzzuf_jobs.csv`

## Extracted Fields

```text
Title, Company, Location, Type, Experience, URL
```

Card container `div.css-pkv5jc`, all fields located relative to the card:

- Title: `.//h2/a` + its `href` as URL
- Company: `a.css-ipsyv7`
- Location: `span.css-16x61xq`
- Type: `div.css-5jhz9n span` (fallback `Not specified`)
- Experience: nested span (fallback `Not found`)
- Pagination: `a[aria-label='Next page']`

## Cleaning

```python
df.drop_duplicates(inplace=True)
df["Company"] = df["Company"].str.strip().str.replace("-", "", regex=False).str.strip()
df["Experience"] = df["Experience"].str.replace("·", "", regex=False).str.replace(".", "", regex=False).str.strip()
df.to_csv("wuzzuf_jobs.csv", index=False, encoding="utf-8-sig")
```

## Project Structure

```text
2/
├── web_scraping.ipynb
├── wuzzuf_jobs.csv
└── README.md
```

## How to Run

```bash
pip install selenium pandas
python -m notebook
```

1. Open `web_scraping.ipynb` -> Run All (Chrome + ChromeDriver required).
2. Output `wuzzuf_jobs.csv` is rewritten in place.

## Notes

- Respects polite crawling: small page budget, sleeps between pages, no auth/CAPTCHA bypass.
- Selectors are Wuzzuf-specific and may need updates if the site markup changes.

## Tools

Python, Selenium + ChromeDriver, Pandas, Jupyter Notebook
