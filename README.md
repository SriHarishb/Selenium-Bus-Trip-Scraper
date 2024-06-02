# Selenium-Bus-Trip-Scraper

## Bus Trip Scraper

**Bus Trip Scraper** automates searching for bus trips between two locations on a specified date using MakeMyTrip. Utilizing Selenium for web automation and BeautifulSoup for HTML parsing, it extracts bus details like names, types, and prices, saving the data into a CSV file for easy access and analysis.

### Features

- Automates bus trip search on MakeMyTrip.
- Extracts bus names, types, and prices.
- Saves the extracted data into a CSV file.

### Requirements

- Python 3.x
- Selenium
- BeautifulSoup4
- pandas
- ChromeDriver

### Code

```
import time
import csv
from bs4 import BeautifulSoup
import pandas as pd
from selenium import webdriver
from selenium.webdriver.common.by import By
from selenium.webdriver.common.keys import Keys
from selenium.webdriver.chrome.options import Options
bus_list=[]
options = Options()
options.add_argument("--disable-notifications")
F=input("From Where?\n")
T=input("To Where?\n")
a=input("When Are you planning to Take Your Train tip?\n (Eg- Fri Jun 07 2024)\n")
driver = webdriver.Chrome(options=options)
driver.get('https://www.makemytrip.com/bus-tickets/')
time.sleep(5)
try:
    close_btn = driver.find_element(By.CLASS_NAME, "commonModal__close")
    close_btn.click()
except:
    pass
time.sleep(1)
From_place_element=driver.find_element(By.ID, "fromCity")
From_place_element.click()
time.sleep(1)
From_place_search=driver.find_element(By.CLASS_NAME,"react-autosuggest__input")
From_place_search.send_keys(F)
time.sleep(1)
first_from_option=driver.find_element(By.ID,"react-autowhatever-1-section-0-item-0")
first_from_option.click()
time.sleep(1)
To_place_search=driver.find_element(By.CLASS_NAME,"react-autosuggest__input")
To_place_search.send_keys(T)
time.sleep(1)
first_to_option=driver.find_element(By.ID,"react-autowhatever-1-section-0-item-0")
first_to_option.click()
time.sleep(1)
travel_month_Element=driver.find_element(By.ID, "travelDate")
time.sleep(1)
while True:
 try:
  travel_date = driver.find_element(By.CSS_SELECTOR, f"div[aria-label='{a}']")
  travel_date.click()
  break
 except:
  nextnavbtn = driver.find_element(By.CSS_SELECTOR, "span[aria-label='Next Month']")
  nextnavbtn.click()
else:
 print("Date Not Found On Make My Trip!")
SearchBtn=driver.find_element(By.ID,"search_button")
SearchBtn.click()
time.sleep(7)
html_content = driver.page_source
soup = BeautifulSoup(html_content, 'html.parser')
bus_cards = soup.find_all("div", class_="busCardContainer")
for bus_card in bus_cards:
    bus_name = bus_card.find("p", class_="makeFlex hrtlCenter appendBottom8 latoBold blackText appendRight15").text.strip()
    bus_type = bus_card.find("p", class_="makeFlex hrtlCenter secondaryTxt nowrapStyle").text.strip()
    bus_price = bus_card.find("span", class_="sc-ckVGcZ kafEbu").text.strip()
    bus_info = {
        "Bus Name": bus_name,
        "Bus Type": bus_type,
        "Bus Price": bus_price
    }
    
    bus_list.append(bus_info)

with open('bus_data.csv', mode='w', newline='') as file:
    fieldnames = ['Bus Name', 'Bus Type', 'Bus Price']
    writer = csv.DictWriter(file, fieldnames=fieldnames)
    writer.writeheader()
    for bus in bus_list:
        writer.writerow({'Bus Name': bus['Bus Name'], 'Bus Type': bus['Bus Type'], 'Bus Price': bus['Bus Price']})
df = pd.read_csv('bus_data.csv')
print(df)
time.sleep(7)
driver.quit()
```

### Sample Output:-

![Screenshot 2024-06-02 230619](https://github.com/SriHarishb/Selenium-Bus-Trip-Scraper/assets/150308442/0c828a34-3af5-4733-963a-da3c0fef4ccd)

![Screenshot 2024-06-02 230725](https://github.com/SriHarishb/Selenium-Bus-Trip-Scraper/assets/150308442/7fa09b99-95f2-472c-b911-eda79d374237)

![Screenshot 2024-06-02 230816](https://github.com/SriHarishb/Selenium-Bus-Trip-Scraper/assets/150308442/76b05b10-0fa4-4041-849b-07a22587d892)

### Conlusion:-

The Selenium Bus Trip Scraper streamlines the process of finding bus trips by automating the search and data extraction from MakeMyTrip. This tool is particularly useful for users who need to gather and analyze bus trip information efficiently. By leveraging Selenium and BeautifulSoup, it demonstrates the power of web automation and data scraping in simplifying travel planning. We hope this project serves as a valuable resource for your travel needs and inspires further development and enhancements.
