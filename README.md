![banner](Assets/Banner.jpg)

# Music-Store-SQL-Project
***SQL queries and solutions for diverse data analysis questions, from senior employees to music genre popularity by country. Explore, and learn the world of data analysis and SQL.***

## Author
- [@saadharoon27](https://github.com/saadharoon27)

## Table of Contents
- [Project Overview](#project-overview)
- [About The Dataset](#about-the-dataset)
- [Queries](#queries)
- [SQL Commands](#sql-commands)

## Project Overview
This repository contains a collection of **SQL queries** and solutions to a variety of **data analysis questions**. These questions cover a wide range of topics and scenarios, from identifying **senior employees** based on job titles to finding the most popular **music genre** in different countries. The queries are designed to showcase **SQL skills** in querying and analyzing data, making them a valuable resource for anyone interested in SQL and data analysis.

## About The Dataset
**Database Schema**<br>
![Schema](MusicSchema.png)
-------
[**Music Store Dataset**](https://www.kaggle.com/datasets/syedibrahim03/music-store-dataset-sql-postgre)

## Queries
**Question Sets Included:**

- **Question Set 1:** Explore employee seniority, top countries by invoices, highest invoice values, best city for a promotional event, and the best customer.
  - 1. **Who is the senior most employee** based on job title?
  - 2. **Which countries have the most Invoices**?
  - 3. What are the **top 3 values of total invoice**?
  - 4. Which **city has the best customers**? Plans of throwing a **Music Festival** in the city, so that most money can be made. Write a query that returns one city that has the highest sum of invoice totals. Return both **the city name & sum of all invoice totals.**
  - 5. **Who is the best customer**? The customer who has spent **the most money will be declared the best customer**. Write a query that returns **the person who has spent the most money**.

- **Question Set 2:** Retrieve information about **Rock Music listeners**, top **Rock bands**, and tracks with **above-average song lengths**.
  - 1. Write a query to return the **email, first name, last name, and genre** of all Rock Music listeners. Return the list ordered alphabetically by email starting with A.
  - 2. Invite the artists who have written the most **rock music** in the dataset. Write a query that returns the **Artist name and total track count** of the ***top 10*** rock bands.
  - 3. Return all the track names that have a song length longer than the **average song length**. Return the **Name and Milliseconds** for each track. Order by the song length with the **longest songs listed first**.

- **Question Set 3:** Analyze **customer spending** on artists, determine the **most popular music genre** in each country, and identify the top **spending customers** by country.
  - 1. **Find how much amount spent by each customer on artists**? Write a query to return **customer name, artist name,** and **total spent**.
  - 2. We want to find out the **most popular music Genre for each country**. We determine the most popular genre as the genre with the **highest amount of purchases**. Write a query that returns each country along with the **top Genre**. For countries where the **maximum number of purchases is shared**, return all Genres.
  - 3. Write a query that determines the **customer that has spent the most on music for each country**. Write a query that returns the **country** along with the **top customer** and **how much they spent**. For countries where the **top amount spent is shared**, provide all customers who spent this amount.

## SQL Commands

**Question Set 1**

- **Q1: Who is the senior most employee based on job title?**
```sql
SELECT * FROM employee
ORDER BY levels DESC
LIMIT 1;
```

- **Q2: Which countries have the most Invoices?**
```sql
SELECT COUNT(*) AS c, billing_country 
FROM invoice
GROUP BY billing_country
ORDER BY c DESC;
```

- **Q3: What are the top 3 values of total invoice?**
```sql
SELECT total FROM invoice
ORDER BY total DESC
LIMIT 3;
```

- **Q4: Which city has the best customers?**
```sql
SELECT SUM(total) AS invoice_total, billing_city 
FROM invoice
GROUP BY billing_city
ORDER BY invoice_total;
```

- **Q5: Who is the best customer?**
```sql
SELECT customer.customer_id, customer.first_name, customer.last_name, SUM(invoice.total) AS total
FROM customer
JOIN invoice ON customer.customer_id = invoice.customer_id
GROUP BY customer.customer_id
ORDER BY total DESC
LIMIT 1;
```

**Question Set 2**

- **Q1: Write a query to return the email, first name, last name, & Genre of all Rock Music listeners.**
```sql
SELECT DISTINCT email, first_name, last_name
FROM customer
JOIN invoice ON customer.customer_id = invoice.customer_id
JOIN invoice_line ON invoice.invoice_id = invoice_line.invoice_id
WHERE track_id IN (
    SELECT track_id FROM track
    JOIN genre ON track.genre_id = genre.genre_id
    WHERE genre.name LIKE 'Rock'
)
ORDER BY email;
```

- **Q2: Let's invite the artists who have written the most rock music.**
```sql
SELECT artist.artist_id, artist.name, COUNT(artist.artist_id) AS number_of_songs
FROM track
JOIN album ON album.album_id = track.album_id
JOIN artist ON artist.artist_id = album.artist_id
JOIN genre ON genre.genre_id = track.genre_id
WHERE genre.name LIKE 'Rock'
GROUP BY artist.artist_id
ORDER BY number_of_songs DESC
LIMIT 10;
```

- **Q3: Return all track names with song length longer than the average.**
```sql
SELECT name, Milliseconds
FROM track
WHERE Milliseconds > (
    SELECT AVG(Milliseconds) AS avg_track_length
    FROM track
)
ORDER BY Milliseconds DESC;
```

**Question Set 3**

- **Q1: Find how much amount spent by each customer on artists.**
```sql
WITH best_selling_artist AS (
    SELECT artist.artist_id AS artist_id, artist.name AS artist_name, SUM(invoice_line.unit_price * invoice_line.quantity) AS total_sales
    FROM invoice_line
    JOIN track ON track.track_id = invoice_line.track_id
    JOIN album ON album.album_id = track.album_id
    JOIN artist ON artist.artist_id = album.artist_id
    GROUP BY 1
    ORDER BY 3 DESC
    LIMIT 1
)
SELECT c.customer_id, c.first_name, c.last_name, bsa.artist_name, SUM(il.unit_price * il.quantity) AS amount_spent
FROM invoice i
JOIN customer c ON c.customer_id = i.customer_id
JOIN invoice_line il ON il.invoice_id = i.invoice_id
JOIN track t ON t.track_id = il.track_id
JOIN album alb ON alb.album_id = t.album_id
JOIN best_selling_artist bsa ON bsa.artist_id = alb.artist_id
GROUP BY 1, 2, 3, 4
ORDER BY 5 DESC;
```

- **Q2: Determine the most popular music Genre for each country.**
```sql
WITH popular_genre AS (
    SELECT COUNT(invoice_line.quantity) AS purchases, customer.country, genre.name, genre.genre_id, 
    ROW_NUMBER() OVER(PARTITION BY customer.country ORDER BY COUNT(invoice_line.quantity) DESC) AS RowNo 
    FROM invoice_line 
    JOIN invoice ON invoice.invoice_id = invoice_line.invoice_id
    JOIN customer ON customer.customer_id = invoice.customer_id
    JOIN track ON track.track_id = invoice_line.track_id
    JOIN genre ON genre.genre_id = track.genre_id
    GROUP BY 2, 3, 4
    ORDER BY 2 ASC, 1 DESC
)
SELECT * FROM popular_genre WHERE RowNo <= 1;
```

- **Q3: Find the customer that has spent the most on music for each country.**
```sql
WITH Customer_with_country AS (
    SELECT customer.customer_id, first_name, last_name, billing_country, SUM(total) AS total_spending,
    ROW_NUMBER() OVER(PARTITION BY billing_country ORDER BY SUM(total) DESC) AS RowNo 
    FROM invoice
    JOIN customer ON customer.customer_id = invoice.customer_id
    GROUP BY 1, 2, 3, 4
    ORDER BY 4 ASC, 5 DESC
)
SELECT * FROM Customer_with_country WHERE RowNo <= 1;
```
