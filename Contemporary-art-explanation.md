# Contemporary Art Library Scraper

This document provides a detailed explanation of the Contemporary Art Library scraper script (contemporary_art_library_scraper.py).

## Overview

The Contemporary Art Library scraper is a Python script designed to extract exhibition details from the Contemporary Art Library website (https://www.contemporaryartlibrary.org). It uses Selenium WebDriver for web navigation and interaction, BeautifulSoup for HTML parsing, and Pandas for data manipulation and storage.

## Key Features

1. Handling of lazy-loaded content

2. Extraction of detailed exhibition information

3. Image URL handling and splitting

4. Robust error handling and retry mechanisms

5. Detailed logging for debugging and monitoring

## Dependencies

- Python 3.7+

- Selenium

- BeautifulSoup4

- Pandas

- webdriver_manager

- logging

## Script Structure
The script is organized into several key functions:

1. extract_date(text): Extracts date information from text using regex.

2. extract_image_urls(soup): Extracts image URLs from the page's HTML.

3. extract_details(driver): Extracts detailed information about an exhibition.

4. split_image_urls(image_urls, max_length=2000): Splits long image URL strings into chunks.

5. load_lazy_content(driver): Handles lazy-loaded content by scrolling the page.

6. scrape_contemporary_art_library(): Main function that orchestrates the entire scraping process.

## Detailed Functionality

### Initialization
The script sets up a headless Chrome browser using Selenium WebDriver, with various options to optimize performance and avoid detection.

### URL Scraping
The script starts by navigating to the main search page for exhibitions. It then extracts links to individual exhibition pages.

### Content Extraction
For each exhibition page, the script extracts:

- Artist name(s)

- Exhibition title

- Venue

- Date

- Image URLs

### Lazy Loading Handling

The script implements a custom function to handle lazy-loaded content, ensuring all relevant information is loaded before extraction.

### Image URL Handling

Image URLs are extracted and then split into chunks to avoid exceeding Excel cell size limits.

### Data Processing

Extracted data is collected in a list of dictionaries, which is then converted to a Pandas DataFrame and saved to an Excel file.

### Error Handling

The script includes robust error handling, with multiple retry attempts for various operations. Detailed logging is implemented for debugging purposes.

## Usage
To use the script:

1. Install the required dependencies:
```bash
pip install -r contemporary-art-library-requirements.txt
```

3. Run the script:
```bash
python contemporary_art_library_scraper.py
```

The script will start scraping the Contemporary Art Library website, and the progress will be logged to the console. Once completed, the data will be saved to an Excel file named contemporary_art_library_details.xlsx in the same directory.

## Customization

You can modify the following variables in the script to adjust its behavior:

- SCROLL_PAUSE_TIME: Time to pause between scrolls

- SCROLL_INCREMENT: How much to scroll each time

- MAX_ATTEMPTS: Maximum number of scroll attempts before stopping

## Output
The script generates an Excel file with the following columns:

- Artist

- Exhibition Title

- Venue

- Date

- Images_1, Images_2, etc. (multiple columns for split image URLs)

Each row represents a single exhibition, with all associated details and image URLs.
