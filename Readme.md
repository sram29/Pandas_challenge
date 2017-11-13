

```python
import pandas as pd
import os
```


```python
#read the file
pymo_file = os.path.join('Pymoli', 'purchase_data.json')
pymo_raw = pd.read_json(pymo_file)
pymo_raw.head()
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
      <th>Age</th>
      <th>Gender</th>
      <th>Item ID</th>
      <th>Item Name</th>
      <th>Price</th>
      <th>SN</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>38</td>
      <td>Male</td>
      <td>165</td>
      <td>Bone Crushing Silver Skewer</td>
      <td>3.37</td>
      <td>Aelalis34</td>
    </tr>
    <tr>
      <th>1</th>
      <td>21</td>
      <td>Male</td>
      <td>119</td>
      <td>Stormbringer, Dark Blade of Ending Misery</td>
      <td>2.32</td>
      <td>Eolo46</td>
    </tr>
    <tr>
      <th>2</th>
      <td>34</td>
      <td>Male</td>
      <td>174</td>
      <td>Primitive Blade</td>
      <td>2.46</td>
      <td>Assastnya25</td>
    </tr>
    <tr>
      <th>3</th>
      <td>21</td>
      <td>Male</td>
      <td>92</td>
      <td>Final Critic</td>
      <td>1.36</td>
      <td>Pheusrical25</td>
    </tr>
    <tr>
      <th>4</th>
      <td>23</td>
      <td>Male</td>
      <td>63</td>
      <td>Stormfury Mace</td>
      <td>1.27</td>
      <td>Aela59</td>
    </tr>
  </tbody>
</table>
</div>




```python
#Total Number of Players
Total_count = pymo_raw['SN'].nunique()
Total_count
```




    573




```python
#Average Purchase Price
average_price = pymo_raw['Price'].mean()
average_price
```




    2.931192307692303




```python
#Total purchase for an item
unique_count = pymo_raw.groupby(['Item Name'])['Price'].sum()
unique_count.head()
```




    Item Name
    Abyssal Shard                      6.12
    Aetherius, Boon of the Blessed    19.00
    Agatha                             9.55
    Alpha                             10.92
    Alpha, Oath of Zeal               20.16
    Name: Price, dtype: float64




```python
#Total Number of Purchases
Total_purchases = pymo_raw['Price'].count()
Total_purchases
```




    780




```python
#Total Revenue
Total_revenue = pymo_raw['Price'].sum()
Total_revenue
```




    2286.3299999999963




```python
#count of Male, Female, and other/undisclosed
gender_count = pd.DataFrame(pymo_raw.groupby(['Gender'])['SN'].nunique())
gender_count = gender_count.rename(columns={"SN":"Count"})
gender_count

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
      <th>Count</th>
    </tr>
    <tr>
      <th>Gender</th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>Female</th>
      <td>100</td>
    </tr>
    <tr>
      <th>Male</th>
      <td>465</td>
    </tr>
    <tr>
      <th>Other / Non-Disclosed</th>
      <td>8</td>
    </tr>
  </tbody>
</table>
</div>




```python
#Percentage of Male, Female, and other/undisclosed Players
gender_percent = (gender_count/gender_count.sum())*100

gender_count['Percent'] = gender_percent
gender_count
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
      <th>Count</th>
      <th>Percent</th>
    </tr>
    <tr>
      <th>Gender</th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>Female</th>
      <td>100</td>
      <td>17.452007</td>
    </tr>
    <tr>
      <th>Male</th>
      <td>465</td>
      <td>81.151832</td>
    </tr>
    <tr>
      <th>Other / Non-Disclosed</th>
      <td>8</td>
      <td>1.396161</td>
    </tr>
  </tbody>
</table>
</div>




```python
#Purchasing Analysis (Gender)** 

#The below each broken by gender
new_df = pymo_raw[["Gender","Item Name","Price"]]
gender_purchase_count = pd.DataFrame(new_df.groupby(['Gender', 'Item Name']).count())
#gender_purchase_count = gender_purchase_count['Item Name'].count()
gender_purchase_count = gender_purchase_count.rename(columns={"Price":"Count"})
gender_purchase_count.head()
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
    </tr>
    <tr>
      <th>Gender</th>
      <th>Item Name</th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th rowspan="5" valign="top">Female</th>
      <th>Alpha</th>
      <td>2</td>
    </tr>
    <tr>
      <th>Alpha, Oath of Zeal</th>
      <td>2</td>
    </tr>
    <tr>
      <th>Alpha, Reach of Ending Hope</th>
      <td>1</td>
    </tr>
    <tr>
      <th>Amnesia</th>
      <td>2</td>
    </tr>
    <tr>
      <th>Apocalyptic Battlescythe</th>
      <td>2</td>
    </tr>
  </tbody>
