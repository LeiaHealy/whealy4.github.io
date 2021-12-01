Liam Healy

Site link: [whealy4.github.io](whealy4.github.io)

# An Analysis of 50 Years of Terrorism

# Milestone 1: Groups, Data, Website, and Extraction, Transform, and Load (ETL)

Terrorism causes fear and terror, as it is aptly named, across the world. People commit acts of terror for political, religious, and personal reasons along with many more. In addition, these are committed in multiple ways — e.g. bombing, kidnapping, assault, etc. — with a variety of weapons such as IEDs, assault rifles, poison, etc. Terrorism has often been used as a weapon of fear, not by the terrorists themselves, but by the state that has just been attacked to further their goals by convincing their populace their goals must be achieved to prevent more terror. Through the Global Terrorism Database (GTD), I hope to robustly analyze data of acts of terror from 1970 to 2019. 

How does one classify an attack as that of a terrorist? 
In what countries are certain acts of terror most committed?
Can we predict what country or region will be attacked given their method and target of terror?

Fortunately, the GTD appears to have the answer to all of the above and more, but the analysis can be potentially taken further with the inclusion of the CIA world factbook (CWF). Regarding the terrorists themselves, people do not simply one day wake up and decide to commit an act of terror. There are many outside factors that can affect such a drastic decision. People from countries with low unemployment, low GDP, and low education may have a higher chance of committing acts of terror. The world factbook contains information on GDP, natural resources, unemployment, and more that can be cross-referenced with the GDT enabling access to more potential predictors (and overall data as a whole) leading to a far more thorough analysis. More questions can be asked such as:

Do countries with high natural resources get terrorized more than those without them, or is it only those with oil and natural gas? 
How does military expending affect the frequency of terror? Or does large military spending offer more or less protection depending on GDP?
Can we use all of the above along with the target type, attack type, and nationality to predict which country has a high chance of being attacked? 


## Reading in Data


```python
pip install openpyxl
```

    Requirement already satisfied: openpyxl in /opt/conda/lib/python3.9/site-packages (3.0.9)
    Requirement already satisfied: et-xmlfile in /opt/conda/lib/python3.9/site-packages (from openpyxl) (1.1.0)
    Note: you may need to restart the kernel to use updated packages.



```python
%matplotlib inline
import pandas as pd
from pandas import ExcelWriter
from pandas import ExcelFile
import numpy as np
import matplotlib
import matplotlib.pyplot as plt
import matplotlib.dates as dates
from matplotlib.pyplot import cm
from matplotlib.patches import Patch

pd.set_option('display.max_columns', None)

```

Read in the GDT data


```python
df = pd.read_excel('./data/globalterrorismdb_0221dist.xlsx')
df_1993 = pd.read_excel('./data/gtd1993_0221dist.xlsx')
```

The 1993 data was left out, so it must be added to the data set and placed into the right location, between 1992 and 1994.


