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
## [Music Store Dataset](https://www.kaggle.com/datasets/syedibrahim03/music-store-dataset-sql-postgre)

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
