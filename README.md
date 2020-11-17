# google-scraper-python-scrapy

Python Scrapy spider that searches Google for a particular keyword and extracts all data from the SERP results. The spider will iterate through all pages returned by the keyword query. The following are the fields the spider scrapes for the Google SERP page:

* Title
* Link
* Related links
* Description
* Snippet
* Images
* Thumbnails
* Sources

This Google SERP spider uses Scraper API as the proxy solution. Scraper API has a free plan that allows you to make up to 1,000 requests per month which makes it ideal for the development phase, but can be easily scaled up to millions of pages per month if needs be.

Scraper API also offers autoparsing functionality free of charge for Google, so by adding `&autoparse=true` to the request the API returns all the SERP data in JSON format.

This spider can be easily customised for your particular search requirements. In this case, it allows you to refine your search queries by specifying a  keyword, the geographic region, the language, the number of results, results from a particular domain, or even to only return safe results.

Full tutorial can be found here: [Scraping Millions of Google SERPs The Easy Way (Python Scrapy Spider)](https://dev.to/iankerins/scraping-millions-of-google-serps-the-easy-way-python-scrapy-spider-4lol-temp-slug-8957520?preview=f73a488815c3cc75236c79ea4bfadbe21121c6edd4d54095bac81832859c6be9464bc9d34bcc32f2f82792d4e97af36ef1836db8b3d20a1009ddf5d1)

## Using the Google Spider

Make sure Scrapy is installed:

```
pip install scrapy
```

Set the keywords you want to search in Google.

```
queries = ['scrapy', 'beautifulsoup']
```

Signup to [Scraper API](https://www.scraperapi.com/signup) and get your free API key that allows you to scrape 1,000 pages per month for free. Enter your API key into the API variable:

```
API_KEY = 'YOUR_API_KEY' 

def get_url(url):
    payload = {'api_key': API_KEY, 'url': url, 'autoparse': 'true', 'country_code': 'us'}
    proxy_url = 'http://api.scraperapi.com/?' + urlencode(payload)
    return proxy_url
```

By default, the spider is set to have a max concurrency of 5 concurrent requests as this the max concurrency allowed on Scraper APIs free plan. If you have a plan with higher concurrency then make sure to increase the max concurrency in `custom_settings` dictionary.

```
custom_settings = {'ROBOTSTXT_OBEY': False, 'LOG_LEVEL': 'INFO',
                       'CONCURRENT_REQUESTS_PER_DOMAIN': 5, 
                       'RETRY_TIMES': 5}
```

We should also set `RETRY_TIMES` to tell Scrapy to retry any failed requests (to 5 for example) and make sure that `DOWNLOAD_DELAY`  and `RANDOMIZE_DOWNLOAD_DELAY` aren’t enabled as these will lower your concurrency and are not needed with Scraper API.

```
## settings.py

# DOWNLOAD_DELAY
# RANDOMIZE_DOWNLOAD_DELAY
```

To run the spider, use:

```
scrapy crawl google -o test.csv
```



