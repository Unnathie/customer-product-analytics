CREATE VIEW gold.report_prod AS
WITH base_q AS (
    SELECT
        -- base query
        p.product_name,
        p.product_key,
        p.category,
        p.subcategory,
        p.product_cost,
        f.sales_amount,
        f.order_number,
        f.order_date,
        f.quantity,
        f.customer_key
    FROM gold.fact_sales f
    LEFT JOIN gold.dim_products p
        ON f.product_key = p.product_key
    WHERE order_date IS NOT NULL
),

aggregated_res AS (
    SELECT
    --Aggregated result
        product_key,
        product_name,
        category,
        subcategory,
        product_cost,
        MAX(order_date) AS last_date,
        SUM(sales_amount) AS ttl_sales,
        COUNT(DISTINCT order_number) AS ttl_ords,
        SUM(quantity) AS ttl_qty,
        COUNT(DISTINCT customer_key) AS ttl_cust,
        DATEDIFF(MONTH, MIN(order_date), MAX(order_date)) AS life_span,
        ROUND(AVG(CAST(sales_amount AS FLOAT) / NULLIF(quantity,0)), 1) AS avg_selling_price
    FROM base_q
    GROUP BY
        product_key,
        product_name,
        category,
        subcategory,
        product_cost
)

SELECT
    product_key,
    product_name,
    category,
    subcategory,
    product_cost,
    avg_selling_price,
    CASE
        WHEN ttl_sales >= 100000 THEN 'HIGH-PERFORMER'
        WHEN ttl_sales BETWEEN 5000 AND 100000 THEN 'MID-PERFORMER'
        ELSE 'LOW-PERFORMER'
    END AS prd_perform,
    ttl_sales,
    ttl_ords,
    ttl_qty,
    ttl_cust,
    life_span,
    DATEDIFF(MONTH, last_date, GETDATE()) AS since_last_ord,
    ttl_sales / NULLIF(ttl_ords, 0) AS avg_order_rev,
    CASE
        WHEN life_span = 0 THEN ttl_sales
        ELSE ttl_sales / life_span
    END AS monthly_rev
FROM aggregated_res;