</table>
</div>




```python
#Purchase Count
gender_purchase_count_male = gender_purchase_count.loc['Male']
gender_purchase_count_female = gender_purchase_count.loc['Female']
gender_purchase_count_other = gender_purchase_count.loc['Other / Non-Disclosed']
print("Male purchase count: " + (str(gender_purchase_count_male['Count'].sum())))
print("Female purchase count: " + (str(gender_purchase_count_female['Count'].sum())))
print("Other/Nondisclosed purchase count: " + (str(gender_purchase_count_other['Count'].sum())))

#Total Purchase Value
#Normalized Totals

```

    Male purchase count: 633
    Female purchase count: 136
    Other/Nondisclosed purchase count: 11



```python
sum_df = pymo_raw[["Gender","Item Name","Price"]]
gender_purchase_sum = pd.DataFrame(sum_df.groupby(['Gender','Item Name'])['Price'].sum())
gender_purchase_sum.head()

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
      <th>Price</th>
    </tr>
    <tr>
      <th>Gender</th>
      <th>Item Name</th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th rowspan="5" valign="top">Female</th>
      <th>Alpha</th>
      <td>3.12</td>
    </tr>
    <tr>
      <th>Alpha, Oath of Zeal</th>
      <td>5.76</td>
    </tr>
    <tr>
      <th>Alpha, Reach of Ending Hope</th>
      <td>1.55</td>
    </tr>
    <tr>
      <th>Amnesia</th>
      <td>7.14</td>
    </tr>
    <tr>
      <th>Apocalyptic Battlescythe</th>
      <td>7.82</td>
    </tr>
  </tbody>
</table>
</div>




```python
#Total Purchase value
gender_purchase_sum_male = gender_purchase_sum.loc['Male']
gender_purchase_sum_female = gender_purchase_sum.loc['Female']
gender_purchase_sum_other = gender_purchase_sum.loc['Other / Non-Disclosed']
print("Male purchase price: " + (str(gender_purchase_sum_male['Price'].sum())))
print("Female purchase price: " + (str(gender_purchase_sum_female['Price'].sum())))
print("Other/Nondisclosed purchase price: " + (str(gender_purchase_sum_other['Price'].sum())))
```

    Male purchase price: 1867.6800000000007
    Female purchase price: 382.9100000000001
    Other/Nondisclosed purchase price: 35.74



```python
#Average Purchase Price
gender_purchase_price_male = (gender_purchase_sum_male.sum()/465)
gender_purchase_price_female = (gender_purchase_sum_female.sum()/100)
gender_purchase_price_other = (gender_purchase_sum_other.sum()/8)
print("Male: " + str(gender_purchase_price_male))
print("Female: " + str(gender_purchase_price_female))
print("Other: " + str(gender_purchase_price_other))
```

    Male: Price    4.016516
    dtype: float64
    Female: Price    3.8291
    dtype: float64
    Other: Price    4.4675
    dtype: float64



```python
#Normalized Total
gender_normalized_price_male = (gender_purchase_sum_male.sum()/633)
gender_normalized_price_female = (gender_purchase_sum_female.sum()/136)
gender_normalized_price_other = (gender_purchase_sum_other.sum()/11)
#male_purchase_percent = pymo_raw.loc('Female', gender_purchase_percent)
#male_purchase_percent
print("Male: " + str(gender_normalized_price_male))
print("Female: " + str(gender_normalized_price_female))
print("Other: " + str(gender_normalized_price_other))
```

    Male: Price    2.950521
    dtype: float64
    Female: Price    2.815515
    dtype: float64
    Other: Price    3.249091
    dtype: float64



```python
gender_data = pd.DataFrame({'Gender' : ['Male', 'Female', 'Other'],
                       'Purchase Count' : ['633', '136', '11'],
                       'Average Purchase price' : ['4.01', '3.82', '4.46'],
                       'Total Purchase Value' : ['1867.68', '382.91', '35.74'],
                       'Normalized Totals' : ['2.95', '2.81', '3.24']})
