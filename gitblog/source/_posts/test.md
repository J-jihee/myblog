```python
import pandas as pd
import numpy as np
import matplotlib.font_manager as fm
import matplotlib.pyplot as plt
font_path = 'C:/Windows/Fonts/NGULIM.TTF'
fontprop = fm.FontProperties(fname=font_path, size=15)
font_family = fm.FontProperties(fname=font_path).get_name()

plt.rcParams["font.family"] = font_family
```


```python
name_2008 = pd.read_csv('Kbaby_data/y2008.txt',names=['name','gender','births'])
name_2008
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
      <th>name</th>
      <th>gender</th>
      <th>births</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>서연</td>
      <td>F</td>
      <td>3280</td>
    </tr>
    <tr>
      <th>1</th>
      <td>민서</td>
      <td>F</td>
      <td>2873</td>
    </tr>
    <tr>
      <th>2</th>
      <td>지민</td>
      <td>F</td>
      <td>2826</td>
    </tr>
    <tr>
      <th>3</th>
      <td>서현</td>
      <td>F</td>
      <td>2606</td>
    </tr>
    <tr>
      <th>4</th>
      <td>서윤</td>
      <td>F</td>
      <td>2484</td>
    </tr>
    <tr>
      <th>...</th>
      <td>...</td>
      <td>...</td>
      <td>...</td>
    </tr>
    <tr>
      <th>995</th>
      <td>은율</td>
      <td>M</td>
      <td>91</td>
    </tr>
    <tr>
      <th>996</th>
      <td>재연</td>
      <td>M</td>
      <td>91</td>
    </tr>
    <tr>
      <th>997</th>
      <td>건민</td>
      <td>M</td>
      <td>91</td>
    </tr>
    <tr>
      <th>998</th>
      <td>민상</td>
      <td>M</td>
      <td>90</td>
    </tr>
    <tr>
      <th>999</th>
      <td>태진</td>
      <td>M</td>
      <td>90</td>
    </tr>
  </tbody>
</table>
<p>1000 rows × 3 columns</p>
</div>




```python
name_2008.groupby('gender').births.sum()
```




    gender
    F    191282
    M    169063
    Name: births, dtype: int64




```python
years = range(2008,2021)
pieces = [] #전체 연도의 리스트를 합칠 것
columns = ['name','gender','births']

for year in years:
    path = 'Kbaby_data/y{}.txt'.format(year)
    frame = pd.read_csv(path,names=columns)
    frame['year']=year
    pieces.append(frame)

names = pd.concat(pieces,ignore_index=True)
names
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
      <th>name</th>
      <th>gender</th>
      <th>births</th>
      <th>year</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>서연</td>
      <td>F</td>
      <td>3280</td>
      <td>2008</td>
    </tr>
    <tr>
      <th>1</th>
      <td>민서</td>
      <td>F</td>
      <td>2873</td>
      <td>2008</td>
    </tr>
    <tr>
      <th>2</th>
      <td>지민</td>
      <td>F</td>
      <td>2826</td>
      <td>2008</td>
    </tr>
    <tr>
      <th>3</th>
      <td>서현</td>
      <td>F</td>
      <td>2606</td>
      <td>2008</td>
    </tr>
    <tr>
      <th>4</th>
      <td>서윤</td>
      <td>F</td>
      <td>2484</td>
      <td>2008</td>
    </tr>
    <tr>
      <th>...</th>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
    </tr>
    <tr>
      <th>12995</th>
      <td>민승</td>
      <td>M</td>
      <td>20</td>
      <td>2020</td>
    </tr>
    <tr>
      <th>12996</th>
      <td>규담</td>
      <td>M</td>
      <td>20</td>
      <td>2020</td>
    </tr>
    <tr>
      <th>12997</th>
      <td>영웅</td>
      <td>M</td>
      <td>20</td>
      <td>2020</td>
    </tr>
    <tr>
      <th>12998</th>
      <td>재성</td>
      <td>M</td>
      <td>20</td>
      <td>2020</td>
    </tr>
    <tr>
      <th>12999</th>
      <td>주빈</td>
      <td>M</td>
      <td>20</td>
      <td>2020</td>
    </tr>
  </tbody>
