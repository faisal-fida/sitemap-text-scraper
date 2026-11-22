# sitemap-text-scraper

Reads a website's sitemap, downloads every listed page concurrently and saves the cleaned page text to one text file, ready to be used as LLM input.

Originally named Scrape-Tokenize-whole-website-for-LLMs. Despite that name, the code does not tokenize; it only extracts and cleans text.

## Features

- Parses `<loc>` URLs from a sitemap XML (a single sitemap, not nested sitemap indexes).
- Fetches pages with 5 worker threads and a retry decorator (3 attempts, 1 second wait).
- Extracts the page body text with BeautifulSoup and cleans it: removes non-ASCII characters, URLs and short lines (fewer than 5 words).
- Writes everything to `output/<name>.txt`, each page prefixed with its URL.

## Tech stack

Python 3.12 or newer, requests, BeautifulSoup4, lxml, Poetry.

## Project structure

```
main.py                    entry point (sitemap URL is set here)
web_text_scraper/
  parser.py                SitemapParser
  scraper.py               Scraper (fetch, clean, threads)
  utils.py                 RetrySession and retry decorator
  allfly.txt               sample output from an earlier run
pyproject.toml, poetry.lock
```

## Setup and run

```bash
pip install poetry
poetry install
poetry run python main.py
```

Edit `sitemap_url` at the bottom of `main.py` first (it currently points at `https://allfly.io/sitemap.xml`). No environment variables are needed.

## Usage

Set `sitemap_url = "https://example.com/sitemap.xml"`, run `main.py`, then read `output/<name>.txt`.

## Status and limitations

- Python 3.12 is required because of nested quotes inside f-strings.
- No tokenization, chunking or robots.txt handling. Only scrape sites you are allowed to.
- Sitemap index files (sitemaps that list other sitemaps) are not followed.
- Results arrive in completion order, not sitemap order.
- `web_text_scraper/allfly.txt` is scraped third-party content from a previous run and could be removed.
- Compiled only in this cleanup; not run against a live site.
