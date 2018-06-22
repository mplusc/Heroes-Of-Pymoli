

```python
import pandas as pd
import numpy as np
```


```python
file = "HeroesOfPymoli/purchase_data.json"

df = pd.read_json(file, orient="records")
df.head()


```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
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
df.info()
```

    <class 'pandas.core.frame.DataFrame'>
    RangeIndex: 780 entries, 0 to 779
    Data columns (total 6 columns):
    Age          780 non-null int64
    Gender       780 non-null object
    Item ID      780 non-null int64
    Item Name    780 non-null object
    Price        780 non-null float64
    SN           780 non-null object
    dtypes: float64(1), int64(2), object(3)
    memory usage: 36.6+ KB
    


```python
df.columns
```




    Index(['Age', 'Gender', 'Item ID', 'Item Name', 'Price', 'SN'], dtype='object')




```python
total_players = df.loc[:,["Gender","SN","Age"]]
total_players = total_players.drop_duplicates()
tp = total_players.count()[0]



print("There are " + str(tp) + " total players in Heroes of Pymoli.")
```

    There are 573 total players in Heroes of Pymoli.
    


```python
unique_items = df["Item Name"].unique()

print("There are a total of " + str(len(unique_items)) + " unique items in Heroes of Pymoli.")
```

    There are a total of 179 unique items in Heroes of Pymoli.
    


```python
average_purchase = df["Price"].mean()
avg = round(average_purchase,2)

print("The average purchase price of an item in Heroes of Pymoli was $" + str(avg))
```

    The average purchase price of an item in Heroes of Pymoli was $2.93
    


```python
total_purchases = df["Price"].count()

print("The total number of purchases in Heroes of Pymoli was " + str(total_purchases) + ".")
```

    The total number of purchases in Heroes of Pymoli was 780.
    


```python
revenue = df["Price"].sum()

print("The total revenue of purchases in Heroes of Pymoli was $" + str(revenue) + ".")
```

    The total revenue of purchases in Heroes of Pymoli was $2286.33.
    


```python
total_players["Gender"].value_counts()

```




    Male                     465
    Female                   100
    Other / Non-Disclosed      8
    Name: Gender, dtype: int64




```python
gender_info = total_players["Gender"].value_counts()
gender_percents = gender_info / tp *100

gender = pd.DataFrame({"Total Count": gender_info, "Percentage of Players": gender_percents})
                                
gender = gender.round(2)
gender
                
                
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Percentage of Players</th>
      <th>Total Count</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>Male</th>
      <td>81.15</td>
      <td>465</td>
    </tr>
    <tr>
      <th>Female</th>
      <td>17.45</td>
      <td>100</td>
    </tr>
    <tr>
      <th>Other / Non-Disclosed</th>
      <td>1.40</td>
      <td>8</td>
    </tr>
  </tbody>
</table>
</div>




```python
avg_gender_purchase = df.groupby("Gender")

gender_info_v2 =df["Gender"].value_counts()

avg_g = avg_gender_purchase["Price"].mean()
avg_g = avg_g.round(2)

total_gender_p = avg_gender_purchase["Price"].sum()
total_gender_p = total_gender_p.round(2)

normalized_t = total_gender_p / gender["Total Count"]
normalized_t = normalized_t.round(2)

gender_purchase = pd.DataFrame({"Purchase Count": gender_info_v2,"Average Purchase Price": avg_g, 
                                "Total Purchase Price":total_gender_p,"Normalized Total":normalized_t})

gender_purchase
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Average Purchase Price</th>
      <th>Normalized Total</th>
      <th>Purchase Count</th>
      <th>Total Purchase Price</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>Female</th>
      <td>2.82</td>
      <td>3.83</td>
      <td>136</td>
      <td>382.91</td>
    </tr>
    <tr>
      <th>Male</th>
      <td>2.95</td>
      <td>4.02</td>
      <td>633</td>
      <td>1867.68</td>
    </tr>
    <tr>
      <th>Other / Non-Disclosed</th>
      <td>3.25</td>
      <td>4.47</td>
      <td>11</td>
      <td>35.74</td>
    </tr>
  </tbody>
</table>
</div>




```python
print(df["Age"].min())
print(df["Age"].max())

