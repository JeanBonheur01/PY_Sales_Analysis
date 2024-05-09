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

'''
results = all_data.groupby('Month').sum()
results.head(12)
'''

<img width="1440" alt="Monthly_sales" src="https://github.com/JeanBonheur01/PY_Sales_Analysis/assets/131664311/9b2b249c-cdd6-4439-9a74-c48c388d60a5">

3. Which city recorded the highest number of sales? 
4. At what time should we display advertisements to maximize the likelihood of customers buying our product?
5. Which products are frequently purchased togheter?
6. Which product had the highest sales volume? What factors contributed to its success?
   
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
