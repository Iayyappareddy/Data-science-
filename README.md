import pandas as pd
import matplotlib.pyplot as plt
import numpy as np
import seaborn as sb

data=pd.read_csv("F:\code\global_food_wastage_dataset.csv")
df=pd.DataFrame(data)


#CLAEANING DATA

#print(df.dropna(inplace=True)) #for null values

#print(df[df.duplicated()].count()) #for duplicates

df.columns=df.columns.str.lower()   #converting all column names into lowercase





#FORMATING
df['country']=df['country'].str.strip() #removing unwanted splaces
df['total waste (tons)']=pd.to_numeric(df['total waste (tons)'])
df['population (million)']=pd.to_numeric(df['population (million)'])






#DATA VISUALISATION

font1={'family':'serif','color':'blue','size':10}
font2={'family':'serif','color':'red','size':20}


#plot-1:food wastage in different countries
plt.figure(figsize=(10,8))
countries_waste=df.groupby('country',as_index=False)['total waste (tons)'].sum().sort_values(by='total waste (tons)')
plt.barh(countries_waste['country'],countries_waste['total waste (tons)'],color='green')
plt.title("Countries and food wastage",fontdict=font2)
plt.xlabel("Food waste in Tons",fontdict=font1)
plt.ylabel("All Countries",fontdict=font1)
plt.show()


#plot-2:Categories in food wastage
plt.figure(figsize=(10,8))
countries_waste=df.groupby('food category')['total waste (tons)'].sum()
countries_waste.plot(kind='pie',autopct='%1.2f%%')
plt.ylabel(" ")
plt.title("types of food waste",fontdict=font2)
plt.show()


#plot-3:Year wise food wastage
plt.figure(figsize=(10,8))
year_waste=df.groupby('year')['total waste (tons)'].mean()
year_waste.plot(kind='line',marker='o',ms=8,mec='black',mfc='green',lw=1)
plt.title("Year wise food wastage",fontdict=font2)
plt.xlabel("Year",fontdict=font1)
plt.ylabel("Food waste",fontdict=font1)
plt.grid(ls='--')
plt.show()


#plot-4:average waste percapita(for each person in country)
plt.figure(figsize=(10,8))
avg_wasteperperson=df.groupby('country')['avg waste per capita (kg)'].sum()
avg_wasteperperson.plot(kind='barh')
plt.title("Avg waste of percapita",fontdict=font2)
plt.ylabel("All Countries",fontdict=font1)
plt.xlabel("Food waste",fontdict=font1)
plt.show()


#plot-5:Household waste in different countries
plt.figure(figsize=(10,8))
avg_household=df.groupby('country')['household waste (%)'].mean()
avg_household.plot(kind='bar')
plt.title("'household waste",fontdict=font2)
plt.ylabel("All Countries",fontdict=font1)
plt.xlabel("Household waste",fontdict=font1)
plt.show()



#plot-6:Food year vs total food waste in different countries
plt.figure(figsize=(10,8))
country_food_year=df.groupby(['year','country'])['total waste (tons)'].sum().reset_index()
sb.relplot(data=country_food_year,x='year',y='total waste (tons)' ,hue='country',kind='line',marker='o' )
plt.title("food waste by year and in different countries",fontdict=font2)
plt.ylabel("food waste",fontdict=font1)
plt.xlabel("Year",fontdict=font1)
plt.show()


#plot-7:Total population and total waste comparision
population_waste=df.groupby('country')[['population (million)','total waste (tons)']].sum()
sb.scatterplot(data=population_waste,x='total waste (tons)',y='population (million)',hue='country')
plt.legend(bbox_to_anchor=(1,1),title='country')
plt.title("Total food waste population wise",fontdict=font2)
plt.ylabel("total waste (tons)",fontdict=font1)
plt.xlabel("population (million)",fontdict=font1)
plt.show()

#plot-8:Food category vs total food waste in different years
plt.figure(figsize=(10,8))
type_food_year=df.groupby(['year','food category'])['total waste (tons)'].sum().unstack()
sb.heatmap(type_food_year,annot=True,fmt='1f',annot_kws={'size':5} )
plt.title("food waste by year and category",fontdict=font2)
plt.ylabel("year",fontdict=font1)
plt.xlabel("food category",fontdict=font1)
plt.show()


#plot-9:population vs total waste and average percapita in defeferent countries
plt.figure(figsize=(10,8))
population_waste=df.groupby('country')[['population (million)','total waste (tons)','avg waste per capita (kg)']].sum()
sb.scatterplot(data=population_waste,x='total waste (tons)',y='population (million)',size='avg waste per capita (kg)',hue='country' ,sizes=(50,500))
plt.legend(loc='upper left',bbox_to_anchor=(1,1),title='country')
plt.title("Countries food waste based on population and percapita",fontdict=font2)
plt.ylabel("total waste (tons)",fontdict=font1)
plt.xlabel("population (million)",fontdict=font1)
plt.show()


