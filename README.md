# PY_Sales_Analysis

## Table of contents

- [Problem Statement](#problem-satement)
- [Data Sources](#data-sources)
- [Tools](#tools)
- [Data Cleaning](#data-cleaning)
- [Questions and PY codes](#questions-and-py-codes)
- [Results/Findings](resultsfindings)
- [Recommendations](#recommendations)
- [Conclusion](#conclusion)


### Problem Statement
A company, armed with monthly data files spanning over a year, seeks to analyze its historical data to gain insights into its sales patterns. This analysis is important for making informed business decisions aimed at enhancing the effectiveness and efficiency of the sales processes. 
The company is in need of a data analyst to consolidate these monthly datasets into a unified structured dataset, clean up the data and subsequently conduct analyzes to adress business questions. 

### Data Sources 
The primary data used for this analysis consists of the CSV monthly datasets, which were concatenated and unified into a single dataset named "all_data." This unified dataset was then utilized to conduct analyses. The data files can be found in the attached folders within this repository. 

### Tools

- Visual Studio Code: The tool is a code editor optimized for building and debugging modern web and cloud applications. It integrates different code editors, such as Python and Jupyter Notebooks, which were activated/enabled to conduct this analysis within Visual Studio Code.  [Find here](https://code.visualstudio.com/)

- The analysis was conducted using the functionalities of Python's pandas and Matplotlib.

### Data Cleaning

The project in Visual Studio Code began by creating and cleaning the all_data. These tasks were iterative, meaning data cleaning was performed based on analysis questions and whenever necessary to answer these questions. Tasks during this section included: 

- Dropping NaN values from the DataFrame
- Removing rows based on a condition
- Changing the types of columns (to_numeric, to_datetime, astype)
  
### Questions and PY codes
1. Which month yielded the highest sales? How much revenue was generated during that month?

```py
results = all_data.groupby('Month').sum()
results.head(12)
```

<img width="1440" alt="Monthly_sales" src="https://github.com/JeanBonheur01/PY_Sales_Analysis/assets/131664311/9b2b249c-cdd6-4439-9a74-c48c388d60a5">

2. Which city recorded the highest number of sales?

```py
def get_city(address): 
   return address.split(',')[1]

def get_state(address): 
   return address.split(',')[2].split(' ')[1]

all_data['City_state'] = all_data['Purchase Address'].apply(lambda x: get_city(x) + ' (' + get_state(x) + ')')

results= all_data.groupby('City_state').sum()
results
```

<img width="1440" alt="Sales_by_city" src="https://github.com/JeanBonheur01/PY_Sales_Analysis/assets/131664311/eabf35b3-0fec-4a9b-8e66-45f492a2d255">

3. At what time should we display advertisements to maximize the likelihood of customers buying our product?

```py
all_data['Hour'] = all_data['Order Date'].dt.hour
all_data['Minute'] = all_data['Order Date'].dt.minute
all_data.head()
```
<img width="1440" alt="Hourly_sales" src="https://github.com/JeanBonheur01/PY_Sales_Analysis/assets/131664311/378fe6f4-6677-4cd2-b59a-e827c46db753">

4. Which products are frequently purchased togheter?

```py
df = all_data[all_data['Order ID'].duplicated(keep=False)].copy()
df.loc[:, 'Grouped_product'] = df.groupby('Order ID')['Product'].transform(lambda x: ','.join(x))
df = df[['Order ID', 'Grouped_product']].drop_duplicates()
print(df.head().to_string(index=False, max_colwidth=1000))

from itertools import combinations
from collections import Counter

count = Counter()

for row in df['Grouped_product']: 
    row_list = row.split(',')
    count.update(Counter(combinations(row_list, 2)))
    
for key, value in count.most_common(10):
    print (key, value)
```
<img width="1440" alt="Corelated_products" src="https://github.com/JeanBonheur01/PY_Sales_Analysis/assets/131664311/c386f6df-885a-4631-9056-73df3b760001">

5. Which product had the highest sales volume? What factors contributed to its success?

- Step 1: Summarize the quantity ordered based on grouping by the product

```py
product_group = all_data.groupby('Product')['Quantity Ordered'].sum().sort_values(ascending=False)

product_group = all_data.groupby('Product')['Quantity Ordered'].sum().reset_index()
product_group_sorted = product_group.sort_values(by='Quantity Ordered', ascending=False)

plt.figure(figsize=(10, 6))
plt.bar(product_group_sorted['Product'], product_group_sorted['Quantity Ordered'])

plt.xlabel('Product')
plt.ylabel('Quantity Ordered')
plt.title('Quantity Ordered per Product')
plt.xticks(rotation=90)

plt.tight_layout()
plt.show()
```
<img width="1440" alt="Qt_ordered_products" src="https://github.com/JeanBonheur01/PY_Sales_Analysis/assets/131664311/c305e4ec-7e23-4481-8896-4ed9e6b27b89">

- Step 2: Finding why these products sold the most.

```py
all_data['Price Each'] = pd.to_numeric(all_data['Price Each'], errors='coerce')
prices = all_data.groupby('Product')['Price Each'].mean()

prices = all_data.groupby('Product')['Price Each'].mean()

fig, ax1 = plt.subplots()

plt.xticks(rotation='vertical')

ax1.bar(prices.index, all_data.groupby('Product')['Quantity Ordered'].sum(), color='g')
ax1.set_xlabel('Product')
ax1.set_ylabel('Quantity Ordered', color='g')

ax2 = ax1.twinx()
ax2.plot(prices.index, prices, 'b-')
ax2.set_ylabel('Price ($)', color='b')

plt.show()
```

<img width="1440" alt="PriceVSqtOrd_Prod" src="https://github.com/JeanBonheur01/PY_Sales_Analysis/assets/131664311/15b0b943-78a5-4d5c-91fe-996201bb4cc5">

### Results/Findings
1. December was the best-selling month, followed by October, April, November and May. January had the lowest sales.
   
2. San Fransisco has the highest sales, followed by Los Angeles and New York City. Portland has the lowest performance.
   
3. Customers order more at midday, between 11 a.m. and 1 p.m., as well as in the evening, between 6 p.m. and 8 p.m. The company sells very little at night, and sales begin to increase again around 6 a.m. when the workday starts.
   
4. The two most correlated products sold togheter are iPhone & Lightning Charging Cable, etc.
5. AAA Batteries (4-pack) was the most sold item, followed by AA Batteries and USB-C Charging, etc. Items with lower prices are mostly highly sold, but this assumption doesn´t apply to all products. For example MacBooks are more expensive than LG Dryers, but MacBooks sell better than the latter. Thus, cheaper products are generally the best-selling ones, but this is not always the case; the are additional factors.  

### Recommendations

#### 1. Price Optimization

The company experienced low revenue at the beginning of the year, a period following the holidays when demand typically decreases. To counteract this decline in sales during this slow period, the company should implement strategies such as offering customized pricing and creating attractive promotions. These initiatives can helst stimilate consumer interest and drive sales during traditionally quieter times.
  
While cheaper products tend to sell better overall, the company should also consider other factors such as brand reputation, product quality, and customer preferences when setting prices. Conducting price sensitive analysis can help optimize pricing strategies for different product categories. 

#### 2. Targeted Advertising

Since midday and early evening are peak times for customer orders, the company should focus its advertising efforts during these periods to maximize customer engagement and increase sales. 

#### 3. Product Bundling

Given the insight into correlated products, the company can create bundled offers or promotions to encourage customers to purchase complementary items together. For exmaple, offering discounts on iPhone & Lightning Charging Cable when purchased with other related accessories. 

#### 4. Optimized Product Offerings

With the knowledge of the best-selling items and their correlations, the company can optimize its products offerings by prioritizing the promotion of high-demand products. This can include highlighting best-selling items on the website, in marketing campaigns, and in-store displays. 

#### 5. Inventory Management

Understanding sales trends by month and city can help the company better manage its inventory. By adjusting inventory levels to match expected demand fluctuations, the company can minimize stockouts and excess inventory, leading to improve efficiency and cost savings. 

#### 6. Customer Engagement

The company can use insights from the data to enhance customer engagement strategies. This can include personalized recommendations based on past purchases, targeted email compaigns, or loyalty programs to incentivize repeat purchases. 

#### 7. Market Expansion

Since San Francisco had been identified as the city with the highest sales, the company could consider expanding its presence in this market or replicating successful strategies in other cities with similar characteristics. 

### Conclusion 

Through comprehensive analysis of sales data, it is evident that the company exhibits seasonal sales patterns, with peaks observed during certain months and in specific cities. By leveraging insights such as peak selling times, correlated product purchases, and best-selling items, the company can strategically optimize its marketing, pricing, and inventory management strategies to capitalize on opportunities for revenue growth. 
Additionally focusing on targeted advertising, product bundling, and personalized promotions can further enhance customer engagement and drive sales during slower periods. Overall, these findings underscore the importance of data-driven decision-making in maximizing sales performance and fostering business success. 

💻🖱️🤖🖱️🤖💻