```python
gtd_df = pd.concat([df_1993, df])
gtd_df = gtd_df.sort_values(by=['eventid'])
gtd_df = gtd_df.reset_index()
gtd_df.drop(['index'],axis=1)
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
      <th>eventid</th>
      <th>iyear</th>
      <th>imonth</th>
      <th>iday</th>
      <th>approxdate</th>
      <th>extended</th>
      <th>resolution</th>
      <th>country</th>
      <th>country_txt</th>
      <th>region</th>
      <th>region_txt</th>
      <th>provstate</th>
      <th>city</th>
      <th>latitude</th>
      <th>longitude</th>
      <th>specificity</th>
      <th>vicinity</th>
      <th>location</th>
      <th>summary</th>
      <th>crit1</th>
      <th>crit2</th>
      <th>crit3</th>
      <th>doubtterr</th>
      <th>alternative</th>
      <th>alternative_txt</th>
      <th>multiple</th>
      <th>success</th>
      <th>suicide</th>
      <th>attacktype1</th>
      <th>attacktype1_txt</th>
      <th>attacktype2</th>
      <th>attacktype2_txt</th>
      <th>attacktype3</th>
      <th>attacktype3_txt</th>
      <th>targtype1</th>
      <th>targtype1_txt</th>
      <th>targsubtype1</th>
      <th>targsubtype1_txt</th>
      <th>corp1</th>
      <th>target1</th>
      <th>natlty1</th>
      <th>natlty1_txt</th>
      <th>targtype2</th>
      <th>targtype2_txt</th>
      <th>targsubtype2</th>
      <th>targsubtype2_txt</th>
      <th>corp2</th>
      <th>target2</th>
      <th>natlty2</th>
      <th>natlty2_txt</th>
      <th>targtype3</th>
      <th>targtype3_txt</th>
      <th>targsubtype3</th>
      <th>targsubtype3_txt</th>
      <th>corp3</th>
      <th>target3</th>
      <th>natlty3</th>
      <th>natlty3_txt</th>
      <th>gname</th>
      <th>gsubname</th>
      <th>gname2</th>
      <th>gsubname2</th>
      <th>gname3</th>
      <th>gsubname3</th>
      <th>motive</th>
      <th>guncertain1</th>
      <th>guncertain2</th>
      <th>guncertain3</th>
      <th>individual</th>
      <th>nperps</th>
      <th>nperpcap</th>
      <th>claimed</th>
      <th>claimmode</th>
      <th>claimmode_txt</th>
      <th>claim2</th>
      <th>claimmode2</th>
      <th>claimmode2_txt</th>
      <th>claim3</th>
      <th>claimmode3</th>
      <th>claimmode3_txt</th>
      <th>compclaim</th>
      <th>weaptype1</th>
      <th>weaptype1_txt</th>
      <th>weapsubtype1</th>
      <th>weapsubtype1_txt</th>
      <th>weaptype2</th>
      <th>weaptype2_txt</th>
      <th>weapsubtype2</th>
      <th>weapsubtype2_txt</th>
      <th>weaptype3</th>
      <th>weaptype3_txt</th>
      <th>weapsubtype3</th>
      <th>weapsubtype3_txt</th>
      <th>weaptype4</th>
      <th>weaptype4_txt</th>
      <th>weapsubtype4</th>
      <th>weapsubtype4_txt</th>
      <th>weapdetail</th>
      <th>nkill</th>
      <th>nkillus</th>
      <th>nkillter</th>
      <th>nwound</th>
      <th>nwoundus</th>
      <th>nwoundte</th>
      <th>property</th>
      <th>propextent</th>
      <th>propextent_txt</th>
      <th>propvalue</th>
      <th>propcomment</th>
      <th>ishostkid</th>
      <th>nhostkid</th>
      <th>nhostkidus</th>
      <th>nhours</th>
      <th>ndays</th>
      <th>divert</th>
      <th>kidhijcountry</th>
      <th>ransom</th>
      <th>ransomamt</th>
      <th>ransomamtus</th>
      <th>ransompaid</th>
      <th>ransompaidus</th>
      <th>ransomnote</th>
      <th>hostkidoutcome</th>
      <th>hostkidoutcome_txt</th>
      <th>nreleased</th>
      <th>addnotes</th>
      <th>scite1</th>
      <th>scite2</th>
      <th>scite3</th>
      <th>dbsource</th>
      <th>INT_LOG</th>
      <th>INT_IDEO</th>
      <th>INT_MISC</th>
      <th>INT_ANY</th>
      <th>related</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>197000000001</td>
      <td>1970</td>
      <td>7</td>
      <td>2</td>
      <td>NaN</td>
      <td>0</td>
      <td>NaT</td>
      <td>58</td>
      <td>Dominican Republic</td>
      <td>2</td>
      <td>Central America &amp; Caribbean</td>
      <td>National</td>
      <td>Santo Domingo</td>
      <td>18.456792</td>
      <td>-69.951164</td>
      <td>1.0</td>
      <td>0</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>0</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>0</td>
      <td>1.0</td>
      <td>0</td>
      <td>1</td>
      <td>Assassination</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>14</td>
      <td>Private Citizens &amp; Property</td>
      <td>68.0</td>
      <td>Named Civilian</td>
      <td>NaN</td>
      <td>Julio Guzman</td>
      <td>58.0</td>
      <td>Dominican Republic</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>MANO-D</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>0.0</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>0</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>13</td>
      <td>Unknown</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>1.0</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>0.0</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>0</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>0.0</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>0.0</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>PGIS</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>1</th>
      <td>197000000002</td>
      <td>1970</td>
      <td>0</td>
      <td>0</td>
      <td>NaN</td>
      <td>0</td>
      <td>NaT</td>
      <td>130</td>
      <td>Mexico</td>
      <td>1</td>
      <td>North America</td>
      <td>Federal</td>
      <td>Mexico city</td>
      <td>19.371887</td>
      <td>-99.086624</td>
      <td>1.0</td>
      <td>0</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>0</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>0</td>
      <td>1.0</td>
      <td>0</td>
      <td>6</td>
      <td>Hostage Taking (Kidnapping)</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>7</td>
      <td>Government (Diplomatic)</td>
      <td>45.0</td>
      <td>Diplomatic Personnel (outside of embassy, cons...</td>
      <td>Belgian Ambassador Daughter</td>
      <td>Nadine Chaval, daughter</td>
      <td>21.0</td>
      <td>Belgium</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>23rd of September Communist League</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>0.0</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>0</td>
      <td>7.0</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>13</td>
      <td>Unknown</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>0.0</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>0.0</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>0</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>1.0</td>
      <td>1.0</td>
      <td>0.0</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>Mexico</td>
      <td>1.0</td>
      <td>800000.0</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>PGIS</td>
      <td>0</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>2</th>
      <td>197001000001</td>
      <td>1970</td>
      <td>1</td>
      <td>0</td>
      <td>NaN</td>
      <td>0</td>
      <td>NaT</td>
      <td>160</td>
      <td>Philippines</td>
      <td>5</td>
      <td>Southeast Asia</td>
      <td>Tarlac</td>
      <td>Unknown</td>
      <td>15.478598</td>
      <td>120.599741</td>
      <td>4.0</td>
      <td>0</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>0</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>0</td>
      <td>1.0</td>
      <td>0</td>
      <td>1</td>
      <td>Assassination</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>10</td>
      <td>Journalists &amp; Media</td>
      <td>54.0</td>
      <td>Radio Journalist/Staff/Facility</td>
      <td>Voice of America</td>
      <td>Employee</td>
      <td>217.0</td>
      <td>United States</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>Unknown</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>0.0</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>0</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>13</td>
      <td>Unknown</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>1.0</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>0.0</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>0</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>0.0</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>0.0</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>PGIS</td>
      <td>-9</td>
      <td>-9</td>
      <td>1</td>
      <td>1</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>3</th>
      <td>197001000002</td>
      <td>1970</td>
      <td>1</td>
      <td>0</td>
      <td>NaN</td>
      <td>0</td>
      <td>NaT</td>
      <td>78</td>
      <td>Greece</td>
      <td>8</td>
      <td>Western Europe</td>
      <td>Attica</td>
      <td>Athens</td>
      <td>37.997490</td>
      <td>23.762728</td>
      <td>1.0</td>
      <td>0</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>0</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>0</td>
      <td>1.0</td>
      <td>0</td>
      <td>3</td>
      <td>Bombing/Explosion</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>7</td>
      <td>Government (Diplomatic)</td>
      <td>46.0</td>
      <td>Embassy/Consulate</td>
      <td>NaN</td>
      <td>U.S. Embassy</td>
      <td>217.0</td>
      <td>United States</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>Unknown</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>0.0</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>0</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>6</td>
      <td>Explosives</td>
      <td>16.0</td>
      <td>Unknown Explosive Type</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>Explosive</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>1</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>0.0</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>0.0</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>PGIS</td>
      <td>-9</td>
      <td>-9</td>
      <td>1</td>
      <td>1</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>4</th>
      <td>197001000003</td>
      <td>1970</td>
      <td>1</td>
      <td>0</td>
      <td>NaN</td>
      <td>0</td>
      <td>NaT</td>
      <td>101</td>
      <td>Japan</td>
      <td>4</td>
      <td>East Asia</td>
      <td>Fukouka</td>
      <td>Fukouka</td>
      <td>33.580412</td>
      <td>130.396361</td>
      <td>1.0</td>
      <td>0</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>-9</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>0</td>
      <td>1.0</td>
      <td>0</td>
      <td>7</td>
      <td>Facility/Infrastructure Attack</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>7</td>
      <td>Government (Diplomatic)</td>
      <td>46.0</td>
      <td>Embassy/Consulate</td>
      <td>NaN</td>
      <td>U.S. Consulate</td>
      <td>217.0</td>
      <td>United States</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>Unknown</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>0.0</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>0</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>8</td>
      <td>Incendiary</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>Incendiary</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>1</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>0.0</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>0.0</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>PGIS</td>
      <td>-9</td>
      <td>-9</td>
      <td>1</td>
      <td>1</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>...</th>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
    </tr>
    <tr>
      <th>201926</th>
      <td>201912310028</td>
      <td>2019</td>
      <td>12</td>
      <td>31</td>
      <td>December 31, 2019</td>
      <td>0</td>
      <td>NaT</td>
      <td>95</td>
      <td>Iraq</td>
      <td>10</td>
      <td>Middle East &amp; North Africa</td>
      <td>Baghdad</td>
      <td>Baghdad</td>
      <td>33.303567</td>
      <td>44.371771</td>
      <td>1.0</td>
      <td>0</td>
      <td>The incident occurred along Palestine Street.</td>
      <td>12/31/2019: An explosive device detonated outs...</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>0</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>0</td>
      <td>1.0</td>
      <td>0</td>
      <td>3</td>
      <td>Bombing/Explosion</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>14</td>
      <td>Private Citizens &amp; Property</td>
      <td>77.0</td>
      <td>Laborer (General)/Occupation Identified</td>
      <td>Not Applicable</td>
      <td>Residence of Tribal Leader</td>
      <td>95.0</td>
      <td>Iraq</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>Unknown</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>0.0</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>0</td>
      <td>-99.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>6</td>
      <td>Explosives</td>
      <td>16.0</td>
      <td>Unknown Explosive Type</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>1</td>
      <td>3.0</td>
      <td>Minor (likely &lt; $1 million)</td>
      <td>-99.0</td>
      <td>Building damaged.</td>
      <td>0.0</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>"Iraq: ISHM 235: December 20, 2019 - January 2...</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>START Primary Collection</td>
      <td>-9</td>
      <td>-9</td>
      <td>0</td>
      <td>-9</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>201927</th>
      <td>201912310030</td>
      <td>2019</td>
      <td>12</td>
      <td>31</td>
      <td>NaN</td>
      <td>0</td>
      <td>NaT</td>
      <td>195</td>
      <td>Sudan</td>
      <td>11</td>
      <td>Sub-Saharan Africa</td>
      <td>West Darfur</td>
      <td>El Geneina</td>
      <td>13.440886</td>
      <td>22.441728</td>
      <td>1.0</td>
      <td>0</td>
      <td>NaN</td>
      <td>12/31/2019: Assailants attacked the police hea...</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>0</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>0</td>
      <td>1.0</td>
      <td>0</td>
      <td>9</td>
      <td>Unknown</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>3</td>
      <td>Police</td>
      <td>22.0</td>
      <td>Police Building (headquarters, station, school)</td>
      <td>Sudanese Police</td>
      <td>West Darfur Police Headquarters</td>
      <td>195.0</td>
      <td>Sudan</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>Unknown</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>0.0</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>0</td>
      <td>-99.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>13</td>
      <td>Unknown</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>2.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>1.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>1</td>
      <td>4.0</td>
      <td>Unknown</td>
      <td>-99.0</td>
      <td>Police vehicle and weapons stolen.</td>
      <td>0.0</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>"World: Protection in Danger Monthly News Brie...</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>START Primary Collection</td>
      <td>-9</td>
      <td>-9</td>
      <td>0</td>
      <td>-9</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>201928</th>
      <td>201912310031</td>
      <td>2019</td>
      <td>12</td>
      <td>31</td>
      <td>December 31, 2019</td>
      <td>0</td>
      <td>NaT</td>
      <td>195</td>
      <td>Sudan</td>
      <td>11</td>
      <td>Sub-Saharan Africa</td>
      <td>West Darfur</td>
      <td>El Geneina</td>
      <td>13.440886</td>
      <td>22.441728</td>
      <td>1.0</td>
      <td>0</td>
      <td>The incident occurred in El Jebel neighborhood.</td>
      <td>12/31/2019: Assailants attacked the West Darfu...</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>0</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>0</td>
      <td>1.0</td>
      <td>0</td>
      <td>2</td>
      <td>Armed Assault</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>2</td>
      <td>Government (General)</td>
      <td>21.0</td>
      <td>Government Building/Facility/Office</td>
      <td>Government of West Darfur</td>
      <td>West Darfur Legislative Council Building</td>
      <td>195.0</td>
      <td>Sudan</td>
      <td>3.0</td>
      <td>Police</td>
      <td>25.0</td>
      <td>Police Security Forces/Officers</td>
      <td>Sudanese Police</td>
      <td>Officers</td>
      <td>195.0</td>
      <td>Sudan</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>Unknown</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>0.0</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>0</td>
      <td>-99.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>5</td>
      <td>Firearms</td>
      <td>5.0</td>
      <td>Unknown Gun Type</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>2.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>1</td>
      <td>4.0</td>
      <td>Unknown</td>
      <td>-99.0</td>
      <td>Items stolen from government building.</td>
      <td>0.0</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>"World: Protection in Danger Monthly News Brie...</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>START Primary Collection</td>
      <td>-9</td>
      <td>-9</td>
      <td>0</td>
      <td>-9</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>201929</th>
      <td>201912310032</td>
      <td>2019</td>
      <td>12</td>
      <td>31</td>
      <td>NaN</td>
      <td>0</td>
      <td>NaT</td>
      <td>92</td>
      <td>India</td>
      <td>6</td>
      <td>South Asia</td>
      <td>Jammu and Kashmir</td>
      <td>Bagiot Dora</td>
      <td>33.812790</td>
      <td>74.097730</td>
      <td>1.0</td>
      <td>0</td>
      <td>NaN</td>
      <td>12/31/2019: A landmine detonated targeting a c...</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>0</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>0</td>
      <td>1.0</td>
      <td>0</td>
      <td>3</td>
      <td>Bombing/Explosion</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>14</td>
      <td>Private Citizens &amp; Property</td>
      <td>67.0</td>
      <td>Unnamed Civilian/Unspecified</td>
      <td>Not Applicable</td>
      <td>Civilian</td>
      <td>92.0</td>
      <td>India</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>Unknown</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>0.0</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>0</td>
      <td>-99.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>6</td>
      <td>Explosives</td>
      <td>8.0</td>
      <td>Landmine</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>1.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>0.0</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>"Civilian injured in landmine blast in Indian-...</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>START Primary Collection</td>
      <td>-9</td>
      <td>-9</td>
      <td>0</td>
      <td>-9</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>201930</th>
      <td>201912310033</td>
      <td>2019</td>
      <td>12</td>
      <td>31</td>
      <td>NaN</td>
      <td>0</td>
      <td>NaT</td>
      <td>44</td>
      <td>China</td>
      <td>4</td>
      <td>East Asia</td>
      <td>Hong Kong</td>
      <td>Hong Kong</td>
      <td>22.340073</td>
      <td>114.138494</td>
      <td>1.0</td>
      <td>0</td>
      <td>The incident occurred in Lai Chi Kok neighborh...</td>
      <td>12/31/2019: Assailants threw petrol bombs at g...</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>0</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>0</td>
      <td>1.0</td>
      <td>0</td>
      <td>7</td>
      <td>Facility/Infrastructure Attack</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>2</td>
      <td>Government (General)</td>
      <td>21.0</td>
      <td>Government Building/Facility/Office</td>
      <td>Government of Lai Chi Kok</td>
      <td>Offices</td>
      <td>89.0</td>
      <td>Hong Kong</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>Unknown</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>0.0</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>0</td>
      <td>2.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>8</td>
      <td>Incendiary</td>
      <td>19.0</td>
      <td>Molotov Cocktail/Petrol Bomb</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>Petrol bombs were used in the attack.</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>1</td>
      <td>3.0</td>
      <td>Minor (likely &lt; $1 million)</td>
      <td>-99.0</td>
      <td>Shutters and a floor were damaged.</td>
      <td>0.0</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>"Hong Kong restaurant firebombed by black-clad...</td>
      <td>"Hong Kong restaurant firebombed by black-clad...</td>
      <td>NaN</td>
      <td>START Primary Collection</td>
      <td>-9</td>
      <td>-9</td>
      <td>1</td>
      <td>1</td>
      <td>NaN</td>
    </tr>
  </tbody>
</table>
<p>201931 rows × 135 columns</p>
</div>



Create datetime objects and add to new column 'date' to enable easy date access. However, it has an incredibly slow runtime relative to the rest of the analysis.


```python
gtd_df['date'] = 0