gender_data = gender_data.set_index("Gender")
gender_data
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
      <th>Average Purchase price</th>
      <th>Normalized Totals</th>
      <th>Purchase Count</th>
      <th>Total Purchase Value</th>
    </tr>
    <tr>
      <th>Gender</th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>Male</th>
      <td>4.01</td>
      <td>2.95</td>
      <td>633</td>
      <td>1867.68</td>
    </tr>
    <tr>
      <th>Female</th>
      <td>3.82</td>
      <td>2.81</td>
      <td>136</td>
      <td>382.91</td>
    </tr>
    <tr>
      <th>Other</th>
      <td>4.46</td>
      <td>3.24</td>
      <td>11</td>
      <td>35.74</td>
    </tr>
  </tbody>
</table>
</div>




```python
bins = [6, 10, 14, 19, 24, 29, 34, 39, 44, 49, 54]
group_labels = ["6 to 10", "10 to 14", "15 to 19", "20 to 24", "25 to 29", "30 to 34", "35 to 39", "40 to 45","46 to 50", "50 to 54"]
pd.cut(pymo_raw["Age"],bins,labels=group_labels).head()
```




    0    35 to 39
    1    20 to 24
    2    30 to 34
    3    20 to 24
    4    20 to 24
    Name: Age, dtype: category
    Categories (10, object): [10 to 14 < 15 to 19 < 20 to 24 < 25 to 29 ... 40 to 45 < 46 to 50 < 50 to 54 < 6 to 10]




```python
pymo_raw ['Age group'] = pd.cut(pymo_raw["Age"],bins,labels=group_labels)
pymo_age = pd.DataFrame(pymo_raw)
pymo_age.head()
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
      <th>Age</th>
      <th>Gender</th>
      <th>Item ID</th>
      <th>Item Name</th>
      <th>Price</th>
      <th>SN</th>
      <th>Age group</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>38</td>
      <td>Male</td>
      <td>165</td>
      <td>Bone Crushing Silver Skewer</td>
      <td>3.37</td>
      <td>Aelalis34</td>
      <td>35 to 39</td>
    </tr>
    <tr>
      <th>1</th>
      <td>21</td>
      <td>Male</td>
      <td>119</td>
      <td>Stormbringer, Dark Blade of Ending Misery</td>
      <td>2.32</td>
      <td>Eolo46</td>
      <td>20 to 24</td>
    </tr>
    <tr>
      <th>2</th>
      <td>34</td>
      <td>Male</td>
      <td>174</td>
      <td>Primitive Blade</td>
      <td>2.46</td>
      <td>Assastnya25</td>
      <td>30 to 34</td>
    </tr>
    <tr>
      <th>3</th>
      <td>21</td>
      <td>Male</td>
      <td>92</td>
      <td>Final Critic</td>
      <td>1.36</td>
      <td>Pheusrical25</td>
      <td>20 to 24</td>
    </tr>
    <tr>
      <th>4</th>
      <td>23</td>
      <td>Male</td>
      <td>63</td>
      <td>Stormfury Mace</td>
      <td>1.27</td>
      <td>Aela59</td>
      <td>20 to 24</td>
    </tr>
  </tbody>
</table>
</div>




```python
age_df = pymo_age[["Age group","Item Name","Price"]]
gender_age = pd.DataFrame(age_df.groupby(["Age group", "Item Name"])["Price"].sum())
#gender_age_sum = gender_age_sum["Item Name"].count()
gender_age.head()
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
      <th>Price</th>
    </tr>
    <tr>
      <th>Age group</th>
      <th>Item Name</th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th rowspan="5" valign="top">10 to 14</th>
      <th>Aetherius, Boon of the Blessed</th>
      <td>4.75</td>
    </tr>
    <tr>
      <th>Azurewrath</th>
      <td>4.44</td>
    </tr>
    <tr>
      <th>Betrayal, Whisper of Grieving Widows</th>
      <td>2.35</td>
    </tr>
    <tr>
      <th>Blazeguard, Reach of Eternity</th>
      <td>4.00</td>
    </tr>
    <tr>
      <th>Brimstone</th>
      <td>2.52</td>
    </tr>
  </tbody>
</table>
</div>




```python
age_count = pd.DataFrame(pymo_raw.groupby(['Age group'])['SN'].nunique())
age_count = age_count.rename(columns={"SN":"Count"})
age_count
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
      <th>Count</th>
    </tr>
    <tr>
      <th>Age group</th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>10 to 14</th>
      <td>20</td>
    </tr>
    <tr>
      <th>15 to 19</th>
      <td>100</td>
    </tr>
    <tr>
      <th>20 to 24</th>
      <td>259</td>
    </tr>
    <tr>
      <th>25 to 29</th>
      <td>87</td>
    </tr>
    <tr>
      <th>30 to 34</th>
      <td>47</td>
    </tr>
    <tr>
      <th>35 to 39</th>
      <td>27</td>
    </tr>
    <tr>
      <th>40 to 45</th>
      <td>10</td>
    </tr>
    <tr>
      <th>46 to 50</th>
      <td>1</td>
    </tr>
    <tr>
      <th>50 to 54</th>
      <td>0</td>
    </tr>
    <tr>
      <th>6 to 10</th>
      <td>22</td>
    </tr>
  </tbody>
</table>
</div>




```python
age_percent = (age_count/age_count.sum())*100

