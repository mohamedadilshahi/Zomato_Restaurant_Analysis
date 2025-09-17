# Zomato_Restaurant_Analysis

## Project Overview

**Project Title**: Zomato_Restaurant_Analysis

This project is designed to demonstrate SQL skills and data analysis techniques applied to the Zomato restaurant dataset. The work involved exploring datasets and performing exploratory data analysis (EDA) to uncover patterns and insights about the restaurant industry. I wrote SQL queries to answer key business questions, including overall market statistics, city-wise performance, pricing segments, service offerings, and cuisine popularity. The analysis also compares budget vs premium restaurants, online vs dine-in services, and provides global insights such as dining costs across countries and service feature adoption.

## Objectives

1. Analyze the Indian restaurant market and uncover overall industry statistics.
2. Identify top-performing cities based on restaurant count, customer ratings, and dining costs.
3. Explore the relationship between pricing segments, service offerings (online vs dine-in), and customer satisfaction.
4. Determine the most popular cuisines and compare performance across budget vs premium restaurant segments.

### 3. Data Analysis & Findings

The following SQL queries were developed to answer specific business questions:

1. **Get overall statastics for the Indian restaurant market.**:
```sql
select COUNT(*) as 'Total_Restaurants', 
       COUNT(distinct City) as 'Total_cities',
       avg(rating) as 'avg_rating',
       AVG(Average_Cost_for_two) as 'avg_cost'
from Restaurant_Data r
inner join Country_Data c
on r.CountryCode=c.Country_Code
where Country = 'India'

```

2. **Top 10 Indian cities with number of restaurant, average rating, average cost**:
```sql
select top 10 City, 
              COUNT(distinct RestaurantName) as 'Total_restaurants', 
              AVG(rating) as 'Avg_rating',
              AVG(average_cost_for_two) as 'avg_cost',
              country
from Restaurant_Data r
join Country_Data c
on r.CountryCode=c.Country_Code
where Country = 'India'
group by City, Country
order by Total_restaurants desc
```

3. **Understand pricing segments and their relationship with ratings in India.**:
```sql
select Price_range,
              count(*) as 'total_restaurants',
              AVG(rating) as 'avg_rating',
	   MIN(average_cost_for_two) as 'min_cost',
	   MAX(average_cost_for_two) as 'max_cost',
	   COUNT(*) * 100 / (select COUNT(*) from Restaurant_Data where CountryCode = 1) as 'percentage'
from Restaurant_Data r
join Country_Data c
on r.CountryCode=c.Country_Code
where Country = 'India'
group by Price_range
order by total_restaurants  desc
```

4. **Compare restaurants types based on service offerings in India. Online delivery vs dine-in analysis.**:
```sql
select Has_Table_booking, Has_Online_delivery,
       count(restaurantname) as 'Total_restaurants',
       AVG(rating) as 'avg_ratng',
       AVG( Average_Cost_for_two) as 'avg_cost',
       AVG(votes) as 'avg_votes'
from Restaurant_Data r
join Country_Data c
on r.CountryCode=c.Country_Code
where Country = 'India'
group by Has_Table_booking, Has_Online_delivery
order by Total_restaurants desc
```

5. **Identify top 15 most popular cuisine types and their performance metrics in India.**:
```sql
select top 15 Cuisines, COUNT(Cuisines) as total,
                        avg(Rating) avg_rating,
                        avg(Average_Cost_for_two) avg_cost,
                        avg(Votes) avg_votes

from Restaurant_Data r
join Country_Data c
on r.CountryCode=c.Country_Code
where Country='India'
group by Cuisines
order by total desc, avg_rating desc
```

6. **Compare budget vs premium restaurant segment in India.**:
```sql
select case 
           when Price_range IN (1,2) then 'Budget'
		   when Price_range in (3,4) then 'Premium'
		   end as 'Segment',
		   COUNT(*) as 'total_restaurants',
		   avg(Rating) as 'avg_rating',
		   avg(Average_Cost_for_two) as 'avg_cost',
		   avg(Votes) as 'avg_votes',
		   SUM(case when Has_online_delivery ='Yes' then 1 else 0 end) as 'no of Has_online_delivery',
		   SUM(case when Has_table_booking ='Yes' then 1 else 0 end) as 'no of Has_table_booking'
from Restaurant_Data r
join Country_Data c
on r.CountryCode=c.Country_Code
where Country='India'
group by  case 
           when Price_range IN (1,2) then 'Budget'
		   when Price_range in (3,4) then 'Premium'
		   end 
```