# There's gotta be a faster way to do this
for i, (y, m, d) in enumerate(zip(gtd_df['iyear'], gtd_df['imonth'], gtd_df['iday'])):
    if m == 0:
        gtd_df.loc[i,'date'] = pd.to_datetime(y, format='%Y')
    else:
        if d == 0:
            gtd_df.loc[i, 'date'] = pd.to_datetime(str(y)+str(m), format='%Y%m')
        else:
            gtd_df.loc[i, 'date'] = pd.to_datetime(str(y)+str(m)+str(d), format='%Y%m%d',errors='coerce')
```

Drop columns that do not appear to be of any use. Rename columns with multiple dropped types.
Replace data indicating nothing with NaNs.


```python
drop_cols = ['extended','resolution','vicinity','latitude','longitude','gname2','gsubname2','gname3','gsubname3',
             'guncertain2','guncertain3','claimmode','claimmode_txt','claim2','claimmode2','claimmode2_txt',
             'claim3','claimmode3','claimmode3_txt','compclaim','nhours','ndays','divert','ransomnote','addnotes',
             'scite1','scite2','scite3','dbsource','imonth','iday','approxdate']
# Potential drops: specificity, multiple, related
gtd_df.drop(columns=drop_cols)
gtd_df = gtd_df.rename(columns={'attacktype1':'attacktype','attacktype1_txt':'attacktype_txt','targtype1':'targtype',
                                'targtype1_txt':'targtype_txt', 'natlty1':'natlty','natlty1_txt':'natlty_txt'})
