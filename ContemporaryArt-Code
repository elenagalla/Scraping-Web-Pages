import logging
import pandas as pd
import time
from selenium import webdriver
from selenium.webdriver.chrome.service import Service
from selenium.webdriver.support.ui import WebDriverWait
from selenium.webdriver.support import expected_conditions as EC
from selenium.webdriver.common.by import By
from webdriver_manager.chrome import ChromeDriverManager
from bs4 import BeautifulSoup
import re

# Set up logging
logging.basicConfig(level=logging.INFO)
logger = logging.getLogger(__name__)

def extract_date(text):
    date_pattern = re.compile(r"([A-Za-z]+\s\d{1,2},?\s\d{4})\s?–?\s?([A-Za-z]+\s\d{1,2},?\s\d{4})")
    match = date_pattern.search(text)
    if match:
        return match.group()
    return text

def extract_image_urls(soup):
    try:
        images = []
        # Extract standard image URLs
        for img_tag in soup.find_all('img'):
            image_url = img_tag.get('src') or img_tag.get('data-src') or img_tag.get('data-lazy')
            if image_url and "logo1.png" not in image_url and not image_url.startswith("data:image/svg+xml"):
                images.append(image_url)
        
        # Extract image URLs from source tags (picture elements)
        for source_tag in soup.find_all('source'):
            image_url = source_tag.get('srcset')
            if image_url and "logo1.png" not in image_url:
                images.append(image_url)

        logger.info(f"Extracted {len(images)} image URLs.")
        return images
    except Exception as e:
        logger.error(f"Error extracting image URLs: {e}")
        return []

def extract_details(driver):
    def get_element_by_xpath(xpath):
        try:
            return driver.find_element(By.XPATH, xpath)
        except:
            return None

    try:
        details = {}
        page_source = driver.page_source
        soup = BeautifulSoup(page_source, 'html.parser')

        artist_element = get_element_by_xpath("//span[contains(@class, 'Caption_artist')]")
        title_element = get_element_by_xpath("//span[contains(@class, 'Caption_title')]")
        venue_element = get_element_by_xpath("//a[contains(@href, '/venue/')]")
        date_element = get_element_by_xpath("//div[contains(@class, 'Caption_caption')]/div[contains(text(), ', ')]")

        details['Artist'] = artist_element.text if artist_element else "N/A"
        details['Exhibition Title'] = title_element.text if title_element else "N/A"
        details['Venue'] = venue_element.text if venue_element else "N/A"
        details['Date'] = extract_date(date_element.text) if date_element else "N/A"
        details['Images'] = extract_image_urls(soup)

        return details
    except Exception as e:
        logger.error(f"Error extracting details: {e}")
        return None

def split_image_urls(image_urls, max_length=2000):
    url_str = ";".join(image_urls)
    logger.info(f"Total length of URLs: {len(url_str)}")
    chunks = []
    while len(url_str) > max_length:
        split_index = url_str.rfind(';', 0, max_length)
        if split_index == -1:
            split_index = max_length
        chunks.append(url_str[:split_index])
        url_str = url_str[split_index + 1:]
    chunks.append(url_str)
    logger.info(f"Split into {len(chunks)} chunks.")
    return chunks

def load_lazy_content(driver):
    SCROLL_PAUSE_TIME = 0.3  # Reduced pause time to make scrolling faster
    SCROLL_INCREMENT = 3000
    MAX_ATTEMPTS = 10
    no_change_attempts = 0

    last_height = driver.execute_script("return document.body.scrollHeight")

    while no_change_attempts < MAX_ATTEMPTS:
        driver.execute_script(f"window.scrollBy(0, {SCROLL_INCREMENT});")
        time.sleep(SCROLL_PAUSE_TIME)
        new_height = driver.execute_script("return document.body.scrollHeight")
        logging.info(f"Scrolled to: {new_height}")
        if new_height == last_height:
            no_change_attempts += 1
        else:
            no_change_attempts = 0
        last_height = new_height

    time.sleep(1)  # Ensure all content is fully loaded

def scrape_contemporary_art_library():
    options = webdriver.ChromeOptions()
    options.add_argument("--headless")
    options.add_argument("--no-sandbox")
    options.add_argument("--disable-dev-shm-usage")
    options.add_argument("--disable-gpu")
    options.add_argument("--disable-features=NetworkService")
    options.add_argument("--window-size=1920,1080")

    driver = webdriver.Chrome(service=Service(ChromeDriverManager().install()), options=options)

    links = []
    data = []

    try:
        url = "https://www.contemporaryartlibrary.org/search/exhibition"
        logger.info(f"Scraping URL: {url}")
        driver.get(url)

        load_lazy_content(driver)  # Load all lazy-loaded content on the main page

        try:
            WebDriverWait(driver, 60).until(
                EC.presence_of_all_elements_located((By.CSS_SELECTOR, "a[href*='/project/']"))
            )
            logger.info("Initial search items loaded successfully.")
        except Exception as e:
            logger.error(f"Timeout waiting for search items to load: {e}")
            driver.quit()
            return

        project_list_items = driver.find_elements(By.CSS_SELECTOR, "a[href*='/project/']")
        logger.info(f"Found {len(project_list_items)} project list items.")

        for item in project_list_items:
            try:
                link = item.get_attribute("href")
                links.append(link)
                logger.info(f"Found link: {link}")
            except Exception as e:
                logger.error(f"Error extracting link: {e}")

        for link in links:
            try:
                logger.info(f"Visiting link: {link}")
                driver.get(link)

                WebDriverWait(driver, 30).until(
                    EC.presence_of_element_located((By.XPATH, "//div[contains(@class, 'Caption_caption')]"))
                )

                load_lazy_content(driver)  # Load all lazy-loaded content on the project page

                for attempt in range(3):
                    try:
                        details = extract_details(driver)
                        if details:
                            image_urls = details.pop('Images')
                            if image_urls:
                                chunks = split_image_urls(image_urls)
                                for idx, chunk in enumerate(chunks):
                                    details[f'Images_{idx + 1}'] = chunk
                            data.append(details)
                            logger.info(f"Extracted details: {details}")
                        break
                    except Exception as e:
                        logger.error(f"Attempt {attempt + 1} failed: {e}")
                        if attempt == 2:
                            raise e

            except Exception as e:
                logger.error(f"Error processing link {link}: {e}")

    finally:
        driver.quit()

    df = pd.DataFrame(data)
    try:
        df.to_excel('contemporary_art_library_details.xlsx', index=False)
        logger.info("Data has been saved to contemporary_art_library_details.xlsx")
    except Exception as e:
        logger.error(f"Error saving data to Excel: {e}")

if __name__ == "__main__":
    try:
        scrape_contemporary_art_library()
    except Exception as e:
        logger.error(f"Error in main function: {e}")

        scrape_contemporary_art_library()
    except Exception as e:
        logger.error(f"Error in main function: {e}")
