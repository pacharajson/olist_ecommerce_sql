# **Olist E-Commerce**
## **Project Overview**
This project aims to develop a scalable and user-friendly e-commerce that includes robust tools for sellers to manage sales, inventory, and customer interactions.

## **Problem Statement:**
- Which city has the highest number of orders?
- Which seller city earn the highest revenue, and how many sellers have sold on this e-commerce platform? (Segment sellers by tier)
- Which product category has the highest and lowest sales on credit card payments?
- How many products which have high average freight value?
- Which product received the most 1 to 3-star reviews?

## Database
![olist_ecom_dbgram.png](..\olist_ecommerce_sql\image\olist_ecom_dbgram.png)

## Data Dictionary
### customers

| Column Name                | Data Type    | Description                                      |
|----------------------------|--------------|--------------------------------------------------|
| `customer_id`              | VARCHAR(50)  | Unique identifier for each customer.             |
| `customer_id_unique`       | VARCHAR(50)  | Ensures unique identification for each customer. |
| `customer_zip_code_prefix` | VARCHAR(50)  | Zip code prefix of the customer.                 |
| `customer_city`            | VARCHAR(50)  | City where the customer resides.                 |
| `customer_state`           | VARCHAR(50)  | State where the customer resides.                |

### geolocation

| Column Name                | Data Type    | Description                                          |
|----------------------------|--------------|------------------------------------------------------|
| `geolocation_zip_code_prefix` | VARCHAR(50)  | Zip code prefix for the geographical location.        |
| `geolocation_lat`          | REAL         | Latitude of the geographical location.              |
| `geolocation_lng`          | REAL         | Longitude of the geographical location.             |
| `geolocation_city`         | VARCHAR(50)  | City of the geographical location.                  |
| `geolocation_state`        | VARCHAR(50)  | State of the geographical location.                 |

### product_category_name_translation

| Column Name                | Data Type    | Description                                         |
|----------------------------|--------------|-----------------------------------------------------|
| `product_category_name`    | VARCHAR(50)  | Original name of the product category.              |
| `product_category_name_english` | VARCHAR(50)  | English translation of the product category name.   |

### products

| Column Name                | Data Type    | Description                                         |
|----------------------------|--------------|-----------------------------------------------------|
| `product_id`               | VARCHAR(50)  | Unique identifier for each product.                 |
| `product_category_name`    | VARCHAR(50)  | Name of the product category.                       |
| `product_name_length`      | NUMERIC      | Length of the product name.                         |
| `product_description_length` | NUMERIC    | Length of the product description.                  |
| `product_photo_qty`        | NUMERIC      | Number of photos associated with the product.       |
| `product_weight_g`         | NUMERIC      | Weight of the product in grams.                     |
| `product_length_cm`        | NUMERIC      | Length of the product in centimeters.               |
| `product_height_cm`        | NUMERIC      | Height of the product in centimeters.               |
| `product_width_cm`         | NUMERIC      | Width of the product in centimeters.                |

### order_reviews

| Column Name                | Data Type    | Description                                         |
|----------------------------|--------------|-----------------------------------------------------|
| `review_id`                | VARCHAR(50)  | Unique identifier for each review.                  |
| `order_id`                 | VARCHAR(50)  | Identifier for the associated order.                |
| `review_score`             | NUMERIC      | Score of the review.                                |
| `review_comment_title`     | VARCHAR(50)  | Title of the review comment.                        |
| `review_comment_message`   | VARCHAR(50)  | Message content of the review.                      |
| `review_date`              | TIMESTAMP    | Date when the review was created.                   |
| `review_answer_date`       | TIMESTAMP    | Date when the review was answered.                  |

### order_list

| Column Name                | Data Type    | Description                                         |
|----------------------------|--------------|-----------------------------------------------------|
| `order_id`                 | VARCHAR(50)  | Unique identifier for each order.                   |
| `customer_id`              | VARCHAR(50)  | Identifier for the customer who placed the order.   |
| `order_status`             | VARCHAR(50)  | Status of the order (e.g., delivered, canceled).    |
| `purchase_date`            | TIMESTAMP    | Date when the order was placed.                     |
| `approved_date`            | TIMESTAMP    | Date when the order was approved.                   |
| `delivered_carrier_date`   | TIMESTAMP    | Date when the order was delivered to the carrier.   |
| `delivered_customer_date`  | TIMESTAMP    | Date when the order was delivered to the customer.  |
| `order_estimated_delivery_date` | TIMESTAMP | Estimated delivery date for the order.            |

### order_items