gtd_df = gtd_df.replace(-99, np.nan)
gtd_df = gtd_df.replace(-9, np.nan)
```

# Milestone 2: Additional Extraction, Transform, and Load (ETL) + Exploratory Data Analysis (EDA)

A basic analysis of the regions with the most acts of terror show the Middle East & Northern Africa along with South Asia in a clear lead, most likely due to greater political instability within the regions.


```python
fig, ax = plt.subplots(1,1)
region_cts = gtd_df['region_txt'].value_counts()
fig = region_cts.plot.bar(figsize=(20,10))
```


    
![png](output_17_0.png)
    


Create dataframes per region along with a list of region names for ease of use later.


```python
region_dfs = [gtd_df[gtd_df['region_txt'] == 'Western Europe'],
              gtd_df[gtd_df['region_txt'] == 'North America'],
              gtd_df[gtd_df['region_txt'] == 'Middle East & North Africa'],
              gtd_df[gtd_df['region_txt'] == 'South America'],
              gtd_df[gtd_df['region_txt'] == 'Southeast Asia'],
              gtd_df[gtd_df['region_txt'] == 'Sub-Saharan Africa'],
              gtd_df[gtd_df['region_txt'] == 'South Asia'],
              gtd_df[gtd_df['region_txt'] == 'Central America & Caribbean'],
              gtd_df[gtd_df['region_txt'] == 'Eastern Europe'],
              gtd_df[gtd_df['region_txt'] == 'East Asia'],
              gtd_df[gtd_df['region_txt'] == 'Australasia & Oceania'],
              gtd_df[gtd_df['region_txt'] == 'Central Asia']
             ]