age_count['Percent'] = age_percent
age_count
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
      <th>Count</th>
      <th>Percent</th>
    </tr>
    <tr>
      <th>Age group</th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>10 to 14</th>
      <td>20</td>
      <td>3.490401</td>
    </tr>
    <tr>
      <th>15 to 19</th>
      <td>100</td>
      <td>17.452007</td>
    </tr>
    <tr>
      <th>20 to 24</th>
      <td>259</td>
      <td>45.200698</td>
    </tr>
    <tr>
      <th>25 to 29</th>
      <td>87</td>
      <td>15.183246</td>
    </tr>
    <tr>
      <th>30 to 34</th>
      <td>47</td>
      <td>8.202443</td>
    </tr>
    <tr>
      <th>35 to 39</th>
      <td>27</td>
      <td>4.712042</td>
    </tr>
    <tr>
      <th>40 to 45</th>
      <td>10</td>
      <td>1.745201</td>
    </tr>
    <tr>
      <th>46 to 50</th>
      <td>1</td>
      <td>0.174520</td>
    </tr>
    <tr>
      <th>50 to 54</th>
      <td>0</td>
      <td>0.000000</td>
    </tr>
    <tr>
      <th>6 to 10</th>
      <td>22</td>
      <td>3.839442</td>
    </tr>
  </tbody>
</table>
</div>




```python
count_age_df = pymo_raw[["Age group","Item Name","Price"]]
age_purchase_count = pd.DataFrame(count_age_df.groupby(['Age group', 'Item Name']).count())
#gender_purchase_count = gender_purchase_count['Item Name'].count()
age_purchase_count = age_purchase_count.rename(columns={"Price":"Count"})
age_purchase_count1 = age_purchase_count.fillna(value=0)
age_purchase_count1.head()

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
    </tr>
    <tr>
      <th>Age group</th>
      <th>Item Name</th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th rowspan="5" valign="top">10 to 14</th>
      <th>Abyssal Shard</th>
      <td>0.0</td>
    </tr>
    <tr>
      <th>Aetherius, Boon of the Blessed</th>
      <td>1.0</td>
    </tr>
    <tr>
      <th>Agatha</th>
      <td>0.0</td>
    </tr>
    <tr>
      <th>Alpha</th>
      <td>0.0</td>
    </tr>
    <tr>
      <th>Alpha, Oath of Zeal</th>
      <td>0.0</td>
    </tr>
  </tbody>
</table>
</div>




```python
age_purchase_sum1 = age_purchase_count1.loc['10 to 14']
age_purchase_sum2 = age_purchase_count1.loc['15 to 19']
age_purchase_sum3 = age_purchase_count1.loc['20 to 24']
age_purchase_sum4 = age_purchase_count1.loc['25 to 29']
age_purchase_sum5 = age_purchase_count1.loc['30 to 34']
age_purchase_sum6 = age_purchase_count1.loc['35 to 39']
age_purchase_sum7 = age_purchase_count1.loc['40 to 45']
age_purchase_sum8 = age_purchase_count1.loc['46 to 50']
age_purchase_sum9 = age_purchase_count1.loc['6 to 10']
print("10 to 14 purchase count: " + (str(age_purchase_sum1['Count'].sum())))
print("15 to 19 purchase count: " + (str(age_purchase_sum2['Count'].sum())))
print("20 to 24 purchase count: " + (str(age_purchase_sum3['Count'].sum())))
print("25 to 29 purchase count: " + (str(age_purchase_sum4['Count'].sum())))
print("30 to 34 purchase count: " + (str(age_purchase_sum5['Count'].sum())))
print("35 to 39 purchase count: " + (str(age_purchase_sum6['Count'].sum())))
print("40 to 45 purchase count: " + (str(age_purchase_sum7['Count'].sum())))
print("46 to 50 purchase count: " + (str(age_purchase_sum8['Count'].sum())))
print("6 to 10 purchase count: " + (str(age_purchase_sum9['Count'].sum())))

