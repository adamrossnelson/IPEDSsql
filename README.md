
# Introduction

This notebook demonstrates code that will connect to, and import from, a Microsoft Access data file. In this case `IPEDS201617.accdb` which is among many `.accdb` files available from the [IPEDS Microsoft Access data files](https://nces.ed.gov/ipeds/use-the-data/download-access-database).

## Configuration note

This example was developed with in a 64bit Python enviornment. To function, your environment should have a 64bit version of Microsoft Office Installed (the default is 32bit). In addition to having installed a 64bit version of Microsoft Office this code also required an installation of the 64bit [Microsoft Acces Database Engine 2010](https://www.microsoft.com/en-US/download/details.aspx?id=13255). Documentation for `pyodbc` [here](https://github.com/mkleehammer/pyodbc/wiki/Connecting-to-Microsoft-Access) and this [Feb 22 2018 Stackoverflow Answer from Grimravus](https://stackoverflow.com/a/48937535/9572143) provide additional background.


```python
import numpy as np
import pandas as pd
from pandas import Series, DataFrame
import pyodbc
import os
```


```python
# Build file location using os.path.join() to ensure cross-platform operations.
db_file = (os.path.join('data', 'IPEDS201617.accdb'))
```


```python
# Check to ensure file exists.
os.path.isfile(db_file)
```




    True




```python
# Open ODBC connection (See configuration notes above, if errors).
conn = pyodbc.connect(
    r'Driver={Microsoft Access Driver (*.mdb, *.accdb)};' +
    r'Dbq=' + db_file + r';')
```


```python
# Get directory information for all institutions & display head.
data = pd.read_sql('SELECT * FROM hd2016', conn)
data.head()
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
      <th>UNITID</th>
      <th>INSTNM</th>
      <th>IALIAS</th>
      <th>ADDR</th>
      <th>CITY</th>
      <th>STABBR</th>
      <th>ZIP</th>
      <th>FIPS</th>
      <th>OBEREG</th>
      <th>CHFNM</th>
      <th>...</th>
      <th>CBSATYPE</th>
      <th>CSA</th>
      <th>NECTA</th>
      <th>COUNTYCD</th>
      <th>COUNTYNM</th>
      <th>CNGDSTCD</th>
      <th>LONGITUD</th>
      <th>LATITUDE</th>
      <th>DFRCGID</th>
      <th>DFRCUSCG</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>100654</td>
      <td>Alabama A &amp; M University</td>
      <td>AAMU</td>
      <td>4900 Meridian Street</td>
      <td>Normal</td>
      <td>AL</td>
      <td>35762</td>
      <td>1</td>
      <td>5</td>
      <td>Dr. Andrew Hugine, Jr.</td>
      <td>...</td>
      <td>1</td>
      <td>290</td>
      <td>-2</td>
      <td>1089</td>
      <td>Madison County</td>
      <td>105</td>
      <td>-86.568502</td>
      <td>34.783368</td>
      <td>128</td>
      <td>1</td>
    </tr>
    <tr>
      <th>1</th>
      <td>100663</td>
      <td>University of Alabama at Birmingham</td>
      <td></td>
      <td>Administration Bldg Suite 1070</td>
      <td>Birmingham</td>
      <td>AL</td>
      <td>35294-0110</td>
      <td>1</td>
      <td>5</td>
      <td>Ray L. Watts</td>
      <td>...</td>
      <td>1</td>
      <td>142</td>
      <td>-2</td>
      <td>1073</td>
      <td>Jefferson County</td>
      <td>107</td>
      <td>-86.799345</td>
      <td>33.505697</td>
      <td>115</td>
      <td>1</td>
    </tr>
    <tr>
      <th>2</th>
      <td>100690</td>
      <td>Amridge University</td>
      <td>Southern Christian University |Regions University</td>
      <td>1200 Taylor Rd</td>
      <td>Montgomery</td>
      <td>AL</td>
      <td>36117-3553</td>
      <td>1</td>
      <td>5</td>
      <td>Michael Turner</td>
      <td>...</td>
      <td>1</td>
      <td>-2</td>
      <td>-2</td>
      <td>1101</td>
      <td>Montgomery County</td>
      <td>102</td>
      <td>-86.174010</td>
      <td>32.362609</td>
      <td>236</td>
      <td>2</td>
    </tr>
    <tr>
      <th>3</th>
      <td>100706</td>
      <td>University of Alabama in Huntsville</td>
      <td>UAH |University of Alabama Huntsville</td>
      <td>301 Sparkman Dr</td>
      <td>Huntsville</td>
      <td>AL</td>
      <td>35899</td>
      <td>1</td>
      <td>5</td>
      <td>Robert A. Altenkirch</td>
      <td>...</td>
      <td>1</td>
      <td>290</td>
      <td>-2</td>
      <td>1089</td>
      <td>Madison County</td>
      <td>105</td>
      <td>-86.640449</td>
      <td>34.724557</td>
      <td>118</td>
      <td>2</td>
    </tr>
    <tr>
      <th>4</th>
      <td>100724</td>
      <td>Alabama State University</td>
      <td></td>
      <td>915 S Jackson Street</td>
      <td>Montgomery</td>
      <td>AL</td>
      <td>36104-0271</td>
      <td>1</td>
      <td>5</td>
      <td>Leon Wilson</td>
      <td>...</td>
      <td>1</td>
      <td>-2</td>
      <td>-2</td>
      <td>1101</td>
      <td>Montgomery County</td>
      <td>107</td>
      <td>-86.295677</td>
      <td>32.364317</td>
      <td>136</td>
      <td>1</td>
    </tr>
  </tbody>
</table>
<p>5 rows × 72 columns</p>
</div>




```python
# Get directory information for specific institutions & display results
data = pd.read_sql('SELECT * FROM hd2016 WHERE UNITID=240444', conn)
```


```python
for var in data.columns:
    print('{:>12} : {:<}'.format(var, data[var][0]))
    
# Below the extensive output below is an alternate display option.
```

          UNITID : 240444
          INSTNM : University of Wisconsin-Madison
          IALIAS :  
            ADDR : 500 Lincoln Dr
            CITY : Madison
          STABBR : WI
             ZIP : 53706-1380
            FIPS : 55
          OBEREG : 3
           CHFNM : Rebecca Blank
        CHFTITLE : Chancellor
         GENTELE : 6082632400
             EIN : 391805963 
            DUNS : 170403497
           OPEID : 00389500  
         OPEFLAG : 1
         WEBADDR : www.wisc.edu
        ADMINURL : www.wisc.edu/admissions/
         FAIDURL : www.finaid.wisc.edu
         APPLURL : https://www.commonapp.org/
        NPRICURL : www.finaid.wisc.edu/award-estimator.php
          VETURL : veterans.wisc.edu
          ATHURL : apir.wisc.edu/retention.htm
         DISAURL : mcburney.wisc.edu/
          SECTOR : 1
         ICLEVEL : 1
         CONTROL : 1
         HLOFFER : 9
         UGOFFER : 1
         GROFFER : 1
        HDEGOFR1 : 11
        DEGGRANT : 1
            HBCU : 2
        HOSPITAL : 2
         MEDICAL : 1
          TRIBAL : 2
          LOCALE : 12
        OPENPUBL : 1
             ACT : A
           NEWID : -2
         DEATHYR : -2
        CLOSEDAT : -2        
        CYACTIVE : 1
         POSTSEC : 1
         PSEFLAG : 1
        PSET4FLG : 1
          RPTMTH : 1
         INSTCAT : 2
        C15BASIC : 15
         C15IPUG : 11
        C15IPGRD : 14
        C15UGPRF : 14
        C15ENPRF : 5
        C15SZSET : 16
         CCBASIC : 15
        CARNEGIE : 15
        LANDGRNT : 1
        INSTSIZE : 5
        F1SYSTYP : 1
        F1SYSNAM : University of Wisconsin System                                                  
        F1SYSCOD : 155010
            CBSA : 31540
        CBSATYPE : 1
             CSA : 357
           NECTA : -2
        COUNTYCD : 55025
        COUNTYNM : Dane County
        CNGDSTCD : 5502
        LONGITUD : -89.405356
        LATITUDE : 43.073858
         DFRCGID : 113
        DFRCUSCG : 1
    


```python
# Alternate single or few (1-3 insts) institutaional display option.
data = pd.read_sql('SELECT * FROM hd2016 WHERE UNITID IN (240444, 238263, 153658)', conn)
data.transpose()
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
      <th>0</th>
      <th>1</th>
      <th>2</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>UNITID</th>
      <td>153658</td>
      <td>238263</td>
      <td>240444</td>
    </tr>
    <tr>
      <th>INSTNM</th>
      <td>University of Iowa</td>
      <td>Madison Area Technical College</td>
      <td>University of Wisconsin-Madison</td>
    </tr>
    <tr>
      <th>IALIAS</th>
      <td>Iowa|UI</td>
      <td>Madison College</td>
      <td></td>
    </tr>
    <tr>
      <th>ADDR</th>
      <td>101 Jessup Hall</td>
      <td>1701 Wright St</td>
      <td>500 Lincoln Dr</td>
    </tr>
    <tr>
      <th>CITY</th>
      <td>Iowa City</td>
      <td>Madison</td>
      <td>Madison</td>
    </tr>
    <tr>
      <th>STABBR</th>
      <td>IA</td>
      <td>WI</td>
      <td>WI</td>
    </tr>
    <tr>
      <th>ZIP</th>
      <td>52242-1316</td>
      <td>53704-2599</td>
      <td>53706-1380</td>
    </tr>
    <tr>
      <th>FIPS</th>
      <td>19</td>
      <td>55</td>
      <td>55</td>
    </tr>
    <tr>
      <th>OBEREG</th>
      <td>4</td>
      <td>3</td>
      <td>3</td>
    </tr>
    <tr>
      <th>CHFNM</th>
      <td>Bruce Harreld</td>
      <td>Jack E Daniels III</td>
      <td>Rebecca Blank</td>
    </tr>
    <tr>
      <th>CHFTITLE</th>
      <td>President</td>
      <td>President</td>
      <td>Chancellor</td>
    </tr>
    <tr>
      <th>GENTELE</th>
      <td>3193353500</td>
      <td>6082466100</td>
      <td>6082632400</td>
    </tr>
    <tr>
      <th>EIN</th>
      <td>426004813</td>
      <td>391086718</td>
      <td>391805963</td>
    </tr>
    <tr>
      <th>DUNS</th>
      <td>062761617</td>
      <td>073849200|102217593|120703392|618159800|078396475</td>
      <td>170403497</td>
    </tr>
    <tr>
      <th>OPEID</th>
      <td>00189200</td>
      <td>00400700</td>
      <td>00389500</td>
    </tr>
    <tr>
      <th>OPEFLAG</th>
      <td>1</td>
      <td>1</td>
      <td>1</td>
    </tr>
    <tr>
      <th>WEBADDR</th>
      <td>www.uiowa.edu</td>
      <td>madisoncollege.edu</td>
      <td>www.wisc.edu</td>
    </tr>
    <tr>
      <th>ADMINURL</th>
      <td>admissions.uiowa.edu</td>
      <td>madisoncollege.edu/apply</td>
      <td>www.wisc.edu/admissions/</td>
    </tr>
    <tr>
      <th>FAIDURL</th>
      <td>financialaid.uiowa.edu/</td>
      <td>madisoncollege.edu/financial-aid</td>
      <td>www.finaid.wisc.edu</td>
    </tr>
    <tr>
      <th>APPLURL</th>
      <td>admissions.uiowa.edu/apply</td>
      <td>madisoncollege.edu/apply</td>
      <td>https://www.commonapp.org/</td>
    </tr>
    <tr>
      <th>NPRICURL</th>
      <td>npc.collegeboard.org/student/app/uiowa</td>
      <td>ire.madisoncollege.edu/heoa-calc/npcalc.htm</td>
      <td>www.finaid.wisc.edu/award-estimator.php</td>
    </tr>
    <tr>
      <th>VETURL</th>
      <td>registrar.uiowa.edu/gi-bill</td>
      <td>madisoncollege.edu/veterans-benefits</td>
      <td>veterans.wisc.edu</td>
    </tr>
    <tr>
      <th>ATHURL</th>
      <td>www.ncaa.org/about/resources/research/graduati...</td>
      <td>madisoncollege.edu/right-know</td>
      <td>apir.wisc.edu/retention.htm</td>
    </tr>
    <tr>
      <th>DISAURL</th>
      <td>sds.studentlife.uiowa.edu/</td>
      <td>https://madisoncollege.edu/disability-resource...</td>
      <td>mcburney.wisc.edu/</td>
    </tr>
    <tr>
      <th>SECTOR</th>
      <td>1</td>
      <td>1</td>
      <td>1</td>
    </tr>
    <tr>
      <th>ICLEVEL</th>
      <td>1</td>
      <td>1</td>
      <td>1</td>
    </tr>
    <tr>
      <th>CONTROL</th>
      <td>1</td>
      <td>1</td>
      <td>1</td>
    </tr>
    <tr>
      <th>HLOFFER</th>
      <td>9</td>
      <td>6</td>
      <td>9</td>
    </tr>
    <tr>
      <th>UGOFFER</th>
      <td>1</td>
      <td>1</td>
      <td>1</td>
    </tr>
    <tr>
      <th>GROFFER</th>
      <td>1</td>
      <td>1</td>
      <td>1</td>
    </tr>
    <tr>
      <th>...</th>
      <td>...</td>
      <td>...</td>
      <td>...</td>
    </tr>
    <tr>
      <th>CYACTIVE</th>
      <td>1</td>
      <td>1</td>
      <td>1</td>
    </tr>
    <tr>
      <th>POSTSEC</th>
      <td>1</td>
      <td>1</td>
      <td>1</td>
    </tr>
    <tr>
      <th>PSEFLAG</th>
      <td>1</td>
      <td>1</td>
      <td>1</td>
    </tr>
    <tr>
      <th>PSET4FLG</th>
      <td>1</td>
      <td>1</td>
      <td>1</td>
    </tr>
    <tr>
      <th>RPTMTH</th>
      <td>1</td>
      <td>1</td>
      <td>1</td>
    </tr>
    <tr>
      <th>INSTCAT</th>
      <td>2</td>
      <td>4</td>
      <td>2</td>
    </tr>
    <tr>
      <th>C15BASIC</th>
      <td>15</td>
      <td>14</td>
      <td>15</td>
    </tr>
    <tr>
      <th>C15IPUG</th>
      <td>14</td>
      <td>5</td>
      <td>11</td>
    </tr>
    <tr>
      <th>C15IPGRD</th>
      <td>14</td>
      <td>0</td>
      <td>14</td>
    </tr>
    <tr>
      <th>C15UGPRF</th>
      <td>15</td>
      <td>5</td>
      <td>14</td>
    </tr>
    <tr>
      <th>C15ENPRF</th>
      <td>4</td>
      <td>3</td>
      <td>5</td>
    </tr>
    <tr>
      <th>C15SZSET</th>
      <td>16</td>
      <td>12</td>
      <td>16</td>
    </tr>
    <tr>
      <th>CCBASIC</th>
      <td>15</td>
      <td>3</td>
      <td>15</td>
    </tr>
    <tr>
      <th>CARNEGIE</th>
      <td>15</td>
      <td>40</td>
      <td>15</td>
    </tr>
    <tr>
      <th>LANDGRNT</th>
      <td>2</td>
      <td>2</td>
      <td>1</td>
    </tr>
    <tr>
      <th>INSTSIZE</th>
      <td>5</td>
      <td>4</td>
      <td>5</td>
    </tr>
    <tr>
      <th>F1SYSTYP</th>
      <td>2</td>
      <td>1</td>
      <td>1</td>
    </tr>
    <tr>
      <th>F1SYSNAM</th>
      <td>...</td>
      <td>Wisconsin Technical College System            ...</td>
      <td>University of Wisconsin System                ...</td>
    </tr>
    <tr>
      <th>F1SYSCOD</th>
      <td>-2</td>
      <td>155020</td>
      <td>155010</td>
    </tr>
    <tr>
      <th>CBSA</th>
      <td>26980</td>
      <td>31540</td>
      <td>31540</td>
    </tr>
    <tr>
      <th>CBSATYPE</th>
      <td>1</td>
      <td>1</td>
      <td>1</td>
    </tr>
    <tr>
      <th>CSA</th>
      <td>168</td>
      <td>357</td>
      <td>357</td>
    </tr>
    <tr>
      <th>NECTA</th>
      <td>-2</td>
      <td>-2</td>
      <td>-2</td>
    </tr>
    <tr>
      <th>COUNTYCD</th>
      <td>19103</td>
      <td>55025</td>
      <td>55025</td>
    </tr>
    <tr>
      <th>COUNTYNM</th>
      <td>Johnson County</td>
      <td>Dane County</td>
      <td>Dane County</td>
    </tr>
    <tr>
      <th>CNGDSTCD</th>
      <td>1902</td>
      <td>5502</td>
      <td>5502</td>
    </tr>
    <tr>
      <th>LONGITUD</th>
      <td>-91.5364</td>
      <td>-89.3279</td>
      <td>-89.4054</td>
    </tr>
    <tr>
      <th>LATITUDE</th>
      <td>41.6619</td>
      <td>43.1218</td>
      <td>43.0739</td>
    </tr>
    <tr>
      <th>DFRCGID</th>
      <td>114</td>
      <td>214</td>
      <td>113</td>
    </tr>
    <tr>
      <th>DFRCUSCG</th>
      <td>1</td>
      <td>1</td>
      <td>1</td>
    </tr>
  </tbody>
</table>
<p>72 rows × 3 columns</p>
</div>




```python
# This example includes a 'Tables16' table which lists the available tables.
data = pd.read_sql('SELECT * FROM Tables16', conn)
```


```python
data
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
      <th>SurveyOrder</th>
      <th>SurveyNumber</th>
      <th>Survey</th>
      <th>YearCoverage</th>
      <th>TableName</th>
      <th>Tablenumber</th>
      <th>TableTitle</th>
      <th>Description</th>
      <th>Release</th>
      <th>Release date</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>10</td>
      <td>1</td>
      <td>Institutional Characteristics</td>
      <td>Academic year 2016-17</td>
      <td>HD2016</td>
      <td>10</td>
      <td>Directory information</td>
      <td>This table contains directory information for ...</td>
      <td>Provisional/final (institutions will not  be a...</td>
      <td>January 2018</td>
    </tr>
    <tr>
      <th>1</th>
      <td>10</td>
      <td>1</td>
      <td>Institutional Characteristics</td>
      <td>Academic year 2016-17</td>
      <td>FLAGS2016</td>
      <td>11</td>
      <td>Response status for all survey components</td>
      <td>This table contains response status informatio...</td>
      <td>Provisional</td>
      <td>January 2018</td>
    </tr>
    <tr>
      <th>2</th>
      <td>10</td>
      <td>1</td>
      <td>Institutional Characteristics</td>
      <td>Academic year 2016-17</td>
      <td>IC2016</td>
      <td>12</td>
      <td>Educational offerings, organization, admission...</td>
      <td>This table contains data on program and award ...</td>
      <td>Provisional</td>
      <td>January 2018</td>
    </tr>
    <tr>
      <th>3</th>
      <td>10</td>
      <td>1</td>
      <td>Institutional Characteristics</td>
      <td>Academic year 2016-17</td>
      <td>IC2016_AY</td>
      <td>13</td>
      <td>Student charges for academic year programs</td>
      <td>This table contains data on student charges fo...</td>
      <td>Provisional/final (institutions will not  be a...</td>
      <td>January 2018</td>
    </tr>
    <tr>
      <th>4</th>
      <td>10</td>
      <td>1</td>
      <td>Institutional Characteristics</td>
      <td>Academic year 2016-17</td>
      <td>IC2016_PY</td>
      <td>14</td>
      <td>Student charges by program (vocational programs)</td>
      <td>This table contains data on student charges by...</td>
      <td>Provisional/final (institutions will not  be a...</td>
      <td>January 2018</td>
    </tr>
    <tr>
      <th>5</th>
      <td>10</td>
      <td>1</td>
      <td>Institutional Characteristics</td>
      <td>Academic year 2016-17</td>
      <td>DRVIC2016</td>
      <td>15</td>
      <td>Frequently used derived variables (IC): Total ...</td>
      <td>This table contains derived variables for tota...</td>
      <td>Provisional (institutions will not  be allowed...</td>
      <td>January 2018</td>
    </tr>
    <tr>
      <th>6</th>
      <td>10</td>
      <td>1</td>
      <td>Institutional Characteristics</td>
      <td>Academic year 2016-17</td>
      <td>IC2016MISSION</td>
      <td>16</td>
      <td>Mission statement</td>
      <td>This table contains institution's mission stat...</td>
      <td>Provisional</td>
      <td>January 2018</td>
    </tr>
    <tr>
      <th>7</th>
      <td>10</td>
      <td>1</td>
      <td>Institutional Characteristics</td>
      <td>Academic year 2016-17</td>
      <td>CUSTOMCGIDS2016</td>
      <td>17</td>
      <td>Custom comparison groups</td>
      <td>This table contains custom comparison groups s...</td>
      <td>Provisional/final (institutions will not  be a...</td>
      <td>January 2018</td>
    </tr>
    <tr>
      <th>8</th>
      <td>20</td>
      <td>2</td>
      <td>Fall Enrollment</td>
      <td>Fall 2016</td>
      <td>EF2016</td>
      <td>20</td>
      <td>Gender, attendance status, and level of studen...</td>
      <td>This table contains the number of students enr...</td>
      <td>Provisional</td>
      <td>December 2017</td>
    </tr>
    <tr>
      <th>9</th>
      <td>20</td>
      <td>2</td>
      <td>Fall Enrollment</td>
      <td>Fall 2016</td>
      <td>EF2016A</td>
      <td>21</td>
      <td>Race/ethnicity, gender, attendance status, and...</td>
      <td>This table contains the number of students enr...</td>
      <td>Provisional</td>
      <td>December 2017</td>
    </tr>
    <tr>
      <th>10</th>
      <td>20</td>
      <td>2</td>
      <td>Fall Enrollment</td>
      <td>Fall 2016</td>
      <td>EF2016CP</td>
      <td>22</td>
      <td>Major field of study, race/ethnicity, gender, ...</td>
      <td>This table contains the number of students enr...</td>
      <td>Provisional</td>
      <td>December 2017</td>
    </tr>
    <tr>
      <th>11</th>
      <td>20</td>
      <td>2</td>
      <td>Fall Enrollment</td>
      <td>Fall 2016</td>
      <td>EF2016B</td>
      <td>23</td>
      <td>Age category, gender, attendance status, and l...</td>
      <td>This table contains the number of students enr...</td>
      <td>Provisional</td>
      <td>December 2017</td>
    </tr>
    <tr>
      <th>12</th>
      <td>20</td>
      <td>2</td>
      <td>Fall Enrollment</td>
      <td>Fall 2016</td>
      <td>EF2016C</td>
      <td>24</td>
      <td>Residence and migration of first-time freshman...</td>
      <td>This table contains the number of first-time f...</td>
      <td>Provisional</td>
      <td>December 2017</td>
    </tr>
    <tr>
      <th>13</th>
      <td>20</td>
      <td>2</td>
      <td>Fall Enrollment</td>
      <td>Fall 2016</td>
      <td>EF2016D</td>
      <td>25</td>
      <td>Total entering class, retention rates, and stu...</td>
      <td>This table contains data on the total entering...</td>
      <td>Provisional</td>
      <td>December 2017</td>
    </tr>
    <tr>
      <th>14</th>
      <td>20</td>
      <td>2</td>
      <td>Fall Enrollment</td>
      <td>Fall 2016</td>
      <td>EF2016A_DIST</td>
      <td>26</td>
      <td>Distance education status and level of student...</td>
      <td>This table contains the number of students enr...</td>
      <td>Provisional</td>
      <td>December 2017</td>
    </tr>
    <tr>
      <th>15</th>
      <td>20</td>
      <td>2</td>
      <td>Fall Enrollment</td>
      <td>Fall 2016</td>
      <td>DRVEF2016</td>
      <td>27</td>
      <td>Frequently used derived variables (EF): Fall e...</td>
      <td>Table includes total full- and part-time enrol...</td>
      <td>Provisional</td>
      <td>December 2017</td>
    </tr>
    <tr>
      <th>16</th>
      <td>30</td>
      <td>3</td>
      <td>Completions</td>
      <td>July 1, 2015 - June 30, 2016</td>
      <td>C2016_A</td>
      <td>30</td>
      <td>Awards/degrees conferred by program (6-digit C...</td>
      <td>This table contains the number of awards by ty...</td>
      <td>Provisional</td>
      <td>January 2018</td>
    </tr>
    <tr>
      <th>17</th>
      <td>30</td>
      <td>3</td>
      <td>Completions</td>
      <td>July 1, 2015 - June 30, 2016</td>
      <td>C2016_B</td>
      <td>31</td>
      <td>Number of students receiving awards/degrees, b...</td>
      <td>This table contains the number of students who...</td>
      <td>Provisional</td>
      <td>January 2018</td>
    </tr>
    <tr>
      <th>18</th>
      <td>30</td>
      <td>3</td>
      <td>Completions</td>
      <td>July 1, 2015 - June 30, 2016</td>
      <td>C2016_C</td>
      <td>32</td>
      <td>Number of students receiving awards/degrees, b...</td>
      <td>This table contains the number of students rec...</td>
      <td>Provisional</td>
      <td>January 2018</td>
    </tr>
    <tr>
      <th>19</th>
      <td>30</td>
      <td>3</td>
      <td>Completions</td>
      <td>July 1, 2015 - June 30, 2016</td>
      <td>C2016DEP</td>
      <td>33</td>
      <td>Number of programs offered and number of progr...</td>
      <td>This file contains the number of programs offe...</td>
      <td>Provisional</td>
      <td>January 2018</td>
    </tr>
    <tr>
      <th>20</th>
      <td>30</td>
      <td>3</td>
      <td>Completions</td>
      <td>July 1, 2015 - June 30, 2016</td>
      <td>DRVC2016</td>
      <td>34</td>
      <td>Frequently used derived variables (C): Complet...</td>
      <td>Table includes number of degrees or certificat...</td>
      <td>Provisional</td>
      <td>January 2018</td>
    </tr>
    <tr>
      <th>21</th>
      <td>60</td>
      <td>6</td>
      <td>Finance</td>
      <td>Fiscal  year 2016</td>
      <td>F1516_F1A</td>
      <td>60</td>
      <td>Public institutions - GASB 34/35: Fiscal year ...</td>
      <td>This table contains institutional finance data...</td>
      <td>Provisional</td>
      <td>December 2017</td>
    </tr>
    <tr>
      <th>22</th>
      <td>60</td>
      <td>6</td>
      <td>Finance</td>
      <td>Fiscal  year 2016</td>
      <td>F1516_F2</td>
      <td>61</td>
      <td>Private not-for-profit institutions or Public ...</td>
      <td>This table contains institutional finance data...</td>
      <td>Provisional</td>
      <td>December 2017</td>
    </tr>
    <tr>
      <th>23</th>
      <td>60</td>
      <td>6</td>
      <td>Finance</td>
      <td>Fiscal  year 2016</td>
      <td>F1516_F3</td>
      <td>62</td>
      <td>Private for-profit institutions: Fiscal year 2...</td>
      <td>This table contains institutional finance data...</td>
      <td>Provisional</td>
      <td>December 2017</td>
    </tr>
    <tr>
      <th>24</th>
      <td>60</td>
      <td>6</td>
      <td>Finance</td>
      <td>Fiscal  year 2016</td>
      <td>DRVF2016</td>
      <td>63</td>
      <td>Frequently used/derived variables Finance (F):...</td>
      <td>Table includes the following financial indicat...</td>
      <td>Provisional</td>
      <td>December 2017</td>
    </tr>
    <tr>
      <th>25</th>
      <td>40</td>
      <td>7</td>
      <td>Student Financial Aid</td>
      <td>July 1, 2015 - June 30, 2016</td>
      <td>SFA1516_P1</td>
      <td>70</td>
      <td>Student financial aid: 2015-16</td>
      <td>This table contains data on the number of full...</td>
      <td>Provisional</td>
      <td>December 2017</td>
    </tr>
    <tr>
      <th>26</th>
      <td>40</td>
      <td>7</td>
      <td>Student Financial Aid</td>
      <td>July 1, 2015 - June 30, 2016</td>
      <td>SFA1516_P2</td>
      <td>71</td>
      <td>Student financial aid and net price: 2015-16</td>
      <td>This table contains the average net price at e...</td>
      <td>Provisional</td>
      <td>December 2017</td>
    </tr>
    <tr>
      <th>27</th>
      <td>40</td>
      <td>7</td>
      <td>Student Financial Aid</td>
      <td>July 1, 2015 - June 30, 2016</td>
      <td>SFAV1516</td>
      <td>72</td>
      <td>Military Servicemembers and Veteran's Benefits...</td>
      <td>This file contains data for students receiving...</td>
      <td>Provisional</td>
      <td>December 2017</td>
    </tr>
    <tr>
      <th>28</th>
      <td>50</td>
      <td>8</td>
      <td>Graduation Rates</td>
      <td>Status of student as of August 31, 2016</td>
      <td>GR2016</td>
      <td>80</td>
      <td>Graduation rate data, 150% of normal time to c...</td>
      <td>This table contains the graduation rate status...</td>
      <td>Provisional</td>
      <td>December 2017</td>
    </tr>
    <tr>
      <th>29</th>
      <td>50</td>
      <td>8</td>
      <td>Graduation Rates</td>
      <td>Status of student as of August 31, 2016</td>
      <td>GR2016_L2</td>
      <td>81</td>
      <td>Graduation rate data, 150% of normal time to c...</td>
      <td>This table contains the graduation rate status...</td>
      <td>Provisional</td>
      <td>December 2017</td>
    </tr>
    <tr>
      <th>30</th>
      <td>50</td>
      <td>8</td>
      <td>Graduation Rates</td>
      <td>Status of student as of August 31, 2016</td>
      <td>GR2016_PELL_SSL</td>
      <td>82</td>
      <td>Graduation rate data for Pell Grant and Subsid...</td>
      <td>This file contains the graduation rate status ...</td>
      <td>Provisional</td>
      <td>December 2017</td>
    </tr>
    <tr>
      <th>31</th>
      <td>50</td>
      <td>8</td>
      <td>Graduation Rates</td>
      <td>Status of student as of August 31, 2016</td>
      <td>GR200_16</td>
      <td>83</td>
      <td>Graduation rate data, 200% of normal time to c...</td>
      <td>This table contains the graduation rate status...</td>
      <td>Provisional</td>
      <td>December 2017</td>
    </tr>
    <tr>
      <th>32</th>
      <td>50</td>
      <td>8</td>
      <td>Graduation Rates</td>
      <td>Status of student as of August 31, 2016</td>
      <td>DRVGR2016</td>
      <td>84</td>
      <td>Frequently used derived variables (GR) 150% of...</td>
      <td>Table contains the graduation rates derived fr...</td>
      <td>Provisional</td>
      <td>December 2017</td>
    </tr>
    <tr>
      <th>33</th>
      <td>90</td>
      <td>9</td>
      <td>Human Resources</td>
      <td>Fall 2016</td>
      <td>EAP2016</td>
      <td>90</td>
      <td>Number of staff by occupational category, facu...</td>
      <td>This table contains the number of staff classi...</td>
      <td>Provisional</td>
      <td>December 2017</td>
    </tr>
    <tr>
      <th>34</th>
      <td>90</td>
      <td>9</td>
      <td>Human Resources</td>
      <td>2016-17</td>
      <td>SAL2016_IS</td>
      <td>91</td>
      <td>Number and salary outlays for full-time nonmed...</td>
      <td>This table contains the number of staff,  tota...</td>
      <td>Provisional</td>
      <td>December 2017</td>
    </tr>
    <tr>
      <th>35</th>
      <td>90</td>
      <td>9</td>
      <td>Human Resources</td>
      <td>2016-17</td>
      <td>SAL2016_NIS</td>
      <td>92</td>
      <td>Number and salary outlays for full-time nonmed...</td>
      <td>This table contains the number and salary outl...</td>
      <td>Provisional</td>
      <td>December 2017</td>
    </tr>
    <tr>
      <th>36</th>
      <td>90</td>
      <td>9</td>
      <td>Human Resources</td>
      <td>Fall 2016</td>
      <td>S2016_OC</td>
      <td>93</td>
      <td>Full- and part-time staff by occupational cate...</td>
      <td>This table contains the number of staff on the...</td>
      <td>Provisional</td>
      <td>December 2017</td>
    </tr>
    <tr>
      <th>37</th>
      <td>90</td>
      <td>9</td>
      <td>Human Resources</td>
      <td>Fall 2016</td>
      <td>S2016_SIS</td>
      <td>94</td>
      <td>Full-time instructional staff, by academic ran...</td>
      <td>This table contains the number of full-time in...</td>
      <td>Provisional</td>
      <td>December 2017</td>
    </tr>
    <tr>
      <th>38</th>
      <td>90</td>
      <td>9</td>
      <td>Human Resources</td>
      <td>Fall 2016</td>
      <td>S2016_IS</td>
      <td>95</td>
      <td>Full-time instructional staff, by faculty and ...</td>
      <td>This table contains the number of full-time in...</td>
      <td>Provisional</td>
      <td>December 2017</td>
    </tr>
    <tr>
      <th>39</th>
      <td>90</td>
      <td>9</td>
      <td>Human Resources</td>
      <td>Fall 2016</td>
      <td>S2016_NH</td>
      <td>96</td>
      <td>New hires by occupational category, race/ethni...</td>
      <td>This table contains the number of full-time ne...</td>
      <td>Provisional</td>
      <td>December 2017</td>
    </tr>
    <tr>
      <th>40</th>
      <td>90</td>
      <td>9</td>
      <td>Human Resources</td>
      <td>Fall 2016</td>
      <td>DRVHR2016</td>
      <td>97</td>
      <td>Frequently used/derived variables Human resour...</td>
      <td>Table contains average salaries equated to 9-m...</td>
      <td>Provisional</td>
      <td>December 2017</td>
    </tr>
    <tr>
      <th>41</th>
      <td>12</td>
      <td>12</td>
      <td>Admissions</td>
      <td>Academic year Fall 2016</td>
      <td>ADM2016</td>
      <td>120</td>
      <td>Admission considerations, applicants, admissio...</td>
      <td>This table contains information about the unde...</td>
      <td>Provisional</td>
      <td>December 2017</td>
    </tr>
    <tr>
      <th>42</th>
      <td>12</td>
      <td>12</td>
      <td>Admissions</td>
      <td>Academic year Fall 2016</td>
      <td>DRVADM2016</td>
      <td>121</td>
      <td>Frequently used derived variables for admissio...</td>
      <td>This table contains derived variables using th...</td>
      <td>Provisional</td>
      <td>December 2017</td>
    </tr>
    <tr>
      <th>43</th>
      <td>14</td>
      <td>14</td>
      <td>12-month Enrollment</td>
      <td>July 1, 2015 - June 30, 2016</td>
      <td>EFFY2016</td>
      <td>140</td>
      <td>12-month unduplicated headcount: 2015-16</td>
      <td>This table contains the unduplicated head coun...</td>
      <td>Provisional</td>
      <td>January 2018</td>
    </tr>
    <tr>
      <th>44</th>
      <td>14</td>
      <td>14</td>
      <td>12-month Enrollment</td>
      <td>July 1, 2015 - June 30, 2016</td>
      <td>EFIA2016</td>
      <td>141</td>
      <td>12-month instructional activity: 2015-16</td>
      <td>This table contains data on instructional acti...</td>
      <td>Provisional</td>
      <td>January 2018</td>
    </tr>
    <tr>
      <th>45</th>
      <td>14</td>
      <td>14</td>
      <td>12-month Enrollment</td>
      <td>July 1, 2015 - June 30, 2016</td>
      <td>DRVEF122016</td>
      <td>142</td>
      <td>Frequently used derived variables (E12): 12-mo...</td>
      <td>This table contains the unduplicated head coun...</td>
      <td>Provisional</td>
      <td>January 2018</td>
    </tr>
    <tr>
      <th>46</th>
      <td>100</td>
      <td>16</td>
      <td>Academic Libraries</td>
      <td>2016-17</td>
      <td>AL2016</td>
      <td>160</td>
      <td>Academic Libraries, 2015-16</td>
      <td>The Academic Library survey became part of the...</td>
      <td>Provisional</td>
      <td>December 2017</td>
    </tr>
    <tr>
      <th>47</th>
      <td>100</td>
      <td>16</td>
      <td>Academic Libraries</td>
      <td>2016-17</td>
      <td>DRVAL2016</td>
      <td>161</td>
      <td>Frequently used/derived variables Academic lib...</td>
      <td>Table contains the percent distribution of lib...</td>
      <td>Provisional</td>
      <td>December 2017</td>
    </tr>
    <tr>
      <th>48</th>
      <td>55</td>
      <td>18</td>
      <td>Outcome Measures</td>
      <td>Status of student as of August 31, 2016</td>
      <td>OM2016</td>
      <td>181</td>
      <td>Award and enrollment data at six and eight yea...</td>
      <td>This table contains award and enrollment data ...</td>
      <td>Provisional</td>
      <td>December 2017</td>
    </tr>
    <tr>
      <th>49</th>
      <td>55</td>
      <td>18</td>
      <td>Outcome Measures</td>
      <td>Status of student as of August 31, 2016</td>
      <td>DRVOM2016</td>
      <td>182</td>
      <td>Frequently used derived variables (OM) Award a...</td>
      <td>This table contains award and enrollment rates...</td>
      <td>Provisional</td>
      <td>None</td>
    </tr>
  </tbody>
</table>
</div>


