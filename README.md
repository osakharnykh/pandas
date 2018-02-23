

```python
import pandas as pd
```


```python
#Concatenate datasets into one data frame
file1='purchase_data.json'
file2='purchase_data2.json'

p1=pd.read_json(file1)
p2=pd.read_json(file2)
frames=[p1,p2]
p_data=pd.concat(frames)
```


```python
#Total players
t_players=p_data['SN'].count()
#But total unique players by SN
t_un_players=p_data['SN'].nunique()
total_players=pd.DataFrame(columns=("Total Players",'Unique Players'))
total_players.loc[0]=[t_players,t_un_players]
total_players
```




<div>
<style>
    .dataframe thead tr:only-child th {
        text-align: right;
    }

    .dataframe thead th {
        text-align: left;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Total Players</th>
      <th>Unique Players</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>858</td>
      <td>612</td>
    </tr>
  </tbody>
</table>
</div>




```python
#Purchasing Analysis (Players)

#Number of Unique items
n_items_un=p_data['Item Name'].nunique()

#Average Purchase Price
av_price=round(p_data['Price'].mean(),2)
 
#Total Purchase Value
total_purch=round(p_data['Price'].sum(),2)
 
#Normalized Totals

purchase_analysis=pd.DataFrame(columns=('Number of Unique Items','Average Purchase Price',
                                       'Total Purchase Value'))
purchase_analysis.loc[0]=[n_items_un,'$'+str(av_price),'$'+str(total_purch)]
purchase_analysis
```




<div>
<style>
    .dataframe thead tr:only-child th {
        text-align: right;
    }

    .dataframe thead th {
        text-align: left;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Number of Unique Items</th>
      <th>Average Purchase Price</th>
      <th>Total Purchase Value</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>180</td>
      <td>$2.93</td>
      <td>$2514.43</td>
    </tr>
  </tbody>
</table>
</div>




```python
#Gender Demographics

player_unique=p_data[['SN','Gender']]
player_unique=player_unique.drop_duplicates()
num_player_unique=player_unique['SN'].count()

#Percentage and Count of Male Players
players_male=len(player_unique[player_unique['Gender']=='Male'])
perc_male=round(players_male/num_player_unique,2)

#Percentage and Count of Female Players
players_female=len(player_unique[player_unique['Gender']=='Female'])
perc_female=round(players_female/num_player_unique,2)

#Percentage and Count of Other / Non-Disclosed
players_other=num_player_unique-players_male-players_female
perc_other=round(players_other/num_player_unique,2)

gender_demogr=pd.DataFrame(columns=('','Count','Percentage'))
gender_demogr.loc[0]=['Male',players_male,perc_male]
gender_demogr.loc[1]=['Female',players_female,perc_female]
gender_demogr.loc[2]=['Other/Non-Disclosed',players_other,perc_other]
gender_demogr
```




<div>
<style>
    .dataframe thead tr:only-child th {
        text-align: right;
    }

    .dataframe thead th {
        text-align: left;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th></th>
      <th>Count</th>
      <th>Percentage</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>Male</td>
      <td>498</td>
      <td>0.80</td>
    </tr>
    <tr>
      <th>1</th>
      <td>Female</td>
      <td>112</td>
      <td>0.18</td>
    </tr>
    <tr>
      <th>2</th>
      <td>Other/Non-Disclosed</td>
      <td>9</td>
      <td>0.01</td>
    </tr>
  </tbody>
</table>
</div>




```python
#Purchasing Analysis (Gender)

#Purchase Count
purchase_male=len(p_data[p_data['Gender']=='Male'])
purchase_female=len(p_data[p_data['Gender']=='Female'])
purchase_other=t_players-purchase_male-purchase_female

#Total Purchase Value
p_val_male=round(p_data[p_data['Gender']=='Male']['Price'].sum(),2)
p_val_female=round(p_data[p_data['Gender']=='Female']['Price'].sum(),2)
p_val_other=round(p_data['Price'].sum()-p_val_male-p_val_female,2)

#Average Purchase Price
av_pur_male=round(p_val_male/purchase_male,2)
av_pur_female=round(p_val_female/purchase_female,2)
av_pur_other=round(p_val_other/purchase_other,2)

#Normalized Totals
n_pur_male=round(p_val_male/players_male,2)
n_pur_female=round(p_val_female/players_female,2)
n_pur_other=round(p_val_other/players_other,2)

gen_analysis=pd.DataFrame(columns=('','Purchase Count','Total Value','Average Price','Normalized'))
gen_analysis.loc[0]=['Male',purchase_male,'$'+str(p_val_male),'$'+str(av_pur_male),'$'+str(n_pur_male)]
gen_analysis.loc[1]=['Female',purchase_female,'$'+str(p_val_female),'$'+str(av_pur_female),'$'+str(n_pur_female)]
gen_analysis.loc[2]=['Other/Non-Disclosed',purchase_other,'$'+str(p_val_other),'$'+str(av_pur_other),'$'+str(n_pur_other)]
gen_analysis
```




<div>
<style>
    .dataframe thead tr:only-child th {
        text-align: right;
    }

    .dataframe thead th {
        text-align: left;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th></th>
      <th>Purchase Count</th>
      <th>Total Value</th>
      <th>Average Price</th>
      <th>Normalized</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>Male</td>
      <td>697</td>
      <td>$2052.28</td>
      <td>$2.94</td>
      <td>$4.12</td>
    </tr>
    <tr>
      <th>1</th>
      <td>Female</td>
      <td>149</td>
      <td>$424.29</td>
      <td>$2.85</td>
      <td>$3.79</td>
    </tr>
    <tr>
      <th>2</th>
      <td>Other/Non-Disclosed</td>
      <td>12</td>
      <td>$37.86</td>
      <td>$3.16</td>
      <td>$4.21</td>
    </tr>
  </tbody>
</table>
</div>




```python
#Age Demographics

#The below each broken into bins of 4 years (i.e. <10, 10-14, 15-19, etc.)
bins=[0,10,15,20,25,30,35,40,120]
group_names=['<10','10-14','15-19','20-24','25-29','30-34','35-39','40+']
p_data['Age range']=pd.cut(p_data['Age'],bins,labels=group_names)

#We'll need unique age ranges for normalization. Ugh...
ages_unique=p_data[['SN','Age range']]
ages_unique=ages_unique.drop_duplicates()
num_age_unique=[]
for i in range(0,8):
    num_age_unique.append(len(ages_unique[ages_unique['Age range']==group_names[i]]))


#Purchase Count,Total Purchase Value,Average Purchase Price, Normalized Totals
p_data_age=pd.DataFrame(columns=('Age Group','Purchase Count','Total Value','Average Price','Normalized Price'))
for i in range(0,8):
    p_data_age.loc[i]=[group_names[i],
                       len(p_data[p_data['Age range']==group_names[i]]),
                       '$'+str(round(p_data[p_data['Age range']==group_names[i]]['Price'].sum(),2)),
                       '$'+str(round(p_data[p_data['Age range']==group_names[i]]['Price'].sum()/len(p_data[p_data['Age range']==group_names[i]]),2)),
                       '$'+str(round(p_data[p_data['Age range']==group_names[i]]['Price'].sum()/num_age_unique[i],2))]
    
#p_data_age    
p_data_age
```




<div>
<style>
    .dataframe thead tr:only-child th {
        text-align: right;
    }

    .dataframe thead th {
        text-align: left;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Age Group</th>
      <th>Purchase Count</th>
      <th>Total Value</th>
      <th>Average Price</th>
      <th>Normalized Price</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>&lt;10</td>
      <td>37</td>
      <td>$110.44</td>
      <td>$2.98</td>
      <td>$4.09</td>
    </tr>
    <tr>
      <th>1</th>
      <td>10-14</td>
      <td>82</td>
      <td>$236.36</td>
      <td>$2.88</td>
      <td>$4.15</td>
    </tr>
    <tr>
      <th>2</th>
      <td>15-19</td>
      <td>204</td>
      <td>$583.43</td>
      <td>$2.86</td>
      <td>$3.67</td>
    </tr>
    <tr>
      <th>3</th>
      <td>20-24</td>
      <td>338</td>
      <td>$1003.03</td>
      <td>$2.97</td>
      <td>$3.87</td>
    </tr>
    <tr>
      <th>4</th>
      <td>25-29</td>
      <td>80</td>
      <td>$230.59</td>
      <td>$2.88</td>
      <td>$4.12</td>
    </tr>
    <tr>
      <th>5</th>
      <td>30-34</td>
      <td>65</td>
      <td>$194.73</td>
      <td>$3.0</td>
      <td>$3.89</td>
    </tr>
    <tr>
      <th>6</th>
      <td>35-39</td>
      <td>49</td>
      <td>$147.21</td>
      <td>$3.0</td>
      <td>$4.91</td>
    </tr>
    <tr>
      <th>7</th>
      <td>40+</td>
      <td>3</td>
      <td>$8.64</td>
      <td>$2.88</td>
      <td>$2.88</td>
    </tr>
  </tbody>
</table>
</div>




```python
#Top Spenders
#Identify the the top 5 spenders in the game by total purchase value, then list (in a table):

spenders=p_data[['SN','Price']]
spenders['Count']=1

#SN
spenders_g=spenders.groupby('SN')
#Purchase Count
spenders_g1=pd.DataFrame(spenders_g['Count'].value_counts())
spenders_g1=spenders_g1.rename(columns={'Count':'Purchase Count'})
spenders_g1=spenders_g1.reset_index()
#Total Purchase Value
spenders_g2=pd.DataFrame(spenders_g['Price'].sum())
spenders_g2=spenders_g2.rename(columns={'Price':'Total Purchase Value'})
spenders_g2=spenders_g2.reset_index()
spenders_g_fin=pd.merge(spenders_g1,spenders_g2,on="SN")
spenders_g_fin=spenders_g_fin[['SN','Purchase Count','Total Purchase Value']]
#Average Purchase Price
spenders_g_fin['Average Purchase Price']=round(spenders_g_fin['Total Purchase Value']/spenders_g_fin['Purchase Count'],2)

top_spenders=spenders_g_fin.sort_values("Total Purchase Value",ascending=False).head()
top_spenders


```

    /Users/osakharnykh/anaconda3/lib/python3.6/site-packages/ipykernel_launcher.py:5: SettingWithCopyWarning: 
    A value is trying to be set on a copy of a slice from a DataFrame.
    Try using .loc[row_indexer,col_indexer] = value instead
    
    See the caveats in the documentation: http://pandas.pydata.org/pandas-docs/stable/indexing.html#indexing-view-versus-copy
      """





<div>
<style>
    .dataframe thead tr:only-child th {
        text-align: right;
    }

    .dataframe thead th {
        text-align: left;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>SN</th>
      <th>Purchase Count</th>
      <th>Total Purchase Value</th>
      <th>Average Purchase Price</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>574</th>
      <td>Undirrala66</td>
      <td>5</td>
      <td>17.06</td>
      <td>3.41</td>
    </tr>
    <tr>
      <th>22</th>
      <td>Aerithllora36</td>
      <td>4</td>
      <td>15.10</td>
      <td>3.78</td>
    </tr>
    <tr>
      <th>459</th>
      <td>Saedue76</td>
      <td>4</td>
      <td>13.56</td>
      <td>3.39</td>
    </tr>
    <tr>
      <th>503</th>
      <td>Sondim43</td>
      <td>4</td>
      <td>13.02</td>
      <td>3.26</td>
    </tr>
    <tr>
      <th>382</th>
      <td>Mindimnya67</td>
      <td>4</td>
      <td>12.74</td>
      <td>3.18</td>
    </tr>
  </tbody>
</table>
</div>




```python
#Most Popular Items
#Identify the 5 most popular items by purchase count, then list (in a table):

#ID
items=p_data[['Item ID','Item Name','Price']]
items_g=items.groupby('Item ID')
#Total PurchaseValue
items_gr1=pd.DataFrame(items_g['Price'].sum())
items_gr1=items_gr1.rename(columns={'Price':'Total Purchase Value'})
items_gr1=items_gr1.reset_index()
#Item Name,Purchase Count
items_gr2=pd.DataFrame(items_g['Item Name'].value_counts())
items_gr2=items_gr2.rename(columns={'Item Name':'Purchase Count'})
items_gr2=items_gr2.reset_index()

#suddenly: one item ID can correspond to multiple prices

items_fin=pd.merge(items_gr1,items_gr2,on="Item ID")
items_fin=items_fin[['Item ID','Item Name','Purchase Count','Total Purchase Value']]
#Average Price then
items_fin['Average Price']=round(items_fin['Total Purchase Value']/items_fin['Purchase Count'],2)

top_pop_items=items_fin.sort_values("Purchase Count",ascending=False).head()
top_pop_items

```




<div>
<style>
    .dataframe thead tr:only-child th {
        text-align: right;
    }

    .dataframe thead th {
        text-align: left;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Item ID</th>
      <th>Item Name</th>
      <th>Purchase Count</th>
      <th>Total Purchase Value</th>
      <th>Average Price</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>84</th>
      <td>84</td>
      <td>Arcane Gem</td>
      <td>12</td>
      <td>29.34</td>
      <td>2.45</td>
    </tr>
    <tr>
      <th>39</th>
      <td>39</td>
      <td>Betrayal, Whisper of Grieving Widows</td>
      <td>11</td>
      <td>25.85</td>
      <td>2.35</td>
    </tr>
    <tr>
      <th>31</th>
      <td>31</td>
      <td>Trickster</td>
      <td>10</td>
      <td>23.22</td>
      <td>2.32</td>
    </tr>
    <tr>
      <th>44</th>
      <td>44</td>
      <td>Bonecarvin Battle Axe</td>
      <td>9</td>
      <td>24.04</td>
      <td>2.67</td>
    </tr>
    <tr>
      <th>154</th>
      <td>154</td>
      <td>Feral Katana</td>
      <td>9</td>
      <td>23.55</td>
      <td>2.62</td>
    </tr>
  </tbody>
</table>
</div>




```python
#Most Profitable Items
#Identify the 5 most profitable items by total purchase value, then list (in a table):

top_prof_items=items_fin.sort_values("Total Purchase Value",ascending=False).head()
top_prof_items

```




<div>
<style>
    .dataframe thead tr:only-child th {
        text-align: right;
    }

    .dataframe thead th {
        text-align: left;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Item ID</th>
      <th>Item Name</th>
      <th>Purchase Count</th>
      <th>Total Purchase Value</th>
      <th>Average Price</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>34</th>
      <td>34</td>
      <td>Retribution Axe</td>
      <td>9</td>
      <td>37.26</td>
      <td>4.14</td>
    </tr>
    <tr>
      <th>107</th>
      <td>107</td>
      <td>Splitter, Foe Of Subtlety</td>
      <td>9</td>
      <td>33.03</td>
      <td>3.67</td>
    </tr>
    <tr>
      <th>115</th>
      <td>115</td>
      <td>Spectral Diamond Doomblade</td>
      <td>7</td>
      <td>29.75</td>
      <td>4.25</td>
    </tr>
    <tr>
      <th>32</th>
      <td>32</td>
      <td>Orenmir</td>
      <td>6</td>
      <td>29.70</td>
      <td>4.95</td>
    </tr>
    <tr>
      <th>84</th>
      <td>84</td>
      <td>Arcane Gem</td>
      <td>12</td>
      <td>29.34</td>
      <td>2.45</td>
    </tr>
  </tbody>
</table>
</div>