</table>
<p>13000 rows × 4 columns</p>
</div>




```python
total_births = names.pivot_table('births', index='year',columns='gender',aggfunc=sum)
total_births.plot(title='total births(gender/year)')
```




    <matplotlib.axes._subplots.AxesSubplot at 0x2c52816afd0>




![svg](/img/output_4_1.svg)



```python
def add_prop(group):
    group['prop'] = group.births / group.births.sum()
    return group
names = names.groupby(['year','gender']).apply(add_prop)
names
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
      <th>name</th>
      <th>gender</th>
      <th>births</th>
      <th>year</th>
      <th>prop</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>서연</td>
      <td>F</td>
      <td>3280</td>
      <td>2008</td>
      <td>0.017147</td>
    </tr>
    <tr>
      <th>1</th>
      <td>민서</td>
      <td>F</td>
      <td>2873</td>
      <td>2008</td>
      <td>0.015020</td>
    </tr>
    <tr>
      <th>2</th>
      <td>지민</td>
      <td>F</td>
      <td>2826</td>
      <td>2008</td>
      <td>0.014774</td>
    </tr>
    <tr>
      <th>3</th>
      <td>서현</td>
      <td>F</td>
      <td>2606</td>
      <td>2008</td>
      <td>0.013624</td>
    </tr>
    <tr>
      <th>4</th>
      <td>서윤</td>
      <td>F</td>
      <td>2484</td>
      <td>2008</td>
      <td>0.012986</td>
    </tr>
    <tr>
      <th>...</th>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
    </tr>
    <tr>
      <th>12995</th>
      <td>민승</td>
      <td>M</td>
      <td>20</td>
      <td>2020</td>
      <td>0.000398</td>
    </tr>
    <tr>
      <th>12996</th>
      <td>규담</td>
      <td>M</td>
      <td>20</td>
      <td>2020</td>
      <td>0.000398</td>
    </tr>
    <tr>
      <th>12997</th>
      <td>영웅</td>
      <td>M</td>
      <td>20</td>
      <td>2020</td>
      <td>0.000398</td>
    </tr>
    <tr>
      <th>12998</th>
      <td>재성</td>
      <td>M</td>
      <td>20</td>
      <td>2020</td>
      <td>0.000398</td>
    </tr>
    <tr>
      <th>12999</th>
      <td>주빈</td>
      <td>M</td>
      <td>20</td>
      <td>2020</td>
      <td>0.000398</td>
    </tr>
  </tbody>
</table>
<p>13000 rows × 5 columns</p>
</div>




```python
names.groupby(['year','gender']).prop.sum()
```




    year  gender
    2008  F         1.0
          M         1.0
    2009  F         1.0
          M         1.0
    2010  F         1.0
          M         1.0
    2011  F         1.0
          M         1.0
    2012  F         1.0
          M         1.0
    2013  F         1.0
          M         1.0
    2014  F         1.0
          M         1.0
    2015  F         1.0
          M         1.0
    2016  F         1.0
          M         1.0
    2017  F         1.0
          M         1.0
    2018  F         1.0
          M         1.0
    2019  F         1.0
          M         1.0
    2020  F         1.0
          M         1.0
    Name: prop, dtype: float64



### 연도별/성별에 따른 선호하는 이름 1000개 추출


```python
def get_top1000(group):
    return group.sort_values(by='births',ascending=False)[:1000]