region_names = ['Western Europe', 'North America', 'Middle East & North Africa', 'South America', 'Southeast Asia', 
                'Sub-Saharan Africa', 'South Asia', 'Central America & Caribbean', 'Eastern Europe', 'East Asia', 
                'Australasia & Oceania', 'Central Asia']
```

Below illustrates the proportions in two separate graphs of terrorist attacks and targets along with their relative size with respect to other regions. 


```python
attacktype_cts = gtd_df['attacktype_txt'].value_counts()
targtype_cts = gtd_df['targtype_txt'].value_counts()

targ_region_cts = pd.crosstab(gtd_df['region_txt'], gtd_df['targtype_txt'], normalize=True)
atk_region_cts = pd.crosstab(gtd_df['region_txt'], gtd_df['attacktype_txt'], normalize=True)
fig1 = atk_region_cts.plot.bar(stacked=True, figsize=(20,10), 
                               title='Types of Terrorist Attacks & Proportions vs. Regions')
fig2 = targ_region_cts.plot.bar(stacked=True, figsize=(20,10), 
                                title= 'Types of Terrorist Attacks & Proportions vs. Regions')

```


    
![png](output_21_0.png)
    



    
![png](output_21_1.png)
    


Next, we look at the proportions of the types of attacks on specific targets. It makes sense that abortion related acts 
of terror would involve a facility/infrastructure attack, as well as airports & aircraft having the highest relative proportion
of hijacking.


```python
fig, ax = plt.subplots(1,1)
targ_region_cts = pd.crosstab(gtd_df['targtype_txt'], gtd_df['attacktype_txt'])
targ_cts = targ_region_cts.sum(axis=1)
attk_cts = targ_region_cts.sum(axis=0)
region_given_targ = targ_region_cts.divide(targ_cts, axis=0)
fig = region_given_targ.plot.bar(ax=ax, stacked=True, figsize=(15,8), title='Proportions of Attack Types on Targets')
ax.set_xlabel('Target Type')
ax.set_ylabel('% Occurrence')
ax.legend(bbox_to_anchor=(1, 1))
```




    <matplotlib.legend.Legend at 0x7f1fdd5483d0>




    
![png](output_23_1.png)
    


Interestingly enough, all terrorist attacks appear to have a staggering success rate. Central America & the Carribean have an incredibly low failure rate.


```python
fig, ax = plt.subplots(4, 3)
color_dict = {0.0:'crimson', 1.0:'forestgreen'}
labels = ['Success', 'Failure']
i = 0
fig.suptitle('Success vs. Failure of Terrorist Attacks by Region', fontsize=20)
for r in range(4):
    for c in range(3):
        ax[r,c] = (pd.crosstab(region_dfs[i]['region_txt'], 
                               region_dfs[i]['success'], 
                               normalize=True)).plot.bar(ax=ax[r,c], legend=False,figsize=(20,10), color=color_dict, rot=0)
        ax[r,c].set_xlabel('')
        i+=1
