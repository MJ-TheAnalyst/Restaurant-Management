# Restaurant Management
## Exploratory Data Analysis on Restaurant Order Patterns

## Table of Contents

- [Project Overveiw](#project-overview)
- [Data Sources](#data-sources)
- [Tools](#tools)
- [Data Initialization](#data-initialization)
- [Exploratory Data Analysis](#exploratory-data-analysis) 
- [Data Analysis](#data-analysis) 
- [Results and Findings](#results-and-findings)
- [Recommendations](#recommendations)
- [Limitations](#limitations)
- [References](#references)

  
### Project Overview

This project leverages SQL-based Exploratory Data Analysis (EDA) and Excel Visualizations to extract insights from a restaurant’s database. The analysis covers both menu design and customer ordering behavior. By combining and analyzing the menu_items and order_details tables, the project uncovers trends in pricing, order volume, popular items, and high-spending customers.

### Data Sources

The data set used in this analysis is from a structured SQL script file "Create_Restaurant_db.sql" file. This file contains the schema and sample data for a simulated restaurant database. The dataset is designed to emulate a real-world restaurant environment and supports exploration of both menu structure and customer purchasing behavior.

### Tools

- MySQL (Data Analysis)
- Excel (Creating Reports)

### Data Initialization

During the Data initalization phase we performed the following:
- We created a new database to stimulate the restuarant environment.
- Created menu_items and order_details tables in the database.
- Inserted sample data into both tables to simulate a restaurant environment.

### Exploratory Data Analysis

We explored the restaurant's data to answer key questions like:

- What items are on the menu, and how are they priced across different categories?
- Which menu items are the most and least expensive overall and within specific cuisines like Italian?
- How many orders were placed, and what was the volume of items ordered over time?
- Which items are most and least frequently ordered, and what categories do they belong to?
- Which orders generated the highest revenue, and what types of items did they include?



### Data Analysis
#### For menu Items

``` SQL
 Use Restaurant_db;

-- 1. View the menu_item table.
Select * From menu_items;

-- 2. Find the number of items on the menu.
Select count(menu_item_id) from menu_items;

-- 3. What are the least and most expensive items on the menu?
--  Least expensive
Select * from menu_items 
order by price;

--  most expensive
Select * from menu_items
order by price desc;

-- 4. How Many italian dishes are in the menu?
Select Count(*)
from menu_items
where category = 'Italian';

-- 5. What are the least and most expensive Italian dishes on the menu?
Select *
from menu_items
where category = 'Italian'
Order by price;

Select *
from menu_items
where category = 'Italian'
Order by price desc;

-- 6. How many dishes are in each category?
Select category, Count(menu_item_id) as  Number_of_Dishes
From menu_items
Group by Category;


-- 7. what is the average dish price within each category?
Select category, avg(Price) as  Average_Price
From menu_items
Group by Category;
```

#### For Order Details

```SQL
-- 1. View the order_details table
Select * from order_details;
-- 2. What is the data range of the table?
Select * from order_details
order by order_date;

-- OR 
Select min(order_date), max(Order_date) from order_details;

-- 3. How many orders were made within this data range?
Select count(distinct order_id) from order_details;

-- 4. How many items were ordered within this data range?
select count(order_details_id) from order_details;

-- 5. Which Orders had the most number of items?
select Order_id, count(item_id) as num_items
from order_details
Group by Order_id
order by num_items desc;

-- how many orders had more than 12 items?
Select count(*) from 
(select Order_id, count(item_id) as num_items
from order_details
Group by Order_id
Having num_items > 12) as  num_orders;
```

#### For Combined Analysis
``` SQL
-- 1. Combine the menu_items and order_Details tables into a single table
Select *
from menu_items;

select * 
From Order_Details;

Select *
from Order_details od
left join menu_items mi
on od.item_id = mi.menu_item_id;

-- 2. What were the least and most ordered items? what categories were they in?
Select item_name, count(Order_details_id) as num_purchases
from Order_details od
left join menu_items mi
on od.item_id = mi.menu_item_id
group by item_name
order by num_purchases;

-- most expensive

Select item_name, count(Order_details_id) as num_purchases
from Order_details od
left join menu_items mi
on od.item_id = mi.menu_item_id
group by item_name
order by num_purchases desc;

-- their categories
Select item_name, category, count(Order_details_id) as num_purchases
from Order_details od
left join menu_items mi
on od.item_id = mi.menu_item_id
group by item_name, category
order by num_purchases;

-- 3. what were the top 5 orders that spent the most money?
Select order_id, sum(price) as Total_Purchases
from Order_details od
left join menu_items mi
on od.item_id = mi.menu_item_id
group by order_id
Order by total_Purchases desc
Limit 5;

-- view details of highest spent order, what insights can you gather from it?
Select category, count(item_id) as num_items
from Order_details od
left join menu_items mi
on od.item_id = mi.menu_item_id
Where order_id = 440
group by category;

-- view details of top 5 highest spent order, what insights can you gather from it?
Select order_id, category, count(item_id) as num_items
from Order_details od
left join menu_items mi
on od.item_id = mi.menu_item_id
Where order_id in ( 440, 2075, 1957, 330, 2675)
group by order_id, category;
```

### Data Visualization: 

<img width="960" height="490" alt="Restaurant Management Visualization (cropped)" src="https://github.com/user-attachments/assets/24486d3f-946e-4f3a-974d-dedbc8bf605b" />

The Excel Dashboard provided a visual summary of key insights from restaurant operations data, focusing on menu pricing, order trends, and item popularity.
Key Highlights included:

Total Orders: 441 orders were recorded over the period analyzed.
1. Most Expensive Item: Shrimp Scampi at $19.95.
2. Least Expensive Item: Edamame at $5.00.
3. Most Ordered Item: Hamburger(Item ID 101).
4. Least Ordered Item: Tacos (Item ID 115).
5. Italian dishes had the highest total price range, followed by Asian and American.
6. Price range visualization shows category-wise contribution to revenue potential.
7. Most Popular Dishes include Shrimp Scampi, Spaghetti & Meatballs, Chicken Parmesan, and Pulled Pork.
8. Least Popular Dishes feature items like Edamame, Chips & Salsa, and French Fries, which may need review or promotion.
9. Daily order volume remained relatively stable, averaging around 150–160 orders/day, with slight dips and peaks visible.



### Results and Findings

Here are my findings summarized below:

1. Italian dishes had the highest total price value at $150.75, followed by Asian ($107.80) and American ($60.40) cuisines, suggesting Italian dishes are the most premium or varied.
2. The most expensive menu item was Shrimp Scampi priced at $19.95, while Spaghetti & Meatballs,Korean Beef Bowl, Pork Ramen and Meat Lasagna followed at $17.95.
3. The least expensive dishes included Edamame priced at just $5.00, followed by Mac & Cheese, French Fries and Chips & Salsa at $7.00.
4. The most liked (frequently ordered) items is the  based on item count in order logs, indicating strong customer preference.
5. The least liked (least ordered) items were Beef Wellington and Miso Soup, which received very few orders and may need to be re-evaluated or better promoted.
6. Daily order volume was consistent, ranging between 154 and 161 orders per day, highlighting steady customer traffic.

### Recommendations

1. Highlight popular items like Cheeseburgers and Chicken Tacos in marketing campaigns and combo deals to maximize revenue.
2. Consider removing or rebranding low-demand dishes like Beef Wellington and Miso Soup to reduce waste and improve menu efficiency.
3. Italian dishes show strong value and presence—develop premium Italian meal bundles or feature them in loyalty promotions.
4. Use insights from pricing ranges to balance the menu with both affordable options (e.g., French Fries) and high-margin items.
5. With steady daily order volumes, continue tracking order trends weekly/monthly to prepare for seasonal fluctuations or staff scheduling.

### Limitations

1. The dataset may not cover a long enough time period to reflect seasonal or long-term customer behavior trends.
2. Without data on customer age, location, or preferences, it's difficult to personalize menu recommendations or promotions.
3. Prices are available, but there’s no direct record of quantities sold or total revenue per item, limiting profitability analysis.
4. The analysis assumes static item prices over time, which may not account for promotions, price changes, or discounts.
5. The project lacks qualitative feedback like reviews or ratings, making it hard to judge why some items are underperforming.

### References

[Guided Tutorial](https://youtu.be/JaUKDbCXMX4?si=45f0TFBBjt7qd9PO)

[Download Dataset Here](https://mavenanalytics.io/data-playground?pageSize=10)