```

    10 to 14 purchase count: 31.0
    15 to 19 purchase count: 133.0
    20 to 24 purchase count: 336.0
    25 to 29 purchase count: 125.0
    30 to 34 purchase count: 64.0
    35 to 39 purchase count: 42.0
    40 to 45 purchase count: 16.0
    46 to 50 purchase count: 1.0
    6 to 10 purchase count: 32.0



```python
#Total Purchase value
age_purchase_1 = gender_age.loc['10 to 14']
age_purchase_2 = gender_age.loc['15 to 19']
age_purchase_3 = gender_age.loc['20 to 24']
age_purchase_4 = gender_age.loc['25 to 29']
age_purchase_5 = gender_age.loc['30 to 34']
age_purchase_6 = gender_age.loc['35 to 39']
age_purchase_7 = gender_age.loc['40 to 45']
age_purchase_8 = gender_age.loc['46 to 50']
age_purchase_9 = gender_age.loc['6 to 10']
print("10 to 14 purchase price: " + (str(age_purchase_1['Price'].sum())))
print("15 to 19 purchase price: " + (str(age_purchase_2['Price'].sum())))
print("20 to 24 purchase price: " + (str(age_purchase_3['Price'].sum())))
print("25 to 29 purchase price: " + (str(age_purchase_4['Price'].sum())))
print("30 to 34 purchase price: " + (str(age_purchase_5['Price'].sum())))
print("35 to 39 purchase price: " + (str(age_purchase_6['Price'].sum())))
print("40 to 45 purchase price: " + (str(age_purchase_7['Price'].sum())))
print("46 to 50 purchase price: " + (str(age_purchase_8['Price'].sum())))
print("6 to 10 purchase price: " + (str(age_purchase_9['Price'].sum())))
```

    10 to 14 purchase price: 83.78999999999999
    15 to 19 purchase price: 386.4199999999998
    20 to 24 purchase price: 978.7699999999999
    25 to 29 purchase price: 370.3300000000001
    30 to 34 purchase price: 197.24999999999994
    35 to 39 purchase price: 119.39999999999998
    40 to 45 purchase price: 51.029999999999994
    46 to 50 purchase price: 2.7199999999999998
    6 to 10 purchase price: 96.61999999999998



```python
#Average purchase price
age_average_price1 = (age_purchase_1.sum()/31)
age_average_price2 = (age_purchase_2.sum()/133)
age_average_price3 = (age_purchase_3.sum()/336)
age_average_price4 = (age_purchase_4.sum()/125)
age_average_price5 = (age_purchase_5.sum()/64)
age_average_price6 = (age_purchase_6.sum()/42)
age_average_price7 = (age_purchase_7.sum()/16)
age_average_price8 = (age_purchase_8.sum()/1)
age_average_price9 = (age_purchase_9.sum()/31)
print("10 to 14 average purchase: " + (str(age_average_price1)))
print("15 to 19 average purchase: " + (str(age_average_price2)))
print("20 to 24 average purchase: " + (str(age_average_price3)))
print("25 to 29 average purchase: " + (str(age_average_price4)))
print("30 to 34 average purchase: " + (str(age_average_price5)))
print("35 to 39 average purchase: " + (str(age_average_price6)))
print("40 to 45 average purchase: " + (str(age_average_price7)))
print("46 to 50 average purchase: " + (str(age_average_price8)))
print("6 to 10 average purchase: " + (str(age_average_price9)))


