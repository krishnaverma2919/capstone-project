# capstone-project
rom selenium import webdriver
from selenium.webdriver.common.keys import Keys
from selenium.webdriver.common.by import By
import pandas as pd
import time
from tqdm import tqdm
driver = webdriver.Chrome()
driver.get("https://www.1mg.com/categories/homeopathy-57")
time.sleep(5)
cancel_xpath = '//*[@id="update-city-modal"]/div/div[3]/div[1]'
driver.find_element(by=By.XPATH, value=cancel_xpath).click()
#driver.maximize_window()
time.sleep(5)

name_arr = []
price_arr = []
size_arr = []
link_arr = []
mrp_arr = []

for i in tqdm(range(3, 40)):
   # print('-'*60)
   # print('-'*60)
   # print(f"page no {i-1}")
   # print('-'*60)
   # print('-'*60)
    if i > 7:
        i = 7
    next_xpath = f'//*[@id="category-container"]/div[2]/div[2]/div[2]/div/div[2]/div[2]/ul/li[{i}]/a'
    driver.find_element(by=By.XPATH, value=next_xpath).click()
    time.sleep(5)
    
    
    
    for j in range(1, 40):
        
        try:
            x_path_name = f'//*[@id="category-container"]/div[2]/div[2]/div[2]/div/div[2]/div[1]/div[{j}]/div/a/div[3]/div[1]'
            element = driver.find_element(by=By.XPATH, value=x_path_name)
            name = element.text
        except:
            
        
            continue
        # print(name)
        
        x_path_size = f'//*[@id="category-container"]/div[2]/div[2]/div[2]/div/div[2]/div[1]/div[{j}]/div/a/div[3]/div[2]'
        element = driver.find_element(by=By.XPATH, value=x_path_size)
        size = element.text
        # print(size)
        
        try:
            x_path_price = f'//*[@id="category-container"]/div[2]/div[2]/div[2]/div/div[2]/div[1]/div[{j}]/div/a/div[4]/div/div[2]/span'
            element = driver.find_element(by=By.XPATH, value=x_path_price)
            price = element.text
        except Exception as e:
            price = None
        # print(price)
        
        x_path_link = f'//*[@id="category-container"]/div[2]/div[2]/div[2]/div/div[2]/div[1]/div[{j}]/div'
        ele = driver.find_element(by=By.XPATH, value=x_path_link) 
        text = ele.get_attribute('innerHTML')
        link = text[text.find('href')+6:text.find("class")-2]
        link = "https://www.1mg.com" + link
        #print(link)
        
        try:
            x_path_mrp = f'//*[@id="category-container"]/div[2]/div[2]/div[2]/div/div[2]/div[1]/div[{j}]/div/a/div[4]/div/div[1]'
            element = driver.find_element(by=By.XPATH, value=x_path_mrp)
            mrp = element.text
        except:
            mrp = None
#             x_path_mrp = f'//*[@id="category-container"]/div[2]/div[2]/div[2]/div/div[2]/div[1]/div[{j}]/div/a/div[4]/div/span[2]'
#             element = driver.find_element(by=By.XPATH, value=x_path_mrp)
#             mrp = element.text
        # print(mrp)
        # print()
            
        name_arr.append(name)
        price_arr.append(price)
        size_arr.append(size)
        link_arr.append(link)
        mrp_arr.append(mrp)
        
    time.sleep(5)

driver.close()
#task 2
from selenium import webdriver
from selenium.webdriver.common.keys import Keys
from selenium.webdriver.common.by import By
import pandas as pd
import time
from selenium.common.exceptions import NoSuchElementException
from tqdm import tqdm
df=pd.read_csv(r"first_tble.csv")
#`name` - Name of the medicine

#`brand_name`  - Brand name

#key_benefits` - Key Benefits area (Hair, eye, joint, skin)

#`key_ingredients` - Ingredient of medicine

#`rating` - user rating of the medicine