# Add a single legend and move it outside of the graph
ax[0, 2].legend(['Failure','Success'], bbox_to_anchor=(1.21,1))
fig.tight_layout()

```


    
![png](output_25_0.png)
    


Below are graphs displaying the frequency of terrorist attacks from 1970 - 2019 per region. However, something to consider is that the y-axis changes per graph, for if they all shared the same, it would lose information in regions with less terrorism attacks than others.


```python
fig, ax = plt.subplots(4, 3)
fig.suptitle('Frequency of Terrorist Attacks Over Time by Region', fontsize=20)
i = 0
for r in range(4):
    for c in range(3):
        region_dfs[i].groupby('iyear').crit1.count().plot(ax=ax[r,c], figsize=(20,10), title=region_names[i])
        i += 1
        ax[r,c].set_xlabel('Year')
        ax[r,c].set_ylabel('# of Occurrences')


fig.tight_layout()
```


    
![png](output_27_0.png)
    


To analyze a correlation between categorical variables, we must create dummy variables from them. Proceed to strip the 
correlation matrix to allow for easier plotting. In addition, strip the prefixes added by turning the values into dummy variables. 


```python
tmp_df = gtd_df[['region_txt', 'targtype_txt']].copy()
terr_dummy = pd.get_dummies(tmp_df)
terr_dummy.drop(columns=['targtype_txt_Unknown'])
terr_corr = terr_dummy.corr()
terr_corr.drop(terr_corr.iloc[:, 12:], inplace = True, axis = 1)
terr_corr.drop(terr_corr.iloc[:, :12], inplace = True, axis = 0)

