## Dataset link https://www.kaggle.com/datasets/atharvaarya25/financials
##The code for jupyter Notebook....
##
##import pandas in notebook and load your csv file
import pandas as pd
df=pd.read_csv('Financials.csv')
print(df)
df.head(5)
#for analysis we have to perform data cleaning process since file contain object data type also some white spaces 
df.isnull().sum() ##check how many white spaces
# for removing spaces
df.columns=df.columns.str.strip()
df.dtypes ##check type of data
## for removing dollor 
for column in df.columns:
    df[column] = df[column].replace('[\$,]', '', regex=True)
   ## for replace - to 0
df['Discounts']=df['Discounts'].replace('-','0',regex=True)
df.head()    
## for converting float data type
df['Units Sold']=df['Units Sold'].astype(float)
df['Manufacturing Price']=df['Manufacturing Price'].astype(float)
df['Sale Price']=df['Sale Price'].astype(float)
df['Gross Sales']=df['Gross Sales'].astype(float)
df['Discounts']=df['Discounts'].astype(float)
df['Sales']=df['Sales'].astype(float)
df['COGS']=df['COGS'].astype(float)
## for date time 
df['Date']=pd.to_datetime(df['Date'])
df.head(5)
## for profit column contain some space which is removing & convert to float data type
df = df[df['Profit'] != ' -   ']
df['Profit']=df['Profit'].str.strip()
df['Profit']=df['Profit'].replace(r'[()]','', regex = True)
df['Profit']= df['Profit'].astype(float)
df.dtypes
df=df.drop(['Month Number', 'Month Name', 'Year'], axis=1)
## now perform analysis by visualization & plotting
import numpy as np
import matplotlib.pyplot as plt
## Country wise analysis
group=df.groupby('Country').agg({'Sales':'sum', 'Profit':'sum', 'Units Sold':'sum', 'COGS':'sum'}).reset_index()
group.head(10)
bar_width = 0.2
plt.figure(figsize=(8,4))
#Calculate the x positions for the bars
x = np.arange(len(group['Country']))
plt.bar(x-bar_width, group['Sales'], bar_width, label='Sales')
plt.xticks(x, group['Country'])
plt.legend()
plt.title('Country wise Total Sales')
plt.show()
#### We can see that highest sale is noticed in USA and in Canada and lowest sale is recorded in Mexico

plt.figure(figsize=(10,6))
x = np.arange(len(group['Country']))
plt.bar(x-bar_width, group['Sales'], bar_width, label='Sales')
plt.bar(x, group['COGS'], bar_width, label='COGS')
#plt.bar(x+bar_width, group['Units Sold'], bar_width, label='Units Sold')
plt.xlabel('Country')
plt.ylabel('Values')
plt.xticks(x, group['Country'])
plt.legend()
plt.title('Analysis - company is in Profit or Not')
plt.show()
#### We can see that Sales is more than the COGS(Cost of Gross Sale) it means company is in profit  

plt.figure(figsize=(10,6))
plt.pie(group['Profit'], labels=group['Country'], autopct='%1.1f%%', startangle=140)
plt.title('Country wise Profit')
plt.show()
#Product wise analysis
group_product=df.groupby('Product').agg({'Units Sold':'sum', 'Manufacturing Price':'mean', 'Sale Price':'mean', 'Gross Sales':'sum', 'Profit':'sum', 'COGS':'sum'}).reset_index()
# we can create one more column which contain margin that is selling price minus manufacturing price

group_product['Gross Margin']=group_product['Sale Price']-group_product['Manufacturing Price']
#Now analyse the margins in all products
plt.figure(figsize=(10,6))
categories=group_product['Product']
margin_values=group_product['Gross Margin']
# Create a bar chart
plt.bar(categories, margin_values, label='Gross Margin')
plt.xticks(group_product['Product'])
plt.title('Margin In Products')
plt.show()
#### From above figure we can see that Product 'Amarilla' and product 'VTT' has negative gross margin and product 'Velo' has very low margin

plt.figure(figsize=(10,6))
plt.pie(group_product['Profit'], labels=group_product['Product'], autopct='%1.1f%%', startangle=140)
plt.title('Product wise Profit')
plt.show()
## From here we can see that Paseo has highest profit and Montana have low profit

plt.figure(figsize=(10,6))
x = np.arange(len(group_product['Product']))
plt.bar(x-bar_width, group_product['Gross Sales'], bar_width, label='Sales')
plt.bar(x, group_product['COGS'], bar_width, label='COGS')
#plt.bar(x+bar_width, group['Units Sold'], bar_width, label='Units Sold')
plt.xlabel('Country')
plt.ylabel('Values')
plt.xticks(x, group_product['Product'])
plt.legend()
plt.title('Analysis- Which Product Give More Profit')
plt.show()
### Sales of Paseo is much more than other Products and Carretera has the low sales 
  
bar_width = 0.3
plt.figure(figsize=(10,6))
#Calculate the x positions for the bars
x = np.arange(len(group_product['Product']))
plt.bar(x-bar_width, group_product['Units Sold'], bar_width, label='Units Sold')
plt.xticks(x, group_product['Product'])
plt.legend()
plt.title('Analysis- Maximum Number of Product sell')
plt.show()
### From here we can clearly see that Paseo is the product which sells maximum 


