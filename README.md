# Scraping the web with Python, BeautifulSoup library, web page used: books.toscrape.com
# linking books'titles to their prices and availability status


import requests
from bs4 import BeautifulSoup

url = "http://books.toscrape.com/index.html"
response = requests.get(url)
html = response.content
scraped = BeautifulSoup(html, 'html.parser')

title = scraped.title.text

first_title = scraped.article.h3.a["title"]

#get all titles on the page
titles = []
scraped_titles = scraped.find_all("a", title=True)
for article in scraped_titles:
    titles.append(article["title"])

#get all the prices on the page
prices = scraped.select(".price_color")
for price in prices:
    price = float(price.text.lstrip("£"))
    
 
#make a title-price dict   
titles_prices = []
articles = scraped.select(".product_pod")

for article in articles:
    titles = article.h3.a["title"]
    prices = article.find("p", class_="price_color")
    prices_float = float(prices.text.lstrip("£"))
    titles_prices.append({titles: prices_float})

titles_availability = []

articles = scraped.select(".product_pod")

#make a title-availability dict
for article in articles:
    titles = article.h3.a["title"]
    availability = article.find("p", class_="instock availability").text.strip()
    titles_availability.append({titles: availability})

#make a title-price-availability dict
my_dict = {}
for title in titles:
    tuple = (titles,prices_float)
    #my_dict.update({tuple : availability})  same as previous line
    my_dict[tuple]=availability
print(my_dict)