terr_corr.columns = terr_corr.columns.str.lstrip('region_txt_')
terr_corr = terr_corr.reset_index()
terr_corr['index'] = terr_corr['index'].str.lstrip('targtype_txt_')
```

The most difficult part of this was honestly figuring out how to color each bar and get a corresponding, single legend. In doing so, I was able to eliminate the x-labels which crowded up the majority of the graph space.


```python
fig, axs = plt.subplots(4, 3, figsize=(20,10), sharey=True)
fig.suptitle('Correlation Between Region & Attack Target', fontsize=20)
i = 0
color = cm.rainbow(np.linspace(0, 1, 22))
for t, ax in enumerate(axs.ravel()):
    x = terr_corr['index']
    height = terr_corr[region_names[i]]
    ax.bar(x, height, color=color)
    ax.set_title(region_names[i])
    ax.get_xaxis().set_visible(False)
    i+=1

custom_leg = []
for i, n in enumerate(terr_corr['index']):
    custom_leg.append(Patch(facecolor=color[i], label=n))    
fig.set_figheight(15)
fig.set_figwidth(15)    

handles = terr_corr['index']

# Add a single legend and move it outside of the graph
axs[1,2].legend(custom_leg, handles, bbox_to_anchor=(1,1))
fig.subplots_adjust(top=0.93)
```


    
![png](output_31_0.png)
    


Now, we have the correlation chart above, illustrating the disproportional abortion related attacks in North America as well as other information such as utilities being frequent targets in South America, Central America, and the Carribbean.

## To do:

Next steps are to incorporate the CIA World Factbook, and combine it with what is already here to go deeper on what has been found. 

How does the overall economy of a country affect the spike in terrorist attacks? For example, what happened in ~2015 that prompted drastic spikes within five regions?

Can we predict the target group given the type of attack, weapon, motive, and group?