```

    10 to 14 average purchase: Price    2.702903
    dtype: float64
    15 to 19 average purchase: Price    2.905414
    dtype: float64
    20 to 24 average purchase: Price    2.913006
    dtype: float64
    25 to 29 average purchase: Price    2.96264
    dtype: float64
    30 to 34 average purchase: Price    3.082031
    dtype: float64
    35 to 39 average purchase: Price    2.842857
    dtype: float64
    40 to 45 average purchase: Price    3.189375
    dtype: float64
    46 to 50 average purchase: Price    2.72
    dtype: float64
    6 to 10 average purchase: Price    3.116774
    dtype: float64



```python
#Normalized purchase price
age_normalized_price1 = (age_purchase_1.sum()/20)
age_normalized_price2 = (age_purchase_2.sum()/100)
age_normalized_price3 = (age_purchase_3.sum()/259)
age_normalized_price4 = (age_purchase_4.sum()/87)
age_normalized_price5 = (age_purchase_5.sum()/47)
age_normalized_price6 = (age_purchase_6.sum()/27)
age_normalized_price7 = (age_purchase_7.sum()/10)
age_normalized_price8 = (age_purchase_8.sum()/1)
age_normalized_price9 = (age_purchase_9.sum()/22)
print("10 to 14 normalized purchase: " + (str(age_normalized_price1)))
print("15 to 19 normalized purchase: " + (str(age_normalized_price2)))
print("20 to 24 normalized purchase: " + (str(age_normalized_price3)))
print("25 to 29 normalized purchase: " + (str(age_normalized_price4)))
print("30 to 34 normalized purchase: " + (str(age_normalized_price5)))
print("35 to 39 normalized purchase: " + (str(age_normalized_price6)))
print("40 to 45 normalized purchase: " + (str(age_normalized_price7)))
print("46 to 50 normalized purchase: " + (str(age_normalized_price8)))
print("6 to 10 normalized purchase: " + (str(age_normalized_price9)))
```

    10 to 14 normalized purchase: Price    4.1895
    dtype: float64
    15 to 19 normalized purchase: Price    3.8642
    dtype: float64
    20 to 24 normalized purchase: Price    3.779035
    dtype: float64
    25 to 29 normalized purchase: Price    4.256667
    dtype: float64
    30 to 34 normalized purchase: Price    4.196809
    dtype: float64
    35 to 39 normalized purchase: Price    4.422222
    dtype: float64
    40 to 45 normalized purchase: Price    5.103
    dtype: float64
    46 to 50 normalized purchase: Price    2.72
    dtype: float64
    6 to 10 normalized purchase: Price    4.391818
    dtype: float64



```python
age_data = pd.DataFrame({'Age group' : ['10 to 14', '15 to 19', '20 to 24', '25 to 29', '30 to 34', '35 to 39', '40 to 45', '46 to 50', '6 to 10'],
                       'Purchase Count' : ['31', '133', '336', '125', '64', '42', '16', '1', '31'],
                       'Average Purchase price' : ['2.70', '2.90', '2.91', '2.96', '3.08', '2.84', '3.18', '2.72', '3.11'],
                       'Total Purchase Value' : ['83.78', '386.41', '978.76', '370.33', '197.24', '119.39', '51.02', '2.71', '96.61'],
                       'Normalized Totals' : ['4.18', '3.86', '3.77', '4.25', '4.19', '4.42', '5.10', '2.72', '4.39']})
age_data = age_data.set_index("Age group")
age_data
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
      <th>Average Purchase price</th>
      <th>Normalized Totals</th>
      <th>Purchase Count</th>
      <th>Total Purchase Value</th>
    </tr>
    <tr>
      <th>Age group</th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>10 to 14</th>
      <td>2.70</td>
      <td>4.18</td>
      <td>31</td>
      <td>83.78</td>
    </tr>
    <tr>
      <th>15 to 19</th>
      <td>2.90</td>
      <td>3.86</td>
      <td>133</td>
      <td>386.41</td>
    </tr>
    <tr>
      <th>20 to 24</th>
      <td>2.91</td>
      <td>3.77</td>
      <td>336</td>
      <td>978.76</td>
    </tr>
    <tr>
      <th>25 to 29</th>
      <td>2.96</td>
      <td>4.25</td>
      <td>125</td>
      <td>370.33</td>
    </tr>
    <tr>
      <th>30 to 34</th>
      <td>3.08</td>
      <td>4.19</td>
      <td>64</td>
      <td>197.24</td>
    </tr>
    <tr>
      <th>35 to 39</th>
      <td>2.84</td>
      <td>4.42</td>
      <td>42</td>
      <td>119.39</td>
    </tr>
    <tr>
      <th>40 to 45</th>
      <td>3.18</td>
      <td>5.10</td>
      <td>16</td>
      <td>51.02</td>
    </tr>
    <tr>
      <th>46 to 50</th>
      <td>2.72</td>
      <td>2.72</td>
      <td>1</td>
      <td>2.71</td>
    </tr>
    <tr>
      <th>6 to 10</th>
      <td>3.11</td>
      <td>4.39</td>
      <td>31</td>
      <td>96.61</td>
    </tr>
  </tbody>
</table>
</div>




