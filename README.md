1.1
SELECT p.Product_Id, p.Product_Name
FROM product_info p
JOIN category c ON p.Category_Id = c.Id
WHERE c.Name LIKE '%golf%'
ORDER BY p.Product_Id;

1.2
SELECT p.Product_Name, SUM(oi.Sales) AS Total_Sales
FROM ordered_items oi
JOIN product_info p ON oi.Item_Id = p.Product_Id
JOIN category c ON p.Category_Id = c.Id
WHERE c.Name LIKE '%golf%'
GROUP BY p.Product_Name
ORDER BY Total_Sales DESC
LIMIT 10;


1.3
SELECT ci.Segment AS Customer_segment, COUNT(o.Order_Id) AS Orders
FROM orders o
JOIN customer_info ci ON o.Customer_Id = ci.Id
GROUP BY ci.Segment
ORDER BY Orders DESC;



1.4
SELECT ci.Segment AS Customer_segment,
       ROUND(COUNT(o.Order_Id) * 100.0 / total_orders.total * 1.0, 1) AS Percentage_order_split
FROM orders o
JOIN customer_info ci ON o.Customer_Id = ci.Id
JOIN (SELECT COUNT(*) AS total FROM orders WHERE Real_Shipping_Days = 6) AS total_orders
ON o.Real_Shipping_Days = 6
GROUP BY ci.Segment
ORDER BY Percentage_order_split DESC;



2.1
SELECT DATE_FORMAT(o.Order_Date, '%Y-%m') AS Month,
       SUM(oi.Quantity) AS Quantities_sold,
       SUM(oi.Sales) AS Sales
FROM ordered_items oi
JOIN product_info p ON oi.Item_Id = p.Product_Id
JOIN orders o ON oi.Order_Id = o.Order_Id
WHERE p.Product_Name LIKE '%Nike%'
GROUP BY Month
ORDER BY Month;

2.2
SELECT p.Product_Id,
       p.Product_Name,
       c.Name AS Category_Name,
       d.Name AS Department_Name,
       p.Product_Price
FROM product_info p
JOIN category c ON p.Category_Id = c.Id
JOIN department d ON p.Department_Id = d.Id
ORDER BY p.Product_Price DESC
LIMIT 5;


2.3
SELECT p.Product_Name,
       SUM(oi.Sales) AS Sales,
       COUNT(DISTINCT oi.Order_Id) AS Order_Count
FROM ordered_items oi
JOIN product_info p ON oi.Item_Id = p.Product_Id
JOIN orders o ON oi.Order_Id = o.Order_Id
WHERE o.Type = 'CASH'
GROUP BY p.Product_Name
ORDER BY Order_Count DESC, Sales DESC
LIMIT 10;




2.4
SELECT o.*
FROM orders o
JOIN customer_info ci ON o.Customer_Id = ci.Id
WHERE ci.State = 'TX' 
  AND ci.Street LIKE '%Plaza%' 
  AND ci.Street NOT LIKE '%Mountain%'
ORDER BY o.Order_Id;




2.5
SELECT COUNT(DISTINCT o.Order_Id) AS Order_Count
FROM orders o
JOIN customer_info ci ON o.Customer_Id = ci.Id
JOIN ordered_items oi ON o.Order_Id = oi.Order_Id
JOIN product_info p ON oi.Item_Id = p.Product_Id
JOIN department d ON p.Department_Id = d.Id
WHERE ci.Segment = 'Home Office'
  AND (d.Name = 'Apparel' OR d.Name = 'Outdoors');




2.6
WITH OrderCounts AS (
    SELECT o.Order_State,
           ci.City AS Order_City,
           COUNT(o.Order_Id) AS Order_Count
    FROM orders o
    JOIN customer_info ci ON o.Customer_Id = ci.Id
    JOIN ordered_items oi ON o.Order_Id = oi.Order_Id
    JOIN product_info p ON oi.Item_Id = p.Product_Id
    WHERE ci.Segment = 'Home Office'
      AND (d.Name = 'Apparel' OR d.Name = 'Outdoors')
    GROUP BY o.Order_State, ci.City
),
RankedCities AS (
    SELECT Order_State,
           Order_City,
           Order_Count,
           DENSE_RANK() OVER (PARTITION BY Order_State ORDER BY Order_Count DESC, Order_City ASC) AS City_rank
    FROM OrderCounts
)
SELECT Order_State, Order_City, Order_Count, City_rank
FROM RankedCities
ORDER BY Order_State, City_rank;




2.7
WITH UnderestimatedOrders AS (
    SELECT o.Shipping_Mode,
           YEAR(o.Order_Date) AS Order_Year,
           COUNT(o.Order_Id) AS Shipping_Underestimated_Order_Count
    FROM orders o
    JOIN customer_info ci ON o.Customer_Id = ci.Id
    WHERE o.Scheduled_Shipping_Days < o.Real_Shipping_Days
      AND (o.Order_Status = 'COMPLETE' OR o.Order_Status = 'CLOSED')
      AND ci.Segment = 'Consumer'
    GROUP BY o.Shipping_Mode, Order_Year
)
SELECT Shipping_Mode,
       Shipping_Underestimated_Order_Count,
       ROW_NUMBER() OVER (PARTITION BY Order_Year ORDER BY Shipping_Underestimated_Order_Count DESC) AS Shipping_Mode_Rank
FROM UnderestimatedOrders;




