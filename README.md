# Art Exhibition Web Scrapers
This repository contains two web scraping scripts designed to extract exhibition details from different art-related websites. The scripts use Selenium WebDriver for web navigation, BeautifulSoup for HTML parsing, and Pandas for data manipulation and storage.
Scripts

## Contemporary Art Library Scraper ('contemporary_art_library_scraper.py')

Scrapes exhibition details from the Contemporary Art Library website.
Extracts artist names, exhibition titles, venues, dates, and image URLs.
Handles lazy-loaded content.
Saves data to an Excel file.


## Art Viewer Scraper (art_viewer_scraper.py)

Scrapes exhibition details from the Art Viewer website.
Extracts artist names, venues, exhibition titles, dates, image URLs, and additional information.
Handles multiple pages and nested URLs.
Processes data in batches and saves to an Excel file.



## Features

Multi-threaded scraping for improved performance
Error handling and retry mechanisms
Logging for debugging and monitoring
Batch processing of data
Handling of lazy-loaded content

## Requirements

Python 3.7+
Chrome browser
ChromeDriver (automatically managed by webdriver_manager)

## Installation

Clone this repository:
```bash
Copygit clone https://github.com/yourusername/art-exhibition-scrapers.git
cd art-exhibition-scrapers
```

### Install the required packages:
```bash
pip install -r requirements.txt
```
### Install the required packages:

```bash
pip install -r requirements.txt
```

## Usage
To run the scraper:

```bash
python art_exhibition_scraper.py
```
The script will start scraping the specified websites, and the progress will be logged to the console. Once completed, the data will be saved to Excel files in the same directory.

## Configuration
You can modify the following variables in the script to adjust the scraper's behavior:

### Contemporary Art Library Scraper:


SCROLL_PAUSE_TIME: Time to pause between scrolls

SCROLL_INCREMENT: How much to scroll each time

MAX_ATTEMPTS: Maximum number of scroll attempts before stopping

### Art Viewer Scraper:

urls: List of URLs to scrape


batch_size: Number of entries to process and save at once

## Output
The scraper generates Excel files with columns including Artist, Venue, Exhibition Title, Date, and Image URLs. The Art Viewer scraper also includes an Additional Info column.

## Logging
The script uses Python's built-in logging module to provide information about the scraping process. You can adjust the logging level in the script if needed.

## Error Handling
The scraper includes error handling to manage common issues such as network errors or changes in the websites' structures. Errors are logged for debugging purposes.

## Limitations
The scraper is designed for the current structure of the respective websites. If the websites undergo significant changes, the scraper may need to be updated.
Respect the websites' robots.txt files and terms of service when using this scraper.