```python
sn_data = pymo_raw.groupby(['SN'])
sn_data_count = pd.DataFrame(sn_data['Price'].count())
sn_data_count.reset_index(inplace=True)
sn_data_count.head()
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
      <th>SN</th>
      <th>Price</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>Adairialis76</td>
      <td>1</td>
    </tr>
    <tr>
      <th>1</th>
      <td>Aduephos78</td>
      <td>3</td>
    </tr>
    <tr>
      <th>2</th>
      <td>Aeduera68</td>
      <td>3</td>
    </tr>
    <tr>
      <th>3</th>
      <td>Aela49</td>
      <td>1</td>
    </tr>
    <tr>
      <th>4</th>
      <td>Aela59</td>
      <td>1</td>
    </tr>
  </tbody>
</table>
</div>




```python
sn_data = pymo_raw.groupby(['SN'])
sn_data1 = pd.DataFrame(sn_data['Price'].sum())
sn_data1.reset_index(inplace=True)
sn_data1.head()
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
      <th>SN</th>
      <th>Price</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>Adairialis76</td>
      <td>2.46</td>
    </tr>
    <tr>
      <th>1</th>
      <td>Aduephos78</td>
      <td>6.70</td>
    </tr>
    <tr>
      <th>2</th>
      <td>Aeduera68</td>
      <td>5.80</td>
    </tr>
    <tr>
      <th>3</th>
      <td>Aela49</td>
      <td>2.46</td>
    </tr>
    <tr>
      <th>4</th>
      <td>Aela59</td>
      <td>1.27</td>
    </tr>
  </tbody>
</table>
</div>




```python
merge_data = pd.merge(sn_data1, sn_data_count, on='SN')
merge_data = merge_data.sort_values(['Price_x'], ascending=False)
merge_data['Average Purchase Price'] = sn_data1["Price"]/sn_data_count["Price"]
merge_data = merge_data.rename(columns={"Price_x":"Total Purchase Value", "Price_y": "Purchase Count"})
merge_data = merge_data.set_index("SN")
merge_data.head()
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
      <th>Total Purchase Value</th>
      <th>Purchase Count</th>
      <th>Average Purchase Price</th>
    </tr>
    <tr>
      <th>SN</th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>Undirrala66</th>
      <td>17.06</td>
      <td>5</td>
      <td>3.412000</td>
    </tr>
    <tr>
      <th>Saedue76</th>
      <td>13.56</td>
      <td>4</td>
      <td>3.390000</td>
    </tr>
    <tr>
      <th>Mindimnya67</th>
      <td>12.74</td>
      <td>4</td>
      <td>3.185000</td>
    </tr>
    <tr>
      <th>Haellysu29</th>
      <td>12.73</td>
      <td>3</td>
      <td>4.243333</td>
    </tr>
    <tr>
      <th>Eoda93</th>
      <td>11.58</td>
      <td>3</td>
      <td>3.860000</td>
    </tr>
  </tbody>
</table>
</div>




```python
price_data = pymo_raw.groupby(['Item Name'])
price_data_count = pd.DataFrame(price_data['Price'].count())
price_data_count.reset_index(inplace=True)
#price_data_count = price_data_count.sort_values(['Price'], ascending=False)
price_data_count.head()
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
      <th>Item Name</th>
      <th>Price</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>Abyssal Shard</td>
      <td>3</td>
    </tr>
    <tr>
      <th>1</th>
      <td>Aetherius, Boon of the Blessed</td>
      <td>4</td>
    </tr>
    <tr>
      <th>2</th>
      <td>Agatha</td>
      <td>5</td>
    </tr>
    <tr>
      <th>3</th>
      <td>Alpha</td>
      <td>7</td>
    </tr>
    <tr>
      <th>4</th>
      <td>Alpha, Oath of Zeal</td>
      <td>7</td>
    </tr>
  </tbody>
</table>
</div>