bins = [0,10,15,20,25,30,35,40,100]

group_labels = ["<10","10-14","15-19","20-24","25-29","30-34","35-39","40+"]

total_players["Age Range"] = pd.cut(df["Age"],bins, labels=group_labels)

age_totals = total_players["Age Range"].value_counts()
age_percents = age_totals / tp *100
age = pd.DataFrame({"Total Counts":age_totals,"Percentage of Players":age_percents})
age = age.round(2)

age.sort_index()
```

    7
    45
    




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Percentage of Players</th>
      <th>Total Counts</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>&lt;10</th>
      <td>3.84</td>
      <td>22</td>
    </tr>
    <tr>
      <th>10-14</th>
      <td>9.42</td>
      <td>54</td>
    </tr>
    <tr>
      <th>15-19</th>
      <td>24.26</td>
      <td>139</td>
    </tr>
    <tr>
      <th>20-24</th>
      <td>40.84</td>
      <td>234</td>
    </tr>
    <tr>
      <th>25-29</th>
      <td>9.08</td>
      <td>52</td>
    </tr>
    <tr>
      <th>30-34</th>
      <td>7.68</td>
      <td>44</td>
    </tr>
    <tr>
      <th>35-39</th>
      <td>4.36</td>
      <td>25</td>
    </tr>
    <tr>
      <th>40+</th>
      <td>0.52</td>
      <td>3</td>
    </tr>
  </tbody>
</table>
</div>




```python
df["Age Range"] = pd.cut(df["Age"],bins,labels=group_labels)

age_total_purchase = df.groupby(["Age Range"]).sum()["Price"]
age_average = df.groupby(["Age Range"]).mean()["Price"]
age_average = age_average.round(2)
age_count = df.groupby(["Age Range"]).count()["Price"]

normalized_total = age_total_purchase / age["Total Counts"]
normalized_total = normalized_total.round(2)

age_range_data = pd.DataFrame({"Purchase Count": age_count, "Average Purchase Price": age_average, 
                               "Total Purchase Value": age_total_purchase, "Normalized Totals": normalized_total})
age_range_data.sort_index()
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Average Purchase Price</th>
      <th>Normalized Totals</th>
      <th>Purchase Count</th>
      <th>Total Purchase Value</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>10-14</th>
      <td>2.87</td>
      <td>4.15</td>
      <td>78</td>
      <td>224.15</td>
    </tr>
    <tr>
      <th>15-19</th>
      <td>2.87</td>
      <td>3.80</td>
      <td>184</td>
      <td>528.74</td>
    </tr>
    <tr>
      <th>20-24</th>
      <td>2.96</td>
      <td>3.86</td>
      <td>305</td>
      <td>902.61</td>
    </tr>
    <tr>
      <th>25-29</th>
      <td>2.89</td>
      <td>4.23</td>
      <td>76</td>
      <td>219.82</td>
    </tr>
    <tr>
      <th>30-34</th>
      <td>3.07</td>
      <td>4.05</td>
      <td>58</td>
      <td>178.26</td>
    </tr>
    <tr>
      <th>35-39</th>
      <td>2.90</td>
      <td>5.10</td>
      <td>44</td>
      <td>127.49</td>
    </tr>
    <tr>
      <th>40+</th>
      <td>2.88</td>
      <td>2.88</td>
      <td>3</td>
      <td>8.64</td>
    </tr>
    <tr>
      <th>&lt;10</th>
      <td>3.02</td>
      <td>4.39</td>
      <td>32</td>
      <td>96.62</td>
    </tr>
  </tbody>
</table>
</div>