grouped = names.groupby(['year','gender'])
top1000 = grouped.apply(get_top1000)
top1000
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
      <th></th>
      <th>name</th>
      <th>gender</th>
      <th>births</th>
      <th>year</th>
      <th>prop</th>
    </tr>
    <tr>
      <th>year</th>
      <th>gender</th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th rowspan="5" valign="top">2008</th>
      <th rowspan="5" valign="top">F</th>
      <th>0</th>
      <td>서연</td>
      <td>F</td>
      <td>3280</td>
      <td>2008</td>
      <td>0.017147</td>
    </tr>
    <tr>
      <th>1</th>
      <td>민서</td>
      <td>F</td>
      <td>2873</td>
      <td>2008</td>
      <td>0.015020</td>
    </tr>
    <tr>
      <th>2</th>
      <td>지민</td>
      <td>F</td>
      <td>2826</td>
      <td>2008</td>
      <td>0.014774</td>
    </tr>
    <tr>
      <th>3</th>
      <td>서현</td>
      <td>F</td>
      <td>2606</td>
      <td>2008</td>
      <td>0.013624</td>
    </tr>
    <tr>
      <th>4</th>
      <td>서윤</td>
      <td>F</td>
      <td>2484</td>
      <td>2008</td>
      <td>0.012986</td>
    </tr>
    <tr>
      <th>...</th>
      <th>...</th>
      <th>...</th>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
    </tr>
    <tr>
      <th rowspan="5" valign="top">2020</th>
      <th rowspan="5" valign="top">M</th>
      <th>12991</th>
      <td>이재</td>
      <td>M</td>
      <td>20</td>
      <td>2020</td>
      <td>0.000398</td>
    </tr>
    <tr>
      <th>12989</th>
      <td>다민</td>
      <td>M</td>
      <td>20</td>
      <td>2020</td>
      <td>0.000398</td>
    </tr>
    <tr>
      <th>12988</th>
      <td>수인</td>
      <td>M</td>
      <td>20</td>
      <td>2020</td>
      <td>0.000398</td>
    </tr>
    <tr>
      <th>12987</th>
      <td>민영</td>
      <td>M</td>
      <td>20</td>
      <td>2020</td>
      <td>0.000398</td>
    </tr>
    <tr>
      <th>12999</th>
      <td>주빈</td>
      <td>M</td>
      <td>20</td>
      <td>2020</td>
      <td>0.000398</td>
    </tr>
  </tbody>
</table>
<p>13000 rows × 5 columns</p>
</div>