| Column Name                | Data Type    | Description                                         |
|----------------------------|--------------|-----------------------------------------------------|
| `order_id`                 | VARCHAR(50)  | Identifier for the associated order.                |
| `order_item_id`            | VARCHAR(50)  | Unique identifier for each item in an order.        |
| `product_id`               | VARCHAR(50)  | Identifier for the associated product.              |
| `seller_id`                | VARCHAR(50)  | Identifier for the seller of the product.           |
| `shipping_date`            | TIMESTAMP    | Date when the item was shipped.                     |
| `price`                    | REAL         | Price of the item.                                  |
| `freight_value`            | REAL         | Shipping cost for the item.                         |

### order_payments

| Column Name                | Data Type    | Description                                         |
|----------------------------|--------------|-----------------------------------------------------|
| `order_id`                 | VARCHAR(50)  | Identifier for the associated order.                |
| `payment_sequential`       | NUMERIC      | Sequential number of the payment (e.g., first payment, second payment). |
| `payment_type`             | VARCHAR(50)  | Type of payment (e.g., credit card, debit card).    |
| `payment_installs`         | NUMERIC      | Number of installments for the payment.             |
| `payment_value`            | REAL         | Total value of the payment.                         |

### sellers

| Column Name                | Data Type    | Description                                         |
|----------------------------|--------------|-----------------------------------------------------|
| `seller_id`                | VARCHAR(50)  | Unique identifier for each seller.                  |
| `seller_zip_code`          | VARCHAR(50)  | Zip code of the seller.                             |
| `seller_city`              | VARCHAR(10)  | City where the seller is located.                   |
| `seller_state`             | VARCHAR(10)  | State where the seller is located.                  |