7. **City-wise rating distribution in top 5 Indian cities**:
```sql
with Top_cites as
(select top 5 City
from Restaurant_Data r
join Country_Data c
on r.CountryCode=c.Country_Code
where Country='India'
group by City)

select t.city, COUNT(*) as total_cities,
              SUM(case when rating >= 4 then 1 else 0 end) as 'Excellent_rating',
	   sum(case when rating >= 3 then 1 else 0 end) as 'Good_rating',
	   sum(case when rating > 0 then 1 else 0 end ) as 'Poor_rating',
	   sum(case when rating < 0 then 1 else 0 end ) as 'No_rating'

from top_cites t
join Restaurant_Data r
on t.city=r.City
group by t.city
order by total_cities desc
```

8. **Compare average dining costs across different countries.**:
```sql
select Country, 
       avg(Average_Cost_for_two) as 'Avg_cost',
       Currency
from Restaurant_Data r
join Country_Data c
on r.CountryCode=c.Country_Code
group by Country, Currency
order by Avg_cost desc 
```

9. **Analyz the digital service adoption across India cities.**:
```sql
select City, count(*) restaurant_count,
    sum(case 
            when Has_Online_delivery = 'Yes' or Has_Table_booking = 'Yes' then 1 
            else 0 
        end) digital_adoption_count,
    sum(case 
            when Has_Online_delivery = 'Yes' or Has_Table_booking = 'Yes' then 1 
            else 0 
        end) * 100 / count(*) as digital_adoption_percentage
from country_data cd 
join restaurant_data rd 
on cd.Country_Code = rd.CountryCode 
where Country = 'India' 
group by city 
order by 2 desc;
```


10. **Top rated restaurants in India with high vote count.**:
```sql
select top 20 RestaurantID,
             RestaurantName,
             City,
             Cuisines, 
             Rating,
             Votes,
             Average_Cost_for_two,
             Has_Online_delivery,
             Has_Table_booking
from country_data cd 
join restaurant_data rd 
on cd.Country_Code = rd.CountryCode 
where Country = 'India' 
            and Rating >= 4
            and Votes >= 100 
order by 5 desc, 6 desc;
```

11. **India vs other countries - service features comparison.**:
```sql
select case 
        when Country = 'India' then 'India'
        else 'Other Countries'
        end as region,
        count(*) restaurant_count,
        avg(Rating) avg_rating,
        avg(Votes) avg_votes,
        sum(case 
            when Has_Online_delivery = 'Yes' or Has_Table_booking = 'Yes' then 1 
            else 0 
        end) * 100 / count(*) as digital_adoption_percentage
from restaurant_data rd 
join country_data cd 
on cd.Country_Code = rd.CountryCode 
group by 
    case 
        when Country = 'India' then 'India'
        else 'Other Countries'
    end ;
```
   
## Findings

- **Major restaurant activity is concentrated in metro cities like Delhi, Bangalore, and Mumbai.
- **Top cities show variations in average ratings and dining costs.
- **Online delivery is more common in urban areas, while dine-in dominates premium restaurants.
- **North Indian, Chinese, and Fast Food are the most popular cuisines, with premium restaurants earning higher ratings.

## Reports

- **Market Summary**: Overview of the Indian restaurant market with city-wise distribution, average ratings, and cost analysis.
- **Service Analysis**: Comparison of online delivery and dine-in services, highlighting adoption trends across cities.
- **Cuisine Insights**: Performance of the top cuisines in India with a focus on budget vs premium restaurant segments.
- **Global Comparison**: Insights into dining costs across countries and service feature differences between India and international markets.

## Conclusion

This project gave me hands-on experience in applying SQL to analyze real-world business problems in the restaurant industry. Through exploratory analysis and business-driven queries, I uncovered insights on market trends, pricing, service models, and cuisines. The findings highlight patterns useful for strategic decision-making in the food sector. This project not only strengthened my SQL skills but also showcased my ability to think analytically and deliver insights valuable for a Business Analyst or Data Analyst role.