```python
top1000.reset_index(inplace = True , drop = True)
top1000
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
      <th>name</th>
      <th>gender</th>
      <th>births</th>
      <th>year</th>
      <th>prop</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>서연</td>
      <td>F</td>
      <td>3280</td>
      <td>2008</td>
      <td>0.017147</td>
    </tr>
    <tr>
      <th>1</th>
      <td>민서</td>
      <td>F</td>
      <td>2873</td>
      <td>2008</td>
      <td>0.015020</td>
    </tr>
    <tr>
      <th>2</th>
      <td>지민</td>
      <td>F</td>
      <td>2826</td>
      <td>2008</td>
      <td>0.014774</td>
    </tr>
    <tr>
      <th>3</th>
      <td>서현</td>
      <td>F</td>
      <td>2606</td>
      <td>2008</td>
      <td>0.013624</td>
    </tr>
    <tr>
      <th>4</th>
      <td>서윤</td>
      <td>F</td>
      <td>2484</td>
      <td>2008</td>
      <td>0.012986</td>
    </tr>
    <tr>
      <th>...</th>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
    </tr>
    <tr>
      <th>12995</th>
      <td>이재</td>
      <td>M</td>
      <td>20</td>
      <td>2020</td>
      <td>0.000398</td>
    </tr>
    <tr>
      <th>12996</th>
      <td>다민</td>
      <td>M</td>
      <td>20</td>
      <td>2020</td>
      <td>0.000398</td>
    </tr>
    <tr>
      <th>12997</th>
      <td>수인</td>
      <td>M</td>
      <td>20</td>
      <td>2020</td>
      <td>0.000398</td>
    </tr>
    <tr>
      <th>12998</th>
      <td>민영</td>
      <td>M</td>
      <td>20</td>
      <td>2020</td>
      <td>0.000398</td>
    </tr>
    <tr>
      <th>12999</th>
      <td>주빈</td>
      <td>M</td>
      <td>20</td>
      <td>2020</td>
      <td>0.000398</td>
    </tr>
  </tbody>
</table>
<p>13000 rows × 5 columns</p>
</div>



### 상위 1000개의 이름데이터를 남자(boys)와 여자(girls)로 분리


```python
boys = top1000[top1000.gender == 'M']
girls = top1000[top1000.gender == 'F']
```

### 연도와 출생수를 피봇테이블로 변환


```python
total_births = top1000.pivot_table('births',index ='year',columns='name',aggfunc=sum)
total_births.info()
```

    <class 'pandas.core.frame.DataFrame'>
    Int64Index: 13 entries, 2008 to 2020
    Columns: 1244 entries, 가람 to 희찬
    dtypes: float64(1244)
    memory usage: 126.4 KB
    


```python
subset = total_births[['지후','지희','민석','민호']]
subset.plot(subplots=True,figsize=(12,10),grid=False,title='Number of birth per year')
```




    array([<matplotlib.axes._subplots.AxesSubplot object at 0x000002C53443A880>,
           <matplotlib.axes._subplots.AxesSubplot object at 0x000002C5333875E0>,
           <matplotlib.axes._subplots.AxesSubplot object at 0x000002C5333A4820>,
           <matplotlib.axes._subplots.AxesSubplot object at 0x000002C5333CE9A0>],
          dtype=object)




![svg](/img/output_14_1.svg)



```python
import matplotlib.pyplot as plt
plt.figure()
table = top1000.pivot_table('prop', index = 'year',columns='gender',aggfunc=sum)

table.plot(title='Sum of table1000.prop by year and sex',
           yticks=np.linspace(0,1,1), xticks=range(2008, 2021, 10))

```




    <matplotlib.axes._subplots.AxesSubplot at 0x2c5344e80d0>




    <Figure size 432x288 with 0 Axes>



![svg](/img/output_15_2.svg)



```python
df = boys[boys.year == 2008]
df
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
      <th>name</th>
      <th>gender</th>
      <th>births</th>
      <th>year</th>
      <th>prop</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>500</th>
      <td>민준</td>
      <td>M</td>
      <td>2642</td>
      <td>2008</td>
      <td>0.015627</td>
    </tr>
    <tr>
      <th>501</th>
      <td>지훈</td>
      <td>M</td>
      <td>2154</td>
      <td>2008</td>
      <td>0.012741</td>
    </tr>
    <tr>
      <th>502</th>
      <td>현우</td>
      <td>M</td>
      <td>1924</td>
      <td>2008</td>
      <td>0.011380</td>
    </tr>
    <tr>
      <th>503</th>
      <td>준서</td>
      <td>M</td>
      <td>1885</td>
      <td>2008</td>
      <td>0.011150</td>
    </tr>
    <tr>
      <th>504</th>
      <td>우진</td>
      <td>M</td>
      <td>1815</td>
      <td>2008</td>
      <td>0.010736</td>
    </tr>
    <tr>
      <th>...</th>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
    </tr>
    <tr>
      <th>995</th>
      <td>은율</td>
      <td>M</td>
      <td>91</td>
      <td>2008</td>
      <td>0.000538</td>
    </tr>
    <tr>
      <th>996</th>
      <td>재연</td>
      <td>M</td>
      <td>91</td>
      <td>2008</td>
      <td>0.000538</td>
    </tr>
    <tr>
      <th>997</th>
      <td>건민</td>
      <td>M</td>
      <td>91</td>
      <td>2008</td>
      <td>0.000538</td>
    </tr>
    <tr>
      <th>998</th>
      <td>민상</td>
      <td>M</td>
      <td>90</td>
      <td>2008</td>
      <td>0.000532</td>
    </tr>
    <tr>
      <th>999</th>
      <td>태진</td>
      <td>M</td>
      <td>90</td>
      <td>2008</td>
      <td>0.000532</td>
    </tr>
  </tbody>