## Data Preprocessing
### Import Data
``` sql
CREATE TABLE public.customers
(
    customer_id TEXT PRIMARY KEY,
    customer_id_unique TEXT,
    customer_zip_code_prefix NUMERIC,
    customer_city TEXT,
    customer_state TEXT
);

CREATE TABLE public.geolocation_staging
(
    geolocation_zip_code_prefix TEXT,
    geolocation_lat NUMERIC,
    geolocation_lng NUMERIC,
    geolocation_city TEXT,
    geolocation_state TEXT
);

CREATE TABLE public.order_items
(
    order_id TEXT PRIMARY KEY,
    order_item_id TEXT,
    product_id TEXT,
    seller_id TEXT,
    shipping_date TIMESTAMP,
    price REAL,
    freight_value REAL,
    FOREIGN KEY (product_id) REFERENCES products(product_id),
    FOREIGN KEY (seller_id) REFERENCES sellers(seller_id)
);

CREATE TABLE public.order_payments
(
    order_id TEXT PRIMARY KEY,
    payment_sequential NUMERIC,
    payment_type TEXT,
    payment_installs NUMERIC,
    payment_value REAL
);

CREATE TABLE public.order_reviews
(
    review_id TEXT,
    order_id TEXT PRIMARY KEY,
    review_score NUMERIC,
    review_comment_title TEXT,
    review_comment_message TEXT,
    review_date TIMESTAMP,
    review_answer_date TIMESTAMP
);

CREATE TABLE public.order_list
(
    order_id TEXT PRIMARY KEY,
    customer_id TEXT,
    order_status TEXT,
    purchase_date TIMESTAMP,
    approved_date TIMESTAMP,
    delivered_carrier_date TIMESTAMP,
    delivered_customer_date TIMESTAMP,
    order_estimated_delivery_date TIMESTAMP,
    FOREIGN KEY (customer_id) REFERENCES customers(customer_id)
);

CREATE TABLE public.products
(
    product_id TEXT PRIMARY KEY,
    product_category_name TEXT,
    product_name_length NUMERIC,
    product_description_length NUMERIC,
    product_photo_qty NUMERIC,
    product_weight_g NUMERIC,
    product_length_cm NUMERIC,
    product_height_cm NUMERIC,
    product_width_cm NUMERIC,
    FOREIGN KEY (product_category_name) REFERENCES product_category_name_translation(product_category_name)
);

CREATE TABLE public.sellers
(
    seller_id TEXT PRIMARY KEY,
    seller_zip_code NUMERIC,
    seller_city TEXT,
    seller_state TEXT
);

CREATE TABLE public.product_category_name_translation
(
    product_category_name TEXT PRIMARY KEY,
    product_category_name_english TEXT
);

CREATE TABLE public.order_items_new
(
    order_id TEXT,
    order_item_id TEXT,
    product_id TEXT,
    seller_id TEXT,
    shipping_date TIMESTAMP,
    price REAL,
    freight_value REAL
);
```
These syntax above is to create table before import data from CSV files. After that, there are two ways to import data via PGAdmin4:
1. Import data by right clicking on tables that were created
2. Import data by using PSQL Tool in this following:
```
\copy customers FROM '..\olist_ecommerce\olist_customers_dataset.csv' WITH (FORMAT csv, HEADER true, DELIMITER ',', ENCODING 'UTF8');
\copy geolocation FROM '..\olist_ecommerce\clean_data\geolocation_cleaned.csv' WITH (FORMAT csv, HEADER true, DELIMITER ',', ENCODING 'UTF8');
\copy order_items FROM '..\olist_ecommerce\clean_data\order_items_cleaned.csv' WITH (FORMAT csv, HEADER true, DELIMITER ',', ENCODING 'UTF8');
\copy order_payments FROM '..\olist_ecommerce\clean_data\order_payments_cleaned.csv' WITH (FORMAT csv, HEADER true, DELIMITER ',', ENCODING 'UTF8');
\copy order_reviews FROM '..\olist_ecommerce\clean_data\order_reviews_cleaned.csv' WITH (FORMAT csv, HEADER true, DELIMITER ',', ENCODING 'UTF8');
\copy orders_list FROM '..\olist_ecommerce\clean_data\orders_cleaned.csv' WITH (FORMAT csv, HEADER true, DELIMITER ',', ENCODING 'UTF8');
\copy products FROM '..\olist_ecommerce\clean_data\products_cleaned.csv' WITH (FORMAT csv, HEADER true, DELIMITER ',', ENCODING 'UTF8');
\copy sellers FROM '..\olist_ecommerce\clean_data\sellers_cleaned.csv' WITH (FORMAT csv, HEADER true, DELIMITER ',', ENCODING 'UTF8');
\copy product_category_name_translation FROM '..\olist_ecommerce\cleane_data\product_category_cleaned.csv' WITH (FORMAT csv, HEADER true, DELIMITER ',', ENCODING 'UTF8');
\copy order_items_new FROM '..\olist_ecommerce\olist_order_items_dataset_dup.csv' WITH (FORMAT csv, HEADER true, DELIMITER ',', ENCODING 'UTF8');
```
### Cleaning Data
```sql
-- Check duplicate all database
-- If some databases have duplicate, cleaning data syntax will be the next line
SELECT customer_id, customer_id_unique, COUNT(*)
FROM customers
GROUP BY customer_id, customer_id_unique
HAVING COUNT(*) > 1;

SELECT order_id, order_item_id, product_id, seller_id, COUNT(*)
FROM order_items
GROUP BY order_id, order_item_id, product_id, seller_id
HAVING COUNT(*) > 1;

SELECT review_id, order_id, COUNT(*)
FROM order_reviews
GROUP BY review_id, order_id
HAVING COUNT(*) > 1;

SELECT product_id, COUNT(*)
FROM products
GROUP BY product_id
HAVING COUNT(*) > 1;

SELECT order_id, customer_id, COUNT(*)
FROM order_list
GROUP BY order_id, customer_id
HAVING COUNT(*) > 1;

SELECT order_id, COUNT(*)
FROM order_payments
GROUP BY order_id
HAVING COUNT(*) > 1;

SELECT seller_id, COUNT(*)
FROM sellers
GROUP BY seller_id
HAVING COUNT(*) > 1;

--drop duplicate on seller db
WITH sellerid_dup AS (
    SELECT seller_id,
           seller_zip_code,
           seller_city,
           seller_state,
           ROW_NUMBER() OVER (PARTITION BY seller_id, 
           seller_zip_code, seller_city, seller_state ORDER BY seller_id) AS row_num
    FROM sellers
)
DELETE FROM sellers
WHERE seller_id IN (
    SELECT seller_id
    FROM sellerid_dup
    WHERE row_num > 1
);

-- identify missing data
SELECT *
FROM customers
WHERE customer_city IS NULL OR customer_id IS NULL OR customer_id_unique IS NULL OR customer_state IS NULL OR customer_zip_code_prefix IS NULL;

SELECT *
FROM geolocation_staging
WHERE geolocation_city IS NULL OR geolocation_lat IS NULL OR geolocation_lng IS NULL OR geolocation_state IS NULL OR geolocation_zip_code_prefix IS NULL;

SELECT *
FROM product_category_name_translation
WHERE product_category_name IS NULL OR product_category_name_english IS NULL;

SELECT *
FROM order_reviews
WHERE review_id IS NULL OR order_id IS NULL OR review_score IS NULL OR review_answer_date IS NULL OR review_comment_message IS NULL OR review_comment_title IS NULL OR review_date IS NULL;
-- use coalesce to replace `no comment` to NULL
SELECT
    review_id,
    order_id,
    review_score,
    review_date,
    review_answer_date,
    COALESCE(review_comment_title, 'no comment') AS review_comment_title,
    COALESCE(review_comment_message, 'no comment') AS review_comment_message
FROM order_reviews;

UPDATE order_reviews
SET
    review_comment_title = COALESCE(review_comment_title, 'No Comment'),
    review_comment_message = COALESCE(review_comment_message, 'No Comment')
WHERE review_comment_title IS NULL OR review_comment_message IS NULL;

SELECT *
FROM order_list
WHERE order_id IS NULL OR customer_id IS NULL OR order_status IS NULL OR purchase_date IS NULL OR approved_date IS NULL OR delivered_carrier_date IS NULL OR delivered_customer_date IS NULL OR order_estimated_delivery_date IS NULL;

-- There are many NULL (2980) on delivered_customer_date, so that's mean it cannot replaced or deleted
SELECT *
FROM order_list
WHERE order_status = 'delivered' AND delivered_customer_date IS NULL;
-- It must key value DATETIME instead. Anyway, It can be problem statement of e-commerce

-- NULL (611) of total (32951)
SELECT *
FROM products
WHERE product_id IS NULL OR product_category_name IS NULL OR product_name_length IS NULL OR product_description_length IS NULL OR product_photo_qty IS NULL OR product_weight_g IS NULL OR product_length_cm IS NULL OR product_height_cm IS NULL OR product_width_cm IS NULL;

-- This NULL can be dropped
DELETE FROM products
WHERE product_category_name IS NULL OR product_name_length IS NULL OR product_description_length IS NULL OR product_photo_qty IS NULL;

-- There is 1 product which is not filled product size
DELETE FROM products
WHERE product_weight_g IS NULL;

SELECT *
FROM order_payments
WHERE order_id IS NULL OR payment_sequential IS NULL OR payment_type IS NULL OR payment_installs IS NULL OR payment_value IS NULL;

SELECT *
FROM order_items
WHERE order_id IS NULL OR order_item_id IS NULL OR product_id IS NULL OR seller_id IS NULL OR shipping_date IS NULL OR price IS NULL OR freight_value IS NULL;

SELECT *
FROM sellers
WHERE seller_id IS NULL OR seller_zip_code IS NULL OR seller_city IS NULL OR seller_state IS NULL;
```
## Data Exploration Analysis
### 1. Which city has the highest number of orders?
```sql
SELECT 
    customer_city, 
    COUNT(*) AS population
FROM customers AS c
JOIN geolocation_staging AS geo 
ON c.customer_zip_code_prefix = geo.geolocation_zip_code_prefix
GROUP BY customer_city
ORDER BY population DESC
LIMIT 10;
```
![table_solution_1.png](..\image\table_solution_1.png)
![viz_solution_1.png](..\image\viz_solution_1.png)

