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
> ![query](https://private-user-images.githubusercontent.com/150418764/286857810-51c785ef-989b-445a-88af-2d39bbec46ce.png?jwt=eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpc3MiOiJnaXRodWIuY29tIiwiYXVkIjoicmF3LmdpdGh1YnVzZXJjb250ZW50LmNvbSIsImtleSI6ImtleTEiLCJleHAiOjE3MDEzMzE2MzIsIm5iZiI6MTcwMTMzMTMzMiwicGF0aCI6Ii8xNTA0MTg3NjQvMjg2ODU3ODEwLTUxYzc4NWVmLTk4OWItNDQ1YS04OGFmLTJkMzliYmVjNDZjZS5wbmc_WC1BbXotQWxnb3JpdGhtPUFXUzQtSE1BQy1TSEEyNTYmWC1BbXotQ3JlZGVudGlhbD1BS0lBSVdOSllBWDRDU1ZFSDUzQSUyRjIwMjMxMTMwJTJGdXMtZWFzdC0xJTJGczMlMkZhd3M0X3JlcXVlc3QmWC1BbXotRGF0ZT0yMDIzMTEzMFQwODAyMTJaJlgtQW16LUV4cGlyZXM9MzAwJlgtQW16LVNpZ25hdHVyZT0zYmU3N2YwNWNmMTYxMWMxZmY0M2QxZTJlZmI4MTNmNTVlMzIxMTBiM2JmNWI3YjUyNGM0OWM5MTU5YmM5MmFiJlgtQW16LVNpZ25lZEhlYWRlcnM9aG9zdCZhY3Rvcl9pZD0wJmtleV9pZD0wJnJlcG9faWQ9MCJ9.akaE6FqJ3P0Bm0ZXtTABUzcJUFZLLk4f0vFjyly8cNI)


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
> ![yıllara göre](https://private-user-images.githubusercontent.com/150418764/286858587-3a06d0d8-0c9a-461a-b2ab-c6bcd3614352.png?jwt=eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpc3MiOiJnaXRodWIuY29tIiwiYXVkIjoicmF3LmdpdGh1YnVzZXJjb250ZW50LmNvbSIsImtleSI6ImtleTEiLCJleHAiOjE3MDEzMzE3NjAsIm5iZiI6MTcwMTMzMTQ2MCwicGF0aCI6Ii8xNTA0MTg3NjQvMjg2ODU4NTg3LTNhMDZkMGQ4LTBjOWEtNDYxYS1iMmFiLWM2YmNkMzYxNDM1Mi5wbmc_WC1BbXotQWxnb3JpdGhtPUFXUzQtSE1BQy1TSEEyNTYmWC1BbXotQ3JlZGVudGlhbD1BS0lBSVdOSllBWDRDU1ZFSDUzQSUyRjIwMjMxMTMwJTJGdXMtZWFzdC0xJTJGczMlMkZhd3M0X3JlcXVlc3QmWC1BbXotRGF0ZT0yMDIzMTEzMFQwODA0MjBaJlgtQW16LUV4cGlyZXM9MzAwJlgtQW16LVNpZ25hdHVyZT1lZTAyMTZjMDVlYzZlN2Q5YzE3YmUxY2NiZTg1YjVmYTViMjIxMGU2ZGE5MjFkNWI4MGNjY2ZiNzFhMzc4YmNjJlgtQW16LVNpZ25lZEhlYWRlcnM9aG9zdCZhY3Rvcl9pZD0wJmtleV9pZD0wJnJlcG9faWQ9MCJ9.-8JCRF4fhMWpjHtIhWYMApvwizh2mgZiUjtjneON6ZU)


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

![parking](https://github.com/ipekhisar/Hotel-Booking-Data-Analysis/assets/150418764/19ff8d2e-4d23-4e17-8b8c-75656a7928c0)