</table>
<p>500 rows × 5 columns</p>
</div>




```python
prop_cumsum = df.sort_values(by='prop',ascending=False).prop.cumsum()
print(prop_cumsum[:10])
prop_cumsum.values.searchsorted(0.5)+1
```

    500    0.015627
    501    0.028368
    502    0.039748
    503    0.050898
    504    0.061634
    505    0.071707
    506    0.081757
    507    0.091309
    508    0.100673
    509    0.109935
    Name: prop, dtype: float64
    




    90




```python
df = boys[boys.year == 2018]
y2018 = df.sort_values(by='prop',ascending=False).prop.cumsum()
y2018.values.searchsorted(0.5)+1
```




    69




```python
def get_quantile_count(group, q=0.5):
    group = group.sort_values(by='prop', ascending=False)
    return group.prop.cumsum().values.searchsorted(q) + 1

diversity = top1000.groupby(['year', 'gender']).apply(get_quantile_count)
diversity = diversity.unstack('gender')
fig = plt.figure()
diversity.plot(title="Number of popular names in top 50%")
```




    <matplotlib.axes._subplots.AxesSubplot at 0x2c529ec57c0>




    <Figure size 432x288 with 0 Axes>



![svg](/img/output_19_2.svg)


### 마지막 글자의 변화


```python
get_last_letter = lambda x: x[-1]
last_letters = names.name.map(get_last_letter)
last_letters.name = 'last_letter'

table = names.pivot_table('births', index=last_letters,columns=['gender','year'],aggfunc=sum)
```


