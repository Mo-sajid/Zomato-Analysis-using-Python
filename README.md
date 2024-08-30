# Zomato-Analysis-using-Python

-------------------------------------------Zomato Analysis Project------------------------------------------------
Importing Library
import pandas as pd
import numpy as np
import matplotlib.pyplot as plt
import seaborn as sns

Create Data Frame
df = pd.read_csv('Zomato data .csv')
df.head(5)
df.info()

CONVERT THE DATA TYPE OF COLUMN - RATE(means replace denominator value from the rate column values)
def handleRate(value):
    value = str(value).split('/')
    value=value[0]
    return float (value);
df['rate'] = df['rate'].apply(handleRate)
print(df.head())

df.info()
-----------------------------------------------------------------------------------------------------------------------------------
1.What type of restaurant do the majority of customers order from?
sns.countplot(x=df['listed_in(type)'])
plt.xlabel("Type of Resturant")
CONCLUSION:majority of customers falls in dining category(means majority of customers are order from dining category resturant)?
----------------------------------------------------------------------------------------------------------------------------------
2.HOW MANY VOTES HAS EACH TYPE OF RESTURANT RECIEVED FROM CUSTOMERS
# Grouping the data by 'listed_in(type)' and summing the 'votes'
grouped_data = df.groupby('listed_in(type)')['votes'].sum()
# Creating a DataFrame from the grouped data
result = pd.DataFrame({'votes':grouped_data})

    # Plotting the result
plt.plot(result, c='green', marker='o')
plt.xlabel("TYPES OF RESTURANT", size=20)
plt.ylabel("VOTES", size=20)
plt.show()
CONCLUSION: Dining has received maximum votes
------------------------------------------------------------------------------------------------------------------------------
3.WHAT ARE THE RATINGS THAT THE MAJORITY OF RESTURANTS HAVE RECEIVED?
df.head()
plt.hist(df['rate'], bins=5)
plt.title("Rating Distribution")
plt.show()
CONCLUSION: The majority of restaurants have received a rating Between 3.5 to 4.
--------------------------------------------------------------------------------------------------------------------------------
4.ZOMATO HAS OBSERVED MOST COUPLES ORDER OF MOST OF THEIR FOOD ONINE. WAHT IS AVERAGE SPENDING ON EACH ORDER?
# Assuming 'df' is your DataFrame containing the 'approx_cost(for two people)' column
couple_data = df["approx_cost(for two people)"]
# Plotting the count plot
sns.countplot(x=couple_data, palette='coolwarm')
CONCLUSION: The majority of couples prefer restaurants with an approximate cost of 300 Rupees
-----------------------------------------------------------------------------------------------------------------------------

5. WHICH MODE (ONLINE OR OFFLINE) HAS RECEIVED THE MAXIMUM RATING
df.head()
plt.figure(figsize=(6,6))
sns.boxplot(x='online_order', y='rate', data = df)
CONCLUSION: Offline order received lower rating as compared to the Onlne order
---------------------------------------------------------------------------------------------------------------------------------

6.WHCH TYPE OF RESTAURANTS RECEIVED MORE OFFLINE ORDERS, SO THAT ZOMATO PAY CUSTOMERS TO SOME GOOD OFFERS ?
pivot_table = df.pivot_table(index='listed_in(type)', columns='online_order', aggfunc='size', fill_value=0)
sns.heatmap(pivot_table, annot=True, cmap="YlGnBu", fmt='d')
plt.title("Heatmap")
plt.xlabel("Online Order")
plt.ylabel("Listed in (Type)")
plt.show()
CONCLUSION: Dining restaurants primarily accept offline orders, whereas cafe primarily received online orders. This suggest that clints prefers
            orders in person at restaurants, but prefer online ordering at cafes.
