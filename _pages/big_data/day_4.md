---
layout: archive
permalink: /Big_Data/big_data/day_4
title: "Practice Data Page"
author_profile: true

header:
  image: "/images/SQL/citi_bike.jpeg"
  
---

So you want to create a DB using an xlxs file, Python, and SQLite??


<a href="/notes/SQL/citi.pdf" class="btn btn--success">SQL CitiBike Notes</a>



[I took a csv file from here!](https://s3.amazonaws.com/tripdata/index.html)

1. Lets First Create the DataBase

We're going to use the columns in the csv file and name the Table, CitiBikes

```python
import sqlite3, csv

conn = sqlite3.connect('citi.db')
cur = conn.cursor()


sql = """
    CREATE TABLE CitiBikes (
        Trip_Duration INTEGER,
        StartTime TEXT,
        StopTime TEXT,
        StartStationID INTEGER,
        StartStationName TEXT,
        StartStationLat DECIMAL,
        StartStationLong DECIMAL,
        EndStationID INTEGER,
        EndStationName TEXT,
        EndStationLat DECIMAL,
        EndStationlog DECIMAL,
        t INTEGAR,
        Usertype TEXT,
        Birthyear TEXT,
        Gender INTEGER
        
        )
    """
    
cur.execute(sql)
print('Database has been created')

conn.commit()
conn.close()
    
```

Now we have sucessfully created a Database named citi.db

Now let import our csv file and the columns into our newly created database.


```python
import sqlite3, csv, pandas

connection  = sqlite3.connect('citi.db')
cursor = connection.cursor()

with open('citibike.csv', 'r') as file:
    no_records = 0
    for row in file:
        cursor.execute("INSERT INTO CitiBikes VALUES (?,?,?,?,?,?,?,?,?,?,?,?,?,?,?)", row.split(","))
        
        connection.commit()
        no_records +=1
        
connection.close()
print('\n{} Records Transferred'.format(no_records))

print("DONE!")


```


Next time we can do some simple data analysis!

Lets import some libraries

```python
import sqlite3, pandas

import matplotlib.pyplot as plt
```

Lets connect to our Database:
```python
db = sqlite3.connect('citi.db')
```

Lets Execute some Simple Queries SQLite using Pandas

```python

pandas.read_sql_query("SELECT COUNT (*) FROM CitiBikes", db)
```
-> this will return the number of rows from our table CitiBikes

Example of output:
```python
	COUNT (*)
0	1155406

```

Lets SELECT one column from our table:

```python
pandas.read_sql_query("SELECT Trip_Duration FROM CitiBikes", db)
```

Example of potential output:

```python
	Trip_Duration
0	695
1	693
2	2059
3	123
4	1521
...	...
1155401	925
1155402	279
1155403	161
1155404	909
1155405	634

1155406 rows × 1 columns


```

Lets Calculate the average Trip Duration (in seconds)

```python
pandas.read_sql_query("SELECT avg(Trip_Duration) from CitiBikes", db)
```

Example Output:

```python
	AVG(Trip_Duration)
    
0	1372.570279
```

The average Trip Duration for this dataset is 22.86 minutes!

Lets add a helper function to make it easier on us when executing Queries

```python
def Q(sql):
    res = pandas.read_sql_query(sql, db, chunksize = 100_000)
    return next(res)
```

The chunksize is the number of rows we exectute at a time, the next(res) calls for the next chunksize, so 100,000 rows at a time!

```python
res = Q("SELECT * FROM CitiBikes")
```

```python
res
```

Awesome, now heres our output, were working with 15 Columns and 100,000 rows at a time!

```python
	Trip_Duration	StartTime	StopTime	StartStationID	StartStationName	StartStationLat	StartStationLong	EndStationID	EndStationName	EndStationLat	EndStationlog	t	Usertype	Birthyear	Gender
0	695	"2013-06-01 00:00:01"	"2013-06-01 00:11:36"	444	"Broadway & W 24 St"	40.742354	-73.989151	434	"9 Ave & W 18 St"	40.7432	-74.0037	19678	"Subscriber"	1983	1
1	693	"2013-06-01 00:00:08"	"2013-06-01 00:11:41"	444	"Broadway & W 24 St"	40.742354	-73.989151	434	"9 Ave & W 18 St"	40.7432	-74.0037	16649	"Subscriber"	1984	1
2	2059	"2013-06-01 00:00:44"	"2013-06-01 00:35:03"	406	"Hicks St & Montague St"	40.695128	-73.995951	406	"Hicks St & Montague St"	40.6951	-73.996	19599	"Customer"	NULL	0
3	123	"2013-06-01 00:01:04"	"2013-06-01 00:03:07"	475	"E 15 St & Irving Pl"	40.735243	-73.987586	262	"Washington Park"	40.6918	-73.9737	16352	"Subscriber"	1960	1
4	1521	"2013-06-01 00:01:22"	"2013-06-01 00:26:43"	2008	"Little West St & 1 Pl"	40.705693	-74.016777	310	"State St & Smith St"	40.6893	-73.9891	15567	"Subscriber"	1983	1
...	...	...	...	...	...	...	...	...	...	...	...	...	...	...	...
99995	312	"2013-06-09 12:12:18"	"2013-06-09 12:17:30"	476	"E 31 St & 3 Ave"	40.743943	-73.979661	509	"9 Ave & W 22 St"	40.7455	-74.002	15509	"Subscriber"	1984	1
99996	1177	"2013-06-09 12:12:24"	"2013-06-09 12:32:01"	445	"E 10 St & Avenue A"	40.727408	-73.981420	477	"W 41 St & 8 Ave"	40.7564	-73.99	20242	"Subscriber"	1964	1
99997	2110	"2013-06-09 12:12:24"	"2013-06-09 12:47:34"	426	"West St & Chambers St"	40.717548	-74.013221	404	"9 Ave & W 14 St"	40.7406	-74.0055	17437	"Subscriber"	1996	1
99998	1058	"2013-06-09 12:12:25"	"2013-06-09 12:30:03"	521	"8 Ave & W 31 St N"	40.750967	-73.994442	428	"E 3 St & 1 Ave"	40.7247	-73.9878	15407	"Subscriber"	1978	1
99999	1870	"2013-06-09 12:12:29"	"2013-06-09 12:43:39"	419	"Carlton Ave & Park Ave"	40.695807	-73.973556	502	"Henry St & Grand St"	40.7142	-73.9813	17989	"Customer"	NULL	0
100000 rows × 15 columns

```


### More Simple SQL Queries

Lets check what Columns are in our Table (CitiBikes)

```python
res.columns
```
Output:

```python
Index(['Trip_Duration', 'StartTime', 'StopTime', 'StartStationID',
       'StartStationName', 'StartStationLat', 'StartStationLong',
       'EndStationID', 'EndStationName', 'EndStationLat', 'EndStationlog', 't',
       'Usertype', 'Birthyear', 'Gender'],
      dtype='object')
```

Ok, lets see what Gender is riding the Bikes
- 0: Unknown Gender
- 1: Male
- 2: Female

```python
res = Q("SELECT Gender, COUNT (*) FROM CitiBikes GROUP BY Gender")
```
```python
res
```

Output:

```python
	Gender	COUNT(*)
0	0	481496
1	1	526984
2	2	146926
```

Lets look at the User Type, which is seperated into:

Customer = 24-hour pass or 3-day pass user (tourist)

Subscriber = Annual Member

Lets Group them by UserType and Gender!

```python
res = Q("SELECT gender, UserType, COUNT(*) from CitiBikes GROUP BY Gender, UserType ")

```
Output
```python
	Gender	UserType	COUNT(*)
0	0	"Customer"	    480638
1	0	"Subscriber"	858
2	1	"Subscriber"	526984
3	2	"Subscriber"	146926

```

Ok now that we have the hang of it, lets do some more complex Queries!

Lets find the Ages of the Bike riders!

```python
res = Q("""
 SELECT
(2018 - Birthyear) as Age,
COUNT(*) 
FROM CitiBikes
GROUP BY Age

""")
```

```python
res
```

Output
```python
	Age	COUNT(*)
0	21	396
1	22	536
2	23	852
3	24	638
4	25	1504
...	...	...
72	105	26
73	117	28
74	118	166
75	119	66
76	2018	480642
77 rows × 2 columns
```

Lets see how many 40-year olds are riding bikes!

```python
res = Q("""
 SELECT
(2018 - Birthyear) as Age,
COUNT(*)
FROM CitiBikes
GROUP BY Age
having Age = 40

""")
```

```python
res
```

Output:

```python
	Age	COUNT(*)
0	40	24706
```


Lets see if we can add Gender and see number of 40-year olds are riding Bikes

```python
res = Q("""
SELECT
Gender,(2018 - Birthyear) as Age, 
COUNT(*)
FROM CitiBikes
GROUP BY Gender, Age
having Age = 40

""")
```

```python
res
```

Output:
```python
	Gender	Age	COUNT(*)
0	1	40	20126
1	2	40	4580

```

Lets look at some other Keywords (Order By)

Lets look at the Bike_Id (t) and the Trip_duration (in hours) to see the sum of each Bike used!
GROUP BY -> The same Bike_Id (t)

ORDER BY is used to sort the result-set in ascending (ASC) or descending (DESC) order.

Descending Order: High -> Low
Ascending Order:  Low -> High

```python
res = Q("""

SELECT t, sum(Trip_Duration)/3600
FROM CitiBikes
GROUP BY t
ORDER BY t DESC;

""")
```

```python
res
```

Ouput:

```python
	    t	sum(Trip_Duration)/3600
0	    20625	21
1	    20624	18
2	    20623	23
3	    20622	27
4	    20621	8
...	...	...
5789	14534	115
5790	14533	64
5791	14532	73
5792	14531	95
5793	14529	59
5794 rows × 2 columns
```


Lets look at Ascending Bike_Id order example:

```python
res = Q("""

SELECT t, sum(Trip_Duration) /3600
FROM CitiBikes
GROUP BY t
ORDER BY t ASC;

""")
```
```python
res
```

Output:

```python
	    t	sum(Trip_Duration) /3600
0	    14529	59
1	    14531	95
2   	14532	73
3	    14533	64
4	    14534	115
...	...	...
5789	20621	8
5790	20622	27
5791	20623	23
5792	20624	18
5793	20625	21
5794 rows × 2 columns

```


Lets work on some more Queries so we can prepare to Graph! (histogram)

```python
res = Q (
"""
SELECT t, sum(Trip_Duration) / 3600 as hours, round(sum(Trip_Duration) / 3600 / 100) * 100 as hour_bucket
FROM CitiBikes
GROUP BY t
ORDER By hours DESC

LIMIT 12
"""
)

```

```python
res
```

Output:

```python
	        t	hours	hour_bucket

    0	15259	2165	2100.0
    1	17918	1524	1500.0
    2	19866	1058	1000.0
    3	17215	782	    700.0
    4	17806	741	    700.0
    5	17917	619	    600.0
    6	18152	569	    500.0
    7	15043	492	    400.0
    8	19755	416	    400.0
    9	19010	382	    300.0
    10	17282	381	    300.0
    11	15948	380	    300.0


```


even more complex... (preparing for histogram)


```python
res = Q ("""

SELECT hour_bucket, COUNT(*) from
(
SELECT t, sum(Trip_Duration) / 3600 as hours, round(sum(Trip_Duration) / 3600 / 100) * 100 as hour_bucket
FROM CitiBikes
GROUP BY t
)

GROUP BY hour_bucket

""")
```

```python
res
```


Output:

```python
	hour_bucket	COUNT(*)
0	    0.0	        4727
1	    100.0	    1006
2	    200.0	    40
3	    300.0	    12
4	    400.0	    2
5	    500.0	    1
6	    600.0	    1
7	    700.0	    2
8	    1000.0	    1
9	    1500.0	    1
10	    2100.0	    1

```


Lets Work on Cleaning our Table a little Bit!

Lets create a new table from our existing table and use some Columns to make "cleaner Columns" and such

New table name: Citi_Bike_Clean

New Column: Age and Sex (X,M, or F)

From our Citibikes table and when Age is > 0 and Age < 80 and Trip_Duration < 6000

```python
db.execute("""
CREATE TABLE
    Citi_Bike_Clean AS
SELECT
    (2018 - Birthyear) AS age,
    CASE WHEN gender = 0 THEN "X"
         WHEN gender = 1 THEN "M"
         WHEN gender = 2 THEN "F"
    END as sex,

*
FROM CitiBikes
WHERE age > 0
  AND age <80
  AND Trip_Duration < 6000
  
    
""")
```
Output
```python
<sqlite3.Cursor at 0x7f9294c12f80>

```

Lets check out our new table that we made!

```python
res = Q("SELECT * FROM Citi_Bike_Clean")
```
```python
res
```

Output:

```python
	age	sex	Trip_Duration	StartTime	StopTime	StartStationID	StartStationName	StartStationLat	StartStationLong	EndStationID	EndStationName	EndStationLat	EndStationlog	BikeId	Usertype	Birthyear	Gender
0	23	M	1404	"2020-02-01 00:00:05.9460"	"2020-02-01 00:23:30.7240"	316	"Fulton St & William St"	40.709560	-74.006536	481	"S 3 St & Bedford Ave"	40.712605	-73.962644	28874	"Customer"	1995	1
1	27	M	1301	"2020-02-01 00:00:06.2230"	"2020-02-01 00:21:48.0580"	237	"E 11 St & 2 Ave"	40.730473	-73.986724	539	"Metropolitan Ave & Bedford Ave"	40.715348	-73.960241	32588	"Subscriber"	1991	1
2	24	M	474	"2020-02-01 00:00:15.7210"	"2020-02-01 00:08:10.3440"	528	"2 Ave & E 31 St"	40.742909	-73.977061	3785	"W 42 St & 6 Ave"	40.754920	-73.984550	41013	"Subscriber"	1994	1
3	45	F	487	"2020-02-01 00:00:21.0520"	"2020-02-01 00:08:28.7520"	380	"W 4 St & 7 Ave S"	40.734011	-74.002939	3263	"Cooper Square & Astor Pl"	40.729515	-73.990753	27581	"Subscriber"	1973	2
4	24	M	619	"2020-02-01 00:00:27.4000"	"2020-02-01 00:10:47.0640"	472	"E 32 St & Park Ave"	40.745712	-73.981948	237	"E 11 St & 2 Ave"	40.730473	-73.986724	29062	"Subscriber"	1994	1
...	...	...	...	...	...	...	...	...	...	...	...	...	...	...	...	...	...
99995	48	M	215	"2020-02-03 17:40:13.0600"	"2020-02-03 17:43:48.1790"	3783	"Cliff St & Fulton St"	40.708380	-74.004950	319	"Fulton St & Broadway"	40.711066	-74.009447	34144	"Subscriber"	1970	1
99996	43	M	459	"2020-02-03 17:40:13.1930"	"2020-02-03 17:47:52.2710"	513	"W 56 St & 10 Ave"	40.768254	-73.988639	3809	"W 55 St & 6 Ave"	40.763189	-73.978434	30136	"Subscriber"	1975	1
99997	19	F	244	"2020-02-03 17:40:13.4040"	"2020-02-03 17:44:18.2250"	426	"West St & Chambers St"	40.717548	-74.013221	328	"Watts St & Greenwich St"	40.724055	-74.009660	21171	"Subscriber"	1999	2
99998	25	F	238	"2020-02-03 17:40:13.6950"	"2020-02-03 17:44:11.9610"	3126	"44 Dr & Jackson Ave"	40.747182	-73.943264	3123	"31 St & Thomson Ave"	40.744697	-73.935404	35312	"Subscriber"	1993	2
99999	49	F	148	"2020-02-03 17:40:13.9880"	"2020-02-03 17:42:42.9180"	3263	"Cooper Square & Astor Pl"	40.729515	-73.990753	403	"E 2 St & 2 Ave"	40.725029	-73.990697	41610	"Subscriber"	1969	2
100000 rows × 17 columns

```

And there it is, out new Table: Citi_Bike_Clean with the new Columns: age and sex!

Now lets take our new Table and do some stuff with it!


```python
%pylab inline
```
```python
Populating the interactive namespace from numpy and matplotlib
```

```python
import seaborn
```

Lets Group the Age of the riders:

```python
res = Q(
"""
SELECT age, COUNT(*) AS c
FROM Citi_Bike_Clean
GROUP BY age
""")
```
```
res
```
Output:

```python
	age	c
0	14	9
1	15	1102
2	16	1908
3	17	3428
4	18	7969
...	...	...
61	75	669
62	76	588
63	77	416
64	78	553
65	79	105
66 rows × 2 columns

```

Doing the above example with a limit of 25 ( first 25 ages) just to show "Cleaner" table
```python
res = Q(
"""
SELECT age, COUNT(*) AS c
FROM Citi_Bike_Clean
GROUP BY age
LIMIT 25
""")

res
```

Output
```python
	age	c
0	14	9
1	15	1102
2	16	1908
3	17	3428
4	18	7969
5	19	11081
6	20	11124
7	21	15293
8	22	23756
9	23	33952
10	24	37243
11	25	39164
12	26	43190
13	27	44856
14	28	45525
15	29	45658
16	30	42212
17	31	37426
18	32	35366
19	33	35985
20	34	32470
21	35	32266
22	36	28700
23	37	26938
24	38	26511

```
Look its from Age 14 to Age 38!


Lets make a Histogram !

```python
hist(res.age, weights=res.c, bins=len(res))
```

Output:

![inserting an Image](/images/SQL/hist.jpg)


```python
res = Q("""
SELECT
    age,
    sum( CASE WHEN sex="F" then 1 end) as "F",
    sum( CASE WHEN sex="M" then 1 end) as "M"
from 
 Citi_Bike_Clean
GROUP BY 1
""")
```

```python
res.head()
```

Output:
```python

	age	F	    M
0	14	3	    6
1	15	151	    951
2	16	284 	1618
3	17	570	    2817
4	18	1406	6404

```

Lets Plot!!!!


```python
figure(figsize=(5,12))
barh(res.age, res.F/res.F.sum(), color='r', label='female')
barh(res.age, -res.M/res.M.sum(), color ='b', label = 'male')
title('Age Distribution')
ylabel("Age [Years]")
grid()
legend()

```

Output:

![inserting an Image](/images/SQL/hist2.jpg)

Lets see how long the trips are (in minutes ) by Gender

more...

```python
res = Q("""
SELECT
    Trip_Duration / 60 AS 'd',
    sum (case when sex = "F" then 1 end) AS "F",
    sum (case when sex = "M" then 1 end) AS "M"
FROM
    Citi_Bike_Clean
GROUP BY 1

    
""")

```

```python
res
```

Output:
```python
    Duration	    F	M
0	1	    3720	20979
1	2	    10005	48018
2	3	    15488	64807
3	4	    18645	71395
4	5	    19424	71060
5	6	    19087	66893
6	7	    18071	59863
7	8	    16155	52887
8	9	    14453	45587
9	10  	12956	39497
10	11	    11346	33745
11	12  	9970	29035
12	13  	8920	25128
13	14	    7893	22126
14	15  	6920	19160
15	16  	6208	16653
16	17  	5451	14469
17	18  	4931	12704
18	19	    4325	11520
19	20  	3982	10436
20	21  	3579	9258
21	22  	3244	8451
22	23  	2857	7648
23	24  	2645	6924
24	25  	2469	6451

```

Lets Graph!


```python
figure(figsize=(5,12))
barh(res.Duration, res.F/res.F.sum(), color='r', label='Female')
barh(res.Duration, -res.M/res.M.sum(), color ='b', label = 'Male')
title('Duration distribution')
ylabel("Duration [Minutes]")
grid()
legend()

```

Output:
![inserting an Image](/images/SQL/hist3.jpg)


Lets Look at the Stations

```python
res = Q("""
SELECT
    StartStationName,
    StartStationLat AS Lat,
    StartStationLong AS Long,
    COUNT (*) as C
FROM
    Citi_Bike_Clean

GROUP by 1,2,3

```

```python
res.head()
```

```python
	StartStationName	Lat	Long	C
0	"1 Ave & E 110 St"	40.792327	-73.938300	619
1	"1 Ave & E 16 St"	40.732219	-73.981656	4382
2	"1 Ave & E 18 St"	40.733812	-73.980544	4097
3	"1 Ave & E 30 St"	40.740000	-73.974000	1
4	"1 Ave & E 30 St"	40.741444	-73.975361	2577

```

```python

hist(res.Lat, range=(40.6, 40.9), bins = 100)

```
Output:
![inserting an Image](/images/SQL/hist4.jpg)

```python
hist(res.Long, range = (-74.1, -73.8), bins = 100)
```
Output:
![inserting an Image](/images/SQL/hist5.jpg)

Lets plot Long vs Lat

```python
plot(res.Long, res.Lat, '.')
title('Long vs Lat')
ylabel("Lat")
xlabel("Long")
grid()

```

Output:

![inserting an Image](/images/SQL/lat_long1.jpg)


What does this Graph above look like? Oooo NYC!!!


![inserting an Image](/images/SQL/nyc.jpg)


```python

figure(figsize = (10,10))
xstart = -74.04
ystart = 40.645
extent = .18
scatter(res.Long, res.Lat, c = 35*res.C/res.C.max(), cmap ='rainbow')
xlim(xstart, xstart+extent)
ylim(ystart, ystart+extent)
title('Long vs Lat')
ylabel("Lat")
xlabel("Long")
grid()
colorbar()
```

Output:
![inserting an Image](/images/SQL/lat_long2.jpg)


Lets make this a function:

```python
def my_plot(x, y, s, c, max_size = 50):

    figure(figsize = (10,10))
    xstart = -74.04
    ystart = 40.645
    extent = .18
    scatter(x, y, s = max_size*s/max(s), c=c, cmap ='rainbow')
    xlim(xstart, xstart+extent)

    ylim(ystart, ystart+extent)
    title('Long vs Lat')
    ylabel("Lat")
    xlabel("Long")
    grid()
    colorbar()


```

Lets use this function

```python
my_plot(res.Long, res.Lat, res.C, res.C)
```

![inserting an Image](/images/SQL/lat_long3.jpg)


Lets look at Gender and Location

```python
res = Q(
"""
SELECT

    StartStationName,
    StartStationLong as Long,
    StartStationLat as Lat,
    
    AVG(Trip_Duration) as D,
    
    sum (case when sex ="F" then 1 end) as "F",
    sum (case when sex ="M" then 1 end) as "M"
FROM
    Citi_Bike_Clean

GROUP BY 1,2,3

"""
)
```

```python
res.head()
```

Output:

```python
	StartStationName	Long	Lat	D	F	M
0	"1 Ave & E 110 St"	-73.938300	40.792327	665.381260	131.0	474.0
1	"1 Ave & E 16 St"	-73.981656	40.732219	592.057052	997.0	3198.0
2	"1 Ave & E 18 St"	-73.980544	40.733812	587.435685	1072.0	2898.0
3	"1 Ave & E 30 St"	-73.975361	40.741444	696.627862	540.0	1944.0
4	"1 Ave & E 30 St"	-73.974000	40.740000	237.000000	1.0	NaN

```

Lets Plot!

```python
my_plot (res.Long, res.Lat, res.D, 100*res.M/(res.M+res.F))
```

Ouput:
![inserting an Image](/images/SQL/lat_long4.jpg)

Looking at the image, the more red the color means there is more Males while the bluer the color means more Females


Lets look at Females, their age, and Location

```python
res = Q(
"""
SELECT

    StartStationName,
    StartStationLong as Long,
    StartStationLat as Lat,
        
    AVG (case when sex ="F" then age end) as "F",
    AVG (case when sex ="M" then age end) as "M",
    COUNT (*) as C
FROM
    Citi_Bike_Clean

GROUP BY 1,2,3

"""
)
```


```python
my_plot(res.Long, res.Lat, res.F*1,res.F )
```

![inserting an Image](/images/SQL/lat_long5.jpg)

Red is older, while blue is younger!