---
### 2. Which seller city earn the highest revenue, and how many sellers have sold on this e-commerce platform? (Segment sellers by tier)
```sql
SELECT
    DISTINCT sellers.seller_city,
    SUM(pay.payment_value) AS total_revenue,
    CASE
        WHEN SUM(pay.payment_value) >= 100000 THEN 'S-tier Sellers'
        WHEN SUM(pay.payment_value) >= 50000 THEN 'A-tier Sellers'
        WHEN SUM(pay.payment_value) >= 10000 THEN 'B-tier Sellers'
        ELSE 'C-tier Sellers'
    END AS seller_tier
FROM sellers
JOIN order_items_new AS oi
USING (seller_id)
JOIN order_list AS ol
USING (order_id)
JOIN order_payments AS pay
USING (order_id)
GROUP BY sellers.seller_city
ORDER BY total_revenue DESC;
```
![table_2.png](..\image\table_2.png)

```sql
SELECT
    seller_category.seller_tier,
    COUNT(*) AS total_sellers
FROM (
    SELECT
        DISTINCT sellers.seller_city,
        SUM(pay.payment_value) AS total_revenue,
        CASE
            WHEN SUM(pay.payment_value) >= 100000 THEN 'S-tier Sellers'
            WHEN SUM(pay.payment_value) >= 50000 THEN 'A-tier Sellers'
            WHEN SUM(pay.payment_value) >= 10000 THEN 'B-tier Sellers'
            ELSE 'C-tier Sellers'
        END AS seller_tier
    FROM sellers
    JOIN order_items_new AS oi
    USING (seller_id)
    JOIN order_list AS ol
    USING (order_id)
    JOIN order_payments AS pay
    USING (order_id)
    GROUP BY sellers.seller_city) AS seller_category
GROUP BY seller_category.seller_tier;
```
![table_sub_2.png](..\image\table_sub_2.png)

