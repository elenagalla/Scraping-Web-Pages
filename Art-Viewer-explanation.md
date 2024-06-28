# Art Viewer Scraper

This document provides a detailed explanation of the Art Viewer scraper script (art_viewer_scraper.py).

## Overview

The Art Viewer scraper is a Python script designed to extract exhibition details from the Art Viewer website (https://artviewer.org). It uses Selenium WebDriver for web navigation, BeautifulSoup for HTML parsing, and Pandas for data manipulation and storage.

## Key Features

1. Multi-threaded scraping for improved performance

2. Handling of lazy-loaded content

3. Extraction of nested URL content

4. Batch processing of data

5. Error handling and retry mechanisms

6. Logging for debugging and monitoring

## Dependencies

- Python 3.7+

- Selenium

- BeautifulSoup4

- Pandas

- Requests

- tqdm

- webdriver_manager

## Script Structure

The script is organized into several key functions:

1. get_text_after_label(label): Extracts text following a specific label in the HTML.

2. scrape_nested_url(url, extracted_info): Scrapes additional data from nested URLs.

3. load_lazy_content(driver): Handles lazy-loaded content by scrolling the page.

4. scrape_url(url): Main function for scraping a single URL.

5. save_data(df, filename, mode='a', header=True): Saves scraped data to an Excel file.

6. process_batches(data, batch_size, filename): Processes and saves data in batches.

7. main(): Orchestrates the entire scraping process.

## Detailed Functionality

### URL Scraping

The script starts by defining a list of URLs to scrape. It then uses a ThreadPoolExecutor to scrape these URLs concurrently, improving performance.

### Content Extraction
For each exhibition on the page, the script extracts:

- Artist name(s)

- Exhibition title

- Venue

- Date

- Image URLs

- Additional information

### Nested URL Handling
The script also follows and scrapes nested URLs associated with each exhibition, extracting additional details and images.

### Data Processing
Extracted data is processed in batches and saved to an Excel file. Image URLs are stored as JSON strings to handle potential length issues.

### Error Handling
The script includes robust error handling, with logging for debugging purposes. It also implements retry mechanisms for failed requests.

## Usage
To use the script:

1. Install the required dependencies:
```bash
pip install -r art-viewer-requirements.txt
```

2. Run the script:
```bash
python art_viewer_scraper.py
```

The script will start scraping the specified URLs, and the progress will be logged to the console. Once completed, the data will be saved to an Excel file named data.xlsx in the same directory.

## Customization
You can modify the following variables in the script to adjust its behavior:

urls: List of URLs to scrape
batch_size: Number of entries to process and save at once
SCROLL_PAUSE_TIME: Time to pause between scrolls
SCROLL_INCREMENT: How much to scroll each time
MAX_ATTEMPTS: Maximum number of scroll attempts before stopping

## Output
The script generates an Excel file with the following columns:

- Artist

- Venue

- Exhibition title

- Date

- Image_URLs

- Additional Info

Each row represents a single exhibition, with all associated details and image URLs.
