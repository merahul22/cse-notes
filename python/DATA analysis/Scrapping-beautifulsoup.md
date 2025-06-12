## Web Scraping with BeautifulSoup and Converting to DataFrame

This guide covers:

1. Theory of web scraping
    
2. Setting up environment and installing libraries
    
3. Fetching page content with `requests`
    
4. Parsing HTML with BeautifulSoup
    
5. Navigating and extracting data (`find`, `find_all`, CSS selectors)
    
6. Error handling and polite scraping
    
7. Building a structured dataset
    
8. Converting scraped data into a Pandas DataFrame
    
9. Complete script example: scraping quotes from [http://quotes.toscrape.com](http://quotes.toscrape.com/)


### 1. Theory of Web Scraping

- **Definition**: Automated retrieval of data from websites by parsing HTML.
    
- **Use Cases**: Collecting product prices, news headlines, public records, academic research.
    
- **Ethics & Legality**: Always check `robots.txt`, respect rate limits, include user-agent.
    

---

### 2. Environment Setup

```bash
pip install requests beautifulsoup4 pandas
```

---

### 3. Fetching Page Content

```python
import requests

url = 'https://quotes.toscrape.com/'
headers = {'User-Agent': 'Mozilla/5.0'}
response = requests.get(url, headers=headers)
if response.status_code == 200:
    html = response.text
else:
    raise Exception(f"Failed to fetch page: {response.status_code}")
```

---

### 4. Parsing HTML with BeautifulSoup

```python
from bs4 import BeautifulSoup

soup = BeautifulSoup(html, 'html.parser')
```

- Parser options: `'html.parser'`, `'lxml'`, `'html5lib'` (install if needed).
    

---

### 5. Navigating and Extracting Data

- **Target elements**: e.g., each quote is in `<div class="quote">`.
    

```python
quotes_divs = soup.find_all('div', class_='quote')
items = []
for div in quotes_divs:
    text = div.find('span', class_='text').get_text(strip=True)
    author = div.find('small', class_='author').get_text(strip=True)
    tags = [t.get_text() for t in div.find_all('a', class_='tag')]
    items.append({'quote': text, 'author': author, 'tags': tags})
```

---

### 6. Error Handling & Polite Scraping

- **Respect ****`robots.txt`**
    
- **Rate limiting**: use `time.sleep()` between requests.
    
- **Try/Except** around parsing to skip malformed elements.
    

---

### 7. Building a Structured Dataset

- Collect list of dictionaries (`items` above).
    
- Each dictionary maps column names to values.
    

---

### 8. Converting to Pandas DataFrame

```python
import pandas as pd

df = pd.DataFrame(items)
print(df.head())
```

- Further cleaning: `df['tags'] = df['tags'].apply(lambda x: ','.join(x))` if needed.
    

---

### 9. Complete Script Example

```python
import requests
from bs4 import BeautifulSoup
import pandas as pd
import time

# Settings
BASE_URL = 'http://quotes.toscrape.com'
HEADERS = {'User-Agent': 'Mozilla/5.0'}
PAGE = ''
all_quotes = []

# Loop through first 3 pages
for page_num in range(1, 4):
    url = f"{BASE_URL}/page/{page_num}/"
    resp = requests.get(url, headers=HEADERS)
    if resp.status_code != 200:
        print(f"Skipping page {page_num}, status code {resp.status_code}")
        continue
    soup = BeautifulSoup(resp.text, 'html.parser')
    quote_divs = soup.find_all('div', class_='quote')
    for div in quote_divs:
        try:
            text = div.find('span', class_='text').get_text(strip=True)
            author = div.find('small', class_='author').get_text(strip=True)
            tags = [t.get_text() for t in div.find_all('a', class_='tag')]
            all_quotes.append({'quote': text, 'author': author, 'tags': tags})
        except AttributeError:
            continue
    time.sleep(1)  # polite pause

# Convert to DataFrame
df = pd.DataFrame(all_quotes)
print(df)
# Optional: save to CSV
df.to_csv('quotes.csv', index=False)
```
