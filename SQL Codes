#For menu Items

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



#For Order Details
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

#For Combined Analysis

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
