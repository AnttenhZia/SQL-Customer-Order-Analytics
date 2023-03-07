#In this SQL, I'm querying a database with multiple tables in it to quantify statistics about customer and order data.


#1. --How many orders were placed in January? 

SELECT COUNT(orderid)
FROM BIT_DB.JanSales
WHERE length(orderid) = 6 
AND orderid <> 'Order ID';

#2.--How many of those orders were for an iPhone? 

SELECT COUNT(orderid)
FROM BIT_DB.JanSales
WHERE Product='iPhone'
AND length(orderid) = 6 
AND orderid <> 'Order ID';


#3. -- the customer account numbers for all the orders that were placed in February.

SELECT distinct acctnum
FROM BIT_DB.customers cust

INNER JOIN BIT_DB.FebSales Feb
ON cust.order_id=FEB.orderid
WHERE length(orderid) = 6 
AND orderid <> 'Order ID';

#4.--Which product was the cheapest one sold in January, and what was the price?

SELECT product, MIN(price) as lowest
FROM BIT_DB.JanSales;


#5.--What is the total revenue for each product sold in January? (Revenue can be calculated using the number of products sold and the price of the products).

SELECT sum(quantity)*price as revenue
,product
FROM BIT_DB.JanSales
GROUP BY product;


#6.--Which products were sold in February at 548 Lincoln St, Seattle, WA 98101, how many of each were sold, and what was the total revenue?

SELECT SUM(quantity)*price as revenue, product, location 
FROM BIT_DB.FebSales
WHERE location = "548 Lincoln St, Seattle, WA 98101";

#7.--How many customers ordered more than 2 products at a time in February, and what was the average amount spent for those customers?

select count(distinct cust.acctnum), 
avg(quantity*price)
FROM BIT_DB.FebSales Feb
LEFT JOIN BIT_DB.customers cust
ON FEB.orderid=cust.order_id
WHERE Feb.Quantity>2
AND length(orderid) = 6 
AND orderid <> 'Order ID';


#8.--List all the products sold in Los Angeles in February, and include how many of each were sold.

SELECT Product, SUM(quantity)
FROM BIT_DB.FebSales 
WHERE location like '%Los Angeles%'
GROUP BY Product;

#9.--Which locations in New York received at least 3 orders in January, and how many orders did they each receive? 

SELECT distinct location, count(orderID)
FROM BIT_DB.JanSales
WHERE location LIKE '%NY%'
AND length(orderid) = 6 
AND orderid <> 'Order ID'
GROUP BY location
HAVING count(orderID)>2;

#10. --How many of each type of headphone were sold in February?

SELECT SUM(quantity) as Quantity, product
FROM BIT_DB.FebSales
WHERE product like '%headphone%'
GROUP BY product;


#11.--What was the average amount spent per account in February? 

SELECT avg(quantity*price)
FROM BIT_DB.FebSales Feb
LEFT JOIN BIT_DB.customers cust
ON FEB.orderid=cust.order_id
WHERE length(orderid) = 6 
AND orderid <> 'Order ID';

#12.--What was the average quantity of products purchased per account in February?

select sum(quantity)/count(cust.acctnum)
FROM BIT_DB.FebSales Feb
LEFT JOIN BIT_DB.customers cust
ON FEB.orderid=cust.order_id
WHERE length(orderid) = 6 
AND orderid <> 'Order ID';

#13.--Which product brought in the most revenue in January and how much revenue did it bring in total?

SELECT product, 
sum(quantity*price)
FROM BIT_DB.JanSales 
GROUP BY product
ORDER BY sum(quantity*price) DESC
LIMIT 1;