```python
subtable = table.reindex(columns=[2008,2012,2018],level='year')
subtable
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead tr th {
        text-align: left;
    }

    .dataframe thead tr:last-of-type th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr>
      <th>gender</th>
      <th colspan="3" halign="left">F</th>
      <th colspan="3" halign="left">M</th>
    </tr>
    <tr>
      <th>year</th>
      <th>2008</th>
      <th>2012</th>
      <th>2018</th>
      <th>2008</th>
      <th>2012</th>
      <th>2018</th>
    </tr>
    <tr>
      <th>last_letter</th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>강</th>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>106.0</td>
      <td>111.0</td>
      <td>74.0</td>
    </tr>
    <tr>
      <th>건</th>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>1755.0</td>
      <td>2410.0</td>
      <td>1580.0</td>
    </tr>
    <tr>
      <th>검</th>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>114.0</td>
    </tr>
    <tr>
      <th>결</th>
      <td>184.0</td>
      <td>63.0</td>
      <td>NaN</td>
      <td>1159.0</td>
      <td>1199.0</td>
      <td>938.0</td>
    </tr>
    <tr>
      <th>겸</th>
      <td>NaN</td>
      <td>65.0</td>
      <td>54.0</td>
      <td>92.0</td>
      <td>NaN</td>
      <td>950.0</td>
    </tr>
    <tr>
      <th>...</th>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
    </tr>
    <tr>
      <th>후</th>
      <td>616.0</td>
      <td>470.0</td>
      <td>95.0</td>
      <td>2049.0</td>
      <td>4636.0</td>
      <td>3863.0</td>
    </tr>
    <tr>
      <th>훈</th>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>7491.0</td>
      <td>6692.0</td>
      <td>2616.0</td>
    </tr>
    <tr>
      <th>훤</th>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>94.0</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>휘</th>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>285.0</td>
      <td>331.0</td>
      <td>71.0</td>
    </tr>
    <tr>
      <th>희</th>
      <td>6797.0</td>
      <td>6204.0</td>
      <td>3424.0</td>
      <td>2148.0</td>
      <td>2120.0</td>
      <td>1035.0</td>
    </tr>
  </tbody>
</table>
<p>128 rows × 6 columns</p>
</div>




```python
subtable.sum()
letter_prop = subtable / subtable.sum()
letter_prop
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead tr th {
        text-align: left;
    }

    .dataframe thead tr:last-of-type th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr>
      <th>gender</th>
      <th colspan="3" halign="left">F</th>
      <th colspan="3" halign="left">M</th>
    </tr>
    <tr>
      <th>year</th>
      <th>2008</th>
      <th>2012</th>
      <th>2018</th>
      <th>2008</th>
      <th>2012</th>
      <th>2018</th>
    </tr>
    <tr>
      <th>last_letter</th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>강</th>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>0.000627</td>
      <td>0.000599</td>
      <td>0.000537</td>
    </tr>
    <tr>
      <th>건</th>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>0.010381</td>
      <td>0.012999</td>
      <td>0.011468</td>
    </tr>
    <tr>
      <th>검</th>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>0.000827</td>
    </tr>
    <tr>
      <th>결</th>
      <td>0.000962</td>
      <td>0.000309</td>
      <td>NaN</td>
      <td>0.006855</td>
      <td>0.006467</td>
      <td>0.006808</td>
    </tr>
    <tr>
      <th>겸</th>
      <td>NaN</td>
      <td>0.000319</td>
      <td>0.000373</td>
      <td>0.000544</td>
      <td>NaN</td>
      <td>0.006895</td>
    </tr>
    <tr>
      <th>...</th>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
    </tr>
    <tr>
      <th>후</th>
      <td>0.003220</td>
      <td>0.002304</td>
      <td>0.000656</td>
      <td>0.012120</td>
      <td>0.025006</td>
      <td>0.028039</td>
    </tr>
    <tr>
      <th>훈</th>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>0.044309</td>
      <td>0.036095</td>
      <td>0.018988</td>
    </tr>
    <tr>
      <th>훤</th>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>0.000507</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>휘</th>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>0.001686</td>
      <td>0.001785</td>
      <td>0.000515</td>
    </tr>
    <tr>
      <th>희</th>
      <td>0.035534</td>
      <td>0.030408</td>
      <td>0.023644</td>
      <td>0.012705</td>
      <td>0.011435</td>
      <td>0.007512</td>
    </tr>
  </tbody>
</table>
<p>128 rows × 6 columns</p>
</div>




```python
import matplotlib.pyplot as plt

fig, axes = plt.subplots(2, 1, figsize=(30, 20))
letter_prop['M'].plot(kind='bar', rot=0, ax=axes[0], title='Male')
letter_prop['F'].plot(kind='bar', rot=0, ax=axes[1], title='Female',
                      legend=False)
```




    <matplotlib.axes._subplots.AxesSubplot at 0x2c52fc2ad00>




![svg](/img/output_24_1.svg)