#`number_of_rating`  - Number of people rated that product
df.head()
Name	Size	Price	Link	MRP
0	Dr Willmar Schwabe India Terminalia Chebula Di...	bottle of 30 ml Dilution	NaN	https://www.1mg.com/otc/dr-willmar-schwabe-ind...	NaN
1	SBL Silicea 0/24 LM	bottle of 20 gm Globules	NaN	https://www.1mg.com/otc/sbl-silicea-0-24-lm-ot...	NaN
2	Dr. Majumder Homeo World Blumea Odorata Diluti...	box of 1 Bottle	230.0	https://www.1mg.com/otc/dr.-majumder-homeo-wor...	MRP₹27015% OFF
3	Bjain Kalium Iodatum Dilution 10M CH	bottle of 30 ml Dilution	270.0	https://www.1mg.com/otc/bjain-kalium-iodatum-d...	MRP₹30010% OFF
4	LDD Bioscience Guaiacum Mother Tincture Q	bottle of 100 ml Mother Tincture	945.0	https://www.1mg.com/otc/ldd-bioscience-guaiacu...	MRP₹105010% 
df['Link']
0       https://www.1mg.com/otc/dr-willmar-schwabe-ind...
1       https://www.1mg.com/otc/sbl-silicea-0-24-lm-ot...
2       https://www.1mg.com/otc/dr.-majumder-homeo-wor...
3       https://www.1mg.com/otc/bjain-kalium-iodatum-d...
4       https://www.1mg.com/otc/ldd-bioscience-guaiacu...
                              ...                        
1438    https://www.1mg.com/otc/dr-willmar-schwabe-ind...
1439    https://www.1mg.com/otc/haslab-hc-37-caladium-...
1440    https://www.1mg.com/otc/sbl-viola-tricolor-dil...
1441    https://www.1mg.com/otc/bakson-s-homeopathy-ph...
1442    https://www.1mg.com/otc/pioneer-pharma-kali-br...
Name: Link, Length: 1443, dtype: object
100%|████████████████████████████████████████████████████████████████████████████| 1443/1443 [6:25:08<00:00, 16.01s/it]
name_arr = []
rating_arr = []
brand_arr = []
no_rating_arr = [] 
key_benefit_arr = []

for i in tqdm(df['Link']):

    driver = webdriver.Chrome()
    driver.get(i)
    time.sleep(2)

    try:
        name = driver.find_element(By.XPATH, '//*[@id="container"]/div/div/div[2]/div[3]/div[1]/div[1]/h1').text
    except NoSuchElementException:
        name = None

    try:
        brand = driver.find_element(By.XPATH, '//*[@id="container"]/div/div/div[2]/div[3]/div[1]/div[2]/a').text
    except NoSuchElementException:
        brand = None

    try:
        rating = driver.find_element(By.XPATH, '//*[@id="container"]/div/div/div[2]/div[3]/div[1]/div[3]/div').text
    except NoSuchElementException:
        rating = None

    try:
        no_rating = driver.find_element(By.XPATH, '//*[@id="container"]/div/div/div[2]/div[3]/div[1]/div[3]/span').text
    except NoSuchElementException:
        no_rating = None

    try:
        key_benefit_elements = driver.find_elements(By.XPATH, '//*[@id="container"]/div/div/div[3]/div[1]/div[2]/div[2]/ul/li')
        key_benefit = [ele.text for ele in key_benefit_elements]
    except NoSuchElementException:
        key_benefit = None

    name_arr.append(name)
    brand_arr.append(brand)
    rating_arr.append(rating)
    no_rating_arr.append(no_rating)
    key_benefit_arr.append(key_benefit)

    driver.close()
    data = {
    'Name': name_arr,
    'brand': brand_arr,
    'rating': rating_arr,
    'no_rating': no_rating_arr,
    'key_benefit': key_benefit_arr
}
df2 = pd.DataFrame(data)
df2.head()
Name	brand	rating	no_rating	key_benefit
0	Dr Willmar Schwabe India Terminalia Chebula Di...	Dr Willmar Schwabe India Pvt Ltd	None	None	[, , , , , , , , , ]
1	SBL Silicea 0/24 LM	SBL Pvt Ltd	4.6	12 Ratings	[, , , , , , , , , , , , , , , , ]
2	Dr. Majumder Homeo World Blumea Odorata Diluti...	Dr. Majumder Homeo World	None	None	[, , , , , , , ]
3	Bjain Kalium Iodatum Dilution 10M CH	Bjain Pharmaceuticals Pvt. Ltd.	None	None	[, , , , , , ]
4	LDD Bioscience Guaiacum Mother Tincture Q	LDD Bioscience Pvt Ltd	None	None	[]
df2.to_csv("Second_tble.csv",index=False)
import pandas as pd
df1=pd.read_csv(r"first_tble.csv")
df2=pd.read_csv(r"Second_tble.csv")
f1.isnull().sum()
merge_df.head()
merge_df.isnull().sum()
merge_df.to_csv("final.csv")


