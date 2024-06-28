# Art Exhibition Web Scrapers
This repository contains two web scraping scripts designed to extract exhibition details from different art-related websites. The scripts use Selenium WebDriver for web navigation, BeautifulSoup for HTML parsing, and Pandas for data manipulation and storage.

## Contemporary Art Library Scraper ('contemporary_art_library_scraper.py')

- Scrapes exhibition details from the Contemporary Art Library website.

- Extracts artist names, exhibition titles, venues, dates, and image URLs.

- Handles lazy-loaded content.

- Saves data to an Excel file.


## Art Viewer Scraper (art_viewer_scraper.py)

- Scrapes exhibition details from the Art Viewer website.

- Extracts artist names, venues, exhibition titles, dates, image URLs, and additional information.

- Handles multiple pages and nested URLs.

- Processes data in batches and saves to an Excel file.


## Features

- Multi-threaded scraping for improved performance

- Error handling and retry mechanisms

- Logging for debugging and monitoring

- Batch processing of data

- Handling of lazy-loaded content

## Requirements

- Python 3.7+

- Chrome browser

- ChromeDriver (automatically managed by webdriver_manager)

## Installation

Clone this repository:
```bash
git clone https://github.com/yourusername/art-exhibition-scrapers.git
cd art-exhibition-scrapers
```

### Install the required packages:
```bash
pip install -r contemporary-art-library-requirements.txt
```
### Install the required packages:

```bash
pip install -r art-viewer-requirements.txt
```

## Usage
To run the scraper:

```bash
python art_exhibition_scraper.py
```
The script will start scraping the specified websites, and the progress will be logged to the console. Once completed, the data will be saved to Excel files in the same directory.

## Configuration
You can modify the following variables in the script to adjust the scraper's behavior:

- SCROLL_PAUSE_TIME: Time to pause between scrolls

- SCROLL_INCREMENT: How much to scroll each time

- MAX_ATTEMPTS: Maximum number of scroll attempts before stopping

- urls: List of URLs to scrape

- batch_size: Number of entries to process and save at once

## Output

### Art Viewer 
1. Artist

      Contains the name of the artist(s) for each exhibition
      Multiple artists are separated by commas
      If no artist is found, it will contain "N/A"


2. Venue

      The name of the venue hosting the exhibition
      If no venue is found, it will contain "N/A"


3. Exhibition title

      The title of the exhibition
      If no title is found, it will contain "N/A"


4. Date

      The date or date range of the exhibition
      If no date is found, it will contain "N/A"


5. Image_URLs

      A JSON string containing URLs of images related to the exhibition
      Includes both images from the main page and any nested pages


6. Additional Info

      Contains additional text information about the exhibition
      This could include descriptions, artist statements, or other relevant details
      The information is collected from both the main page and any nested pages

### Contemporary Art

1. Artist

      Contains the name of the artist(s) for each exhibition
      If multiple artists, they are likely separated by commas
      If no artist is found, it will contain "N/A"


2. Exhibition Title

      The title of the exhibition
      If no title is found, it will contain "N/A"


3. Venue

      The name of the venue hosting the exhibition
      If no venue is found, it will contain "N/A"


4. Date

      The date or date range of the exhibition
      If no date is found, it will contain "N/A"


4. Images_1, Images_2, etc.

      These columns contain URLs of images related to the exhibition
      The number of these columns depends on how many images are found and how they are split (due to Excel cell size limitations)
      Each cell in these columns contains a JSON string of image URLs

## Logging
The script uses Python's built-in logging module to provide information about the scraping process. You can adjust the logging level in the script if needed.

## Error Handling
The scraper includes error handling to manage common issues such as network errors or changes in the websites' structures. Errors are logged for debugging purposes.

## Limitations
The scraper is designed for the current structure of the respective websites. If the websites undergo significant changes, the scraper may need to be updated.
Respect the websites' robots.txt files and terms of service when using this scraper.
