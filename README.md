# Hotel-Booking-Data-Analysis (SQL & PowerBI)
## The aim of the project
Develop a Database to analyze and visualize Hotel Booking Data
Build a dashboard using Power BI to present to your stakeholders.

## Requirements
1-Are our hotels revenue growing yearly?
2-Should we increase our parking lot size?
3-What trends can we see in the data? 

## Steps
### 1- Create a Database
The database is created using the Import Data dialog box from Excel document.

### 2- Querying Data
*Taking Data from Tables*

```
select * from dbo.['2018$']

select * from dbo.['2019$']

select * from dbo.['2020$']
```
https://github.com/ipekhisar/Hotel-Booking-Data-Analysis/issues/4#issue-2018086403

*Combining the Data*

```
select * from dbo.['2018$']

union

select * from dbo.['2019$']

union

select * from dbo.['2020$']
```

*Exploratory Data Analysis*

Temporary Table

```
with hotels as 
(select * from dbo.['2018$']

union

select * from dbo.['2019$']

union

select * from dbo.['2020$'])

select*from hotels

```
![tek tablo](https://github.com/ipekhisar/Hotel-Booking-Data-Analysis/assets/150418764/4adaf019-7432-4985-8087-fb5fc95b4dbb)

**Q1- Is our hotel revenue growing yearly?**

*Creating a new column revenue by using the data*

```
select*, ((stays_in_week_nights+stays_in_weekend_nights)*adr) Revenue from hotels
```

*Calculate the sum of revenue while grouping the data by year.*

```
select arrival_date_year, sum((stays_in_week_nights+stays_in_weekend_nights)*adr) Revenue from hotels

Group by arrival_date_year
```

***We can see that the revenue increased from 2018 to 2019 but then decreased again in 2020.***

*Determining the revenue trend by hotel type by grouping the data by hotel and then seeing which hotels have generated the most revenue.*
```
select arrival_date_year, hotel,
sum((stays_in_week_nights + stays_in_weekend_nights) * adr)
as revenue from hotels group by arrival_date_year, hotel
```

**Q2-Should we increase our parking lot size?**

*To answer this question, we will focus on the car_parking_spaces and number of guests staying in the hotel.* 

```
select arrival_date_year, hotel , sum((stays_in_week_nights+stays_in_weekend_nights)*adr) Revenue,
concat (round((sum(required_car_parking_spaces)/sum(stays_in_week_nights +
stays_in_weekend_nights)) * 100, 2), '%') as parking_percentage from hotels
GRoup by arrival_date_year, hotel
```
***So, there is no need to increase our parking lot size.***