```python
price_data1 = pd.DataFrame(pymo_raw[['Item Name', 'Item ID','Price']])
#price_data2 = price_data1.groupby(['Item Name'])
price_data1.head()
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
      <th>Item Name</th>
      <th>Item ID</th>
      <th>Price</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>Bone Crushing Silver Skewer</td>
      <td>165</td>
      <td>3.37</td>
    </tr>
    <tr>
      <th>1</th>
      <td>Stormbringer, Dark Blade of Ending Misery</td>
      <td>119</td>
      <td>2.32</td>
    </tr>
    <tr>
      <th>2</th>
      <td>Primitive Blade</td>
      <td>174</td>
      <td>2.46</td>
    </tr>
    <tr>
      <th>3</th>
      <td>Final Critic</td>
      <td>92</td>
      <td>1.36</td>
    </tr>
    <tr>
      <th>4</th>
      <td>Stormfury Mace</td>
      <td>63</td>
      <td>1.27</td>
    </tr>
  </tbody>
</table>
</div>




```python
#Most profitable item
merge_data1 = pd.merge(price_data1, price_data_count, on='Item Name', how ='left')
merge_data1['Total Purchase Price'] = price_data1["Price"]*price_data_count["Price"]
merge_data1 = merge_data1.rename(columns={"Price_x":"Item Price", "Price_y": "Purchase Count"})
merge_data1 = merge_data1.sort_values(['Total Purchase Price'], ascending=False)
merge_data1 = merge_data1.set_index("Item ID")
merge_data1.head()
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
      <th>Item Name</th>
      <th>Item Price</th>
      <th>Purchase Count</th>
      <th>Total Purchase Price</th>
    </tr>
    <tr>
      <th>Item ID</th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>57</th>
      <td>Despair, Favor of Due Diligence</td>
      <td>3.81</td>
      <td>5</td>
      <td>34.29</td>
    </tr>
    <tr>
      <th>49</th>
      <td>The Oculus, Token of Lost Worlds</td>
      <td>4.23</td>
      <td>5</td>
      <td>33.84</td>
    </tr>
    <tr>
      <th>105</th>
      <td>Hailstorm Shadowsteel Scythe</td>
      <td>3.73</td>
      <td>3</td>
      <td>33.57</td>
    </tr>
    <tr>
      <th>101</th>
      <td>Final Critic</td>
      <td>4.62</td>
      <td>14</td>
      <td>32.34</td>
    </tr>
    <tr>
      <th>118</th>
      <td>Ghost Reaver, Longsword of Magic</td>
      <td>2.77</td>
      <td>3</td>
      <td>30.47</td>
    </tr>
  </tbody>
</table>
</div>




```python
popular_data = pymo_raw.groupby(['Item Name'])
popular_data_count = pd.DataFrame(popular_data['Price'].count())
popular_data_count.reset_index(inplace=True)
popular_data_count.head()
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
      <th>Item Name</th>
      <th>Price</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>Abyssal Shard</td>
      <td>3</td>
    </tr>
    <tr>
      <th>1</th>
      <td>Aetherius, Boon of the Blessed</td>
      <td>4</td>
    </tr>
    <tr>
      <th>2</th>
      <td>Agatha</td>
      <td>5</td>
    </tr>
    <tr>
      <th>3</th>
      <td>Alpha</td>
      <td>7</td>
    </tr>
    <tr>
      <th>4</th>
      <td>Alpha, Oath of Zeal</td>
      <td>7</td>
    </tr>
  </tbody>
</table>
</div>




```python
popular_data = pymo_raw.groupby(['Item Name'])
popular_data1 = pd.DataFrame(popular_data['Price'].sum())
popular_data1.reset_index(inplace=True)
popular_data1.head()
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
      <th>Item Name</th>
      <th>Price</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>Abyssal Shard</td>
      <td>6.12</td>
    </tr>
    <tr>
      <th>1</th>
      <td>Aetherius, Boon of the Blessed</td>
      <td>19.00</td>
    </tr>
    <tr>
      <th>2</th>
      <td>Agatha</td>
      <td>9.55</td>
    </tr>
    <tr>
      <th>3</th>
      <td>Alpha</td>
      <td>10.92</td>
    </tr>
    <tr>
      <th>4</th>
      <td>Alpha, Oath of Zeal</td>
      <td>20.16</td>
    </tr>
  </tbody>
</table>
</div>




```python
merge_data2 = pd.merge(popular_data1, popular_data_count, on='Item Name')
merge_data2['Item Price'] = popular_data1["Price"]/popular_data_count["Price"]
merge_data2 = merge_data2.sort_values(['Price_y'], ascending=False)
merge_data2 = merge_data2.rename(columns={"Price_y": "Purchase Count", "Item Price": "Item Price Value", "Price_x":"Total Value"})
merge_data2 = merge_data2.set_index("Item Name")
merge_data2.head()
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
      <th>Total Value</th>
      <th>Purchase Count</th>
      <th>Item Price Value</th>
    </tr>
    <tr>
      <th>Item Name</th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>Final Critic</th>
      <td>38.60</td>
      <td>14</td>
      <td>2.757143</td>
    </tr>
    <tr>
      <th>Arcane Gem</th>
      <td>24.53</td>
      <td>11</td>
      <td>2.230000</td>
    </tr>
    <tr>
      <th>Betrayal, Whisper of Grieving Widows</th>
      <td>25.85</td>
      <td>11</td>
      <td>2.350000</td>
    </tr>
    <tr>
      <th>Stormcaller</th>
      <td>34.65</td>
      <td>10</td>
      <td>3.465000</td>
    </tr>
    <tr>
      <th>Woeful Adamantite Claymore</th>
      <td>11.16</td>
      <td>9</td>
      <td>1.240000</td>
    </tr>
  </tbody>
</table>
</div>




```python

```