```python
letter_prop = table/table.sum()
dny_is = letter_prop.loc[['민','우','준'],'M'].T
dny_is.head()
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
      <th>last_letter</th>
      <th>민</th>
      <th>우</th>
      <th>준</th>
    </tr>
    <tr>
      <th>year</th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>2008</th>
      <td>0.070548</td>
      <td>0.098378</td>
      <td>0.095745</td>
    </tr>
    <tr>
      <th>2009</th>
      <td>0.071303</td>
      <td>0.106945</td>
      <td>0.103008</td>
    </tr>
    <tr>
      <th>2010</th>
      <td>0.061610</td>
      <td>0.112214</td>
      <td>0.112383</td>
    </tr>
    <tr>
      <th>2011</th>
      <td>0.058171</td>
      <td>0.113082</td>
      <td>0.118197</td>
    </tr>
    <tr>
      <th>2012</th>
      <td>0.051883</td>
      <td>0.120518</td>
      <td>0.116748</td>
    </tr>
  </tbody>
</table>
</div>




```python
plt.close()
fig = plt.figure()
dny_is.plot()
```




    <matplotlib.axes._subplots.AxesSubplot at 0x2c52cd15be0>




    <Figure size 432x288 with 0 Axes>



![svg](/img/output_26_2.svg)


### 남자이름 -> 여자이름   



```python
all_names = pd.Series(top1000.name.unique())
lesley_like = all_names[all_names.str.lower().str.contains('진')]
lesley_like
```




    10      유진
    13      예진
    28      서진
    53      수진
    104     현진
    127     혜진
    154     은진
    167     여진
    172     하진
    189     윤진
    191     희진
    199     진서
    209     효진
    210     아진
    220     진아
    283     민진
    290     진영
    293     진희
    301      진
    304     진주
    306     연진
    313     세진
    359     미진
    448     진경
    454     소진
    476     경진
    499     우진
    515     진우
    598     성진
    605     진호
    614     진혁
    624     호진
    629     영진
    660     진성
    704     형진
    707     진욱
    712     상진
    739     승진
    745     진수
    755     진석
    770     동진
    783     진현
    790     진원
    805     명진
    835     규진
    838     혁진
    849     진형
    851     범진
    870     석진
    881     태진
    937     의진
    939     원진
    971     정진
    1003    도진
    1044    용진
    1061    진후
    1107    이진
    1141    시진
    dtype: object




```python
filtered = top1000[top1000.name.isin(lesley_like)]
filtered.groupby('name').births.sum()
```




    name
    경진       73
    규진      766
    도진     2327
    동진      249
    명진      484
    미진      266
    민진      843
    범진      500
    상진      393
    서진    25862
    석진       94
    성진     2128
    세진     2902
    소진      265
    수진     5855
    승진      699
    시진      393
    아진     3121
    여진     2291
    연진      811
    영진     1226
    예진    13152
    용진       91
    우진    19024
    원진       88
    유진    17521
    윤진     2297
    은진     1725
    의진      180
    이진      473
    정진      100
    진      2401
    진경       83
    진서     4389
    진석      375
    진성     1296
    진수      530
    진아     2044
    진영     2371
    진우    10936
    진욱     1713
    진원     1060
    진주      927
    진혁     2628
    진현      316
    진형      301
    진호     2879
    진후      352
    진희      853
    태진       90
    하진    10414
    혁진      205
    현진     6272
    형진      671
    혜진     2628
    호진     2681
    효진     1599
    희진     1649
    Name: births, dtype: int64




```python
table = filtered.pivot_table('births', index='year',
                             columns='gender', aggfunc='sum')
table = table.div(table.sum(1), axis=0)
table.tail()
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
      <th>gender</th>
      <th>F</th>
      <th>M</th>
    </tr>
    <tr>
      <th>year</th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>2016</th>
      <td>0.400176</td>
      <td>0.599824</td>
    </tr>
    <tr>
      <th>2017</th>
      <td>0.396162</td>
      <td>0.603838</td>
    </tr>
    <tr>
      <th>2018</th>
      <td>0.373367</td>
      <td>0.626633</td>
    </tr>
    <tr>
      <th>2019</th>
      <td>0.348980</td>
      <td>0.651020</td>
    </tr>
    <tr>
      <th>2020</th>
      <td>0.357599</td>
      <td>0.642401</td>
    </tr>
  </tbody>
</table>
</div>




```python
fig = plt.figure()
table.plot(style={'M': 'k-', 'F': 'b--'})
```




    <matplotlib.axes._subplots.AxesSubplot at 0x2c52e70f880>




    <Figure size 432x288 with 0 Axes>



![svg](/img/output_31_2.svg)



```python
from IPython.core.display import display, HTML
display(HTML("<style>.container {width:90% !important; }</style>"))
```


<style>.container {width:90% !important; }</style>



```python

```