```python
purchase_count = df.groupby(["SN"]).count()["Price"]
purchase_average = df.groupby(["SN"]).mean()["Price"]
purchase_average = purchase_average.round(2)
purchase_value = df.groupby(["SN"]).sum()["Price"]


spender_data = pd.DataFrame({"Purchase Count": purchase_count, "Average Purchase Price": purchase_average, 
                               "Total Purchase Value": purchase_value})

```


```python
spender_data.sort_values("Total Purchase Value", ascending = False).head(5)
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Average Purchase Price</th>
      <th>Purchase Count</th>
      <th>Total Purchase Value</th>
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
      <td>3.41</td>
      <td>5</td>
      <td>17.06</td>
    </tr>
    <tr>
      <th>Saedue76</th>
      <td>3.39</td>
      <td>4</td>
      <td>13.56</td>
    </tr>
    <tr>
      <th>Mindimnya67</th>
      <td>3.18</td>
      <td>4</td>
      <td>12.74</td>
    </tr>
    <tr>
      <th>Haellysu29</th>
      <td>4.24</td>
      <td>3</td>
      <td>12.73</td>
    </tr>
    <tr>
      <th>Eoda93</th>
      <td>3.86</td>
      <td>3</td>
      <td>11.58</td>
    </tr>
  </tbody>
</table>
</div>




```python
item_data = df.loc[:,["Item ID","Item Name","Price"]]

total_item_purchase = item_data.groupby(["Item ID","Item Name"]).sum()["Price"]
average_item = item_data.groupby(["Item ID","Item Name"]).mean()["Price"]
item = item_data.groupby(["Item ID","Item Name"]).count()["Price"]

items = pd.DataFrame({"Purchase Count" : item,"Item Price" : average_item,"Total Purchase Value" : total_item_purchase})

```


```python
items.sort_values("Purchase Count", ascending=False).head(5)
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th></th>
      <th>Item Price</th>
      <th>Purchase Count</th>
      <th>Total Purchase Value</th>
    </tr>
    <tr>
      <th>Item ID</th>
      <th>Item Name</th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>39</th>
      <th>Betrayal, Whisper of Grieving Widows</th>
      <td>2.35</td>
      <td>11</td>
      <td>25.85</td>
    </tr>
    <tr>
      <th>84</th>
      <th>Arcane Gem</th>
      <td>2.23</td>
      <td>11</td>
      <td>24.53</td>
    </tr>
    <tr>
      <th>31</th>
      <th>Trickster</th>
      <td>2.07</td>
      <td>9</td>
      <td>18.63</td>
    </tr>
    <tr>
      <th>175</th>
      <th>Woeful Adamantite Claymore</th>
      <td>1.24</td>
      <td>9</td>
      <td>11.16</td>
    </tr>
    <tr>
      <th>13</th>
      <th>Serenity</th>
      <td>1.49</td>
      <td>9</td>
      <td>13.41</td>
    </tr>
  </tbody>
</table>
</div>




```python
items.sort_values("Total Purchase Value", ascending=False).head(5)
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th></th>
      <th>Item Price</th>
      <th>Purchase Count</th>
      <th>Total Purchase Value</th>
    </tr>
    <tr>
      <th>Item ID</th>
      <th>Item Name</th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>34</th>
      <th>Retribution Axe</th>
      <td>4.14</td>
      <td>9</td>
      <td>37.26</td>
    </tr>
    <tr>
      <th>115</th>
      <th>Spectral Diamond Doomblade</th>
      <td>4.25</td>
      <td>7</td>
      <td>29.75</td>
    </tr>
    <tr>
      <th>32</th>
      <th>Orenmir</th>
      <td>4.95</td>
      <td>6</td>
      <td>29.70</td>
    </tr>
    <tr>
      <th>103</th>
      <th>Singed Scalpel</th>
      <td>4.87</td>
      <td>6</td>
      <td>29.22</td>
    </tr>
    <tr>
      <th>107</th>
      <th>Splitter, Foe Of Subtlety</th>
      <td>3.61</td>
      <td>8</td>
      <td>28.88</td>
    </tr>
  </tbody>
</table>
</div>