---
### 3. Which product category has the highest and lowest sales on credit card payments?
```sql
SELECT
    pname.product_category_name_english AS category_name,
    op.payment_type,
    MAX(op.payment_value) AS high_revenue,
    ROUND(AVG(op.payment_value)::NUMERIC, 2) AS avg_revenue,
    MIN(op.payment_value) AS low_revenue
FROM products AS p
JOIN product_category_name_translation AS pname 
ON p.product_category_name = pname.product_category_name
JOIN order_items_new AS oi 
ON p.product_id = oi.product_id
JOIN order_list AS ol 
ON oi.order_id = ol.order_id
JOIN order_payments AS op 
ON ol.order_id = op.order_id
WHERE op.payment_type = 'credit_card'
GROUP BY pname.product_category_name_english, op.payment_type
ORDER BY avg_revenue DESC
LIMIT 10;
```
![table_s_3.png](..\image\table_s_3.png)
![viz_s_3.png](..\image\viz_s_3.png)

---
### 4. Which technology products category (IOT) has the highest average freight value?
```sql
WITH order_totals AS (
    SELECT
        oi.order_id,
        CASE 
            WHEN pt.product_category_name_english IN ('home_appliances', 'home_appliances_2') 
            THEN 'home_appliances_combined'
            ELSE pt.product_category_name_english
        END AS product_category_name_english,
        COUNT(oi.order_item_id) AS total_items_per_order,
        SUM(oi.freight_value * CAST(oi.order_item_id AS INTEGER)) AS total_freight_value,
        SUM(oi.price * CAST(oi.order_item_id AS INTEGER)) AS total_product_value
    FROM order_items_new AS oi
    JOIN products AS p ON oi.product_id = p.product_id
    JOIN product_category_name_translation AS pt ON p.product_category_name = pt.product_category_name
    GROUP BY oi.order_id, product_category_name_english
)
SELECT
    product_category_name_english,   
    ROUND(CAST(AVG(total_freight_value) AS NUMERIC), 2) AS avg_freight_value_per_order,
    ROUND(CAST(AVG(total_product_value) AS NUMERIC), 2) AS avg_product_value_per_order,
    ROUND(CAST(AVG(total_freight_value + total_product_value) AS NUMERIC), 2) AS avg_total_order_value,
    SUM(total_items_per_order) AS total_order_items_per_category
FROM order_totals
GROUP BY product_category_name_english
HAVING product_category_name_english IN 
('computers', 'computers_accessories', 'signaling_and_security', 'home_appliances_combined', 'electronics')
ORDER BY avg_total_order_value DESC;
```
![table_s_4.png](..\image\table_s_4.png)
![viz_s_4.png](..\image\viz_s_4.png)

---
### 5. Which product received the most 1 to 3-star reviews?
```sql
SELECT
    pct.product_category_name_english,
    SUM(CASE WHEN ore.review_score = 1 THEN 1 ELSE 0 END) AS "1_star_reviews",
    SUM(CASE WHEN ore.review_score = 2 THEN 1 ELSE 0 END) AS "2_star_reviews",
    SUM(CASE WHEN ore.review_score = 3 THEN 1 ELSE 0 END) AS "3_star_reviews",
    COUNT(*) AS total_reviews
FROM order_reviews AS ore
JOIN order_list AS ol USING(order_id)
JOIN order_items AS oi USING(order_id)
JOIN products AS p USING(product_id)
JOIN product_category_name_translation AS pct USING (product_category_name)
WHERE ore.review_score IN (1, 2, 3)
GROUP BY pct.product_category_name_english
ORDER BY total_reviews DESC
LIMIT 10;
```
![table_s_5.png](..\image\table_s_5.png)
![viz_s_5.png](..\image\viz_s_5.png)


## **Conclusion**
This project can help sales and marketing teams to understand customers who bought products and reviewed via this platform in this following:
- Understand geographical terms that most of Brazilian online shoppers are from Rio and Sao Paolo.
- Identify seller tier that the company can help sellers to boost their sales which is C-tier seller.
- Credit-card is the best way to do collaborative marketing campaign with banks and sellers can earn large amount of revenue from IT product category (computers, home appliances, etc.)
- Signaling and Security is the highest shipping cost, followed by computers.
- Sentiment analysis would be helped on customer reviews on 1-3 stars review and decoration (bed, bath, table) is the highest number of these review score.

