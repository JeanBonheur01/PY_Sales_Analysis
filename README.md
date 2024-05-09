# PY_Sales_Analysis

## Table of contents

- [Problem Statement](#problem-satement)
- [Data Sources](#data-sources)
- [Tools](#tools)
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
- Finding 1: December was the best selling month followed by october, april, november and may. January was the lowest month for sales.
- Finding 2: San Francisco is the city with the highest sales, followed by Los Angeles and New York City. Portland has the lowest performance
- Finding 3: Customers order more at midday, between 11 and 13 o'clock, as well as in the evening, between 18 and 20 o'clock. The company sells very little at night, and sales begin to increase again when the workday starts at approximately 6 o'clock.

Answer to question 3: Before 11 o'clock and between 18 o'clock should be the best times to display the advertisement to attract customers to buy more."
- Finding 4: The most two corelated products sell togethet is Iphone & Lightning Charging Cable, etc. This information can help the company to structure its products offering and promotions.
- Finding 5: AAA Batteries (4-pack) was the most solt item, followed by AA Bateries and USB-C Charging, etc
We can note that items with lower prices are also highly sold, but this assumption doesn't apply to all products. For example, "Wired Headphones" are cheaper than the Lightning Charging Cable, yet the latter has higher sales. Macbooks are more expansive than LG Dryer, but Macbooks sell better than the latter. Thus, cheaper products are generally the best-selling ones, but this is not always the case, the are other additional factors.

### Recommendations
### Conclusion 

💻🖱️🤖🖱️🤖💻
