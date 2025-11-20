CREATE VIEW gold.customer_report AS
WITH base AS(
--Base Query retrives needed columns
SELECT
S.order_number,
S.product_key,
S.order_date,
S.sales_amount,
S.quantity,
C.customer_key,
C.customer_number,
concat(C.first_name,' ',C.last_name) name,
datediff(year,C.birthdate,GETDATE()) AS age
FROM
gold.fact_sales S
LEFT JOIN
gold.dim_customer C
ON 
C.customer_key=S.customer_key

WHERE order_date IS NOT NULL)

,aggregated_res AS(
--AGGREGATED RESULTS
SELECT
customer_key,
customer_number,
name,
age,
COUNT(DISTINCT order_number) no_ord,
SUM(sales_amount) total_sales,
SUM(quantity) total_qty,
COUNT(product_key) ttl_prod,
MAX(order_date) AS last_ord,
DATEDIFF(MONTH,MIN(order_date),MAX(order_date)) AS lifespan
FROM
base
GROUP BY customer_key,
customer_number,
name,
age)

SELECT
customer_key,
customer_number,
name,
age,
CASE
	WHEN age<20 THEN 'UNDER 20'
	WHEN age BETWEEN 20 AND 29 THEN '20-29'
	WHEN age BETWEEN 30 AND 39 THEN '30-39'
	WHEN age BETWEEN 40 AND 49 THEN '40-49'
	ELSE '50 and above'
END AS age_cat
,
CASE
	WHEN lifespan>=12 and total_sales>5000 THEN 'VIP'
	WHEN lifespan>=12 and total_sales<=5000 THEN 'REGULAR'
	ELSE 'NEW'
END AS customer_cat,
DATEDIFF(MONTH,last_ord,GETDATE()) AS since_last_ord,
no_ord,
total_sales,
total_qty,
ttl_prod,
lifespan,
--compute average value(cvo)
total_sales/NULLIF(total_qty,0) as cvo,
--Average monthly spend
CASE 
	WHEN lifespan=0 THEN total_sales
	ELSE total_sales/lifespan
END avg_monthly_sales
FROM
aggregated_res;
