# Web-Scraping-in-Python-using-Beautiful-Soup
I  wrote a python program to scrape the IMDB website using Beautiful Soup and Requests libraries and then load the desired data into an excel file



!pip3 install requests
!pip3 install bs4
!pip3 install openpyxl

from bs4 import BeautifulSoup
import requests, openpyxl

##creating an excel workbook
excel = openpyxl.Workbook()
sheet = excel.active
sheet.title = 'Top Rated Movies'
print(excel.sheetnames)

sheet.append(['Movie Rank','Movie Name','Year of Release','IMDB Rating'])

##extracting movie ratings from IMDB website to excel sheet
try:
  source = requests.get('https://www.imdb.com/chart/top/')
  source.raise_for_status()

  soup = BeautifulSoup(source.text, 'html.parser')
  movies = soup.find('tbody',class_='lister-list').find_all('tr')
  for movie in movies:
    name = movie.find('td', class_='titleColumn').a.text
    rank = movie.find('td', class_='titleColumn').get_text(strip=True).split('.')[0]
    year = movie.find('td', class_='titleColumn').span.text.strip('()')
    rating = movie.find('td', class_='ratingColumn imdbRating').strong.text
    print(name,rank,year,rating)
    sheet.append([rank,name,year,rating])
  

except Exception as e:
  print(e)

excel.save('IMDB Movie Ratings.xlsx')


