---
layout: page
title: "#009 : Store Business Dashboard"
cover-img: /assets/img/project-009/metabase-store-dashboard-illustration.png
---

> #### Key tools/skills:
> -  [Dataset Lomba 04 Sept 2020 - DQLab](https://storage.googleapis.com/dqlab-dataset/dataset%20lomba%204%20sept%202020.zip) as a data source
> - PostgreSQL as a data manipulation tool
> - Metabase as a data visualization tool
> - Data Analysis
> - Data Visualization

> Project : [_https://github.com/nairkivm/store-sales-data-analysis_](https://github.com/nairkivm/store-sales-data-analysis)

<br>

## Introduction

Previously, I created an [ETL data pipeline for an e-commerce store](https://nairkivm.github.io/projects/project-008-e-commerce-lambda-etl). Recently, I discovered another intriguing e-commerce dataset that I wanted to analyze. This personal project aims to enhance my data analytics and visualization skills while gaining more insights into e-commerce data. In this blog, I’ll walk you through how I familiarized myself with the data by tackling SQL exercises, preparing the data, building an analysis report, and presenting the findings via a dashboard (with some explanations).

## Getting exercise with some SQL problems

This dataset was originally published by the learning platform [DQLab](https://academy.dqlab.id/) as part of a data challenge in 2020.  It is also featured in their SQL learning course, which involves solving real-world case examples. The course is divided into three chapters based on difficulty and complexity. Some of the problems include:

- **Understanding the Tables**: Analyzing row numbers, column lengths, null values, distinct values, etc.
- **Top Buyers**: Identifying the top buyers of all time.
- **String Manipulation**: Extracting user emails.
- **Greatest Mean Value Buyers**: Finding buyers with the highest average purchase value.
- **Big Orders**: Identifying large orders in a specific month.
- **Dropshipper Identification**: Detecting dropshippers.
- **Average Payment Duration**: Calculating the average payment duration.

You can explore these questions by enrolling in their course (no paid endorsement, just appreciation). I have also modified some of the questions and solved them [here](https://github.com/nairkivm/store-sales-data-analysis).

## Data Preparation

I loaded the [data]((https://storage.googleapis.com/dqlab-dataset/dataset%20lomba%204%20sept%202020.zip)) into my local PostgreSQL database and made some modifications. I added `rating` column to `orders` table.


```sql
-- Add 'rating' column
alter table orders
add column rating int;

-- Fill `rating` column with random ratings value in range 0-5
-- and add some right-skewness (with factor of 0.4) to the data distribution
update orders
set rating = floor((random() ^ 0.4) * 6)::int;

-- Move some '3-rate' values to the `0-rate` to make the data more 'realistic'
update orders 
set rating = 0
where rating = 3 and random() < 0.5;
```

I also added `latitude`, `longitude`, and `age` columns to `users` table.

```sql
-- Add 'latitude' & 'longitude' columns
alter table users 
add column latitude float, 
add column longitude float;

-- Because 'latitude' & 'longitude' are coupled to represent a single meaningful location,
-- we have to add them simultaneously (in this case, using json data type)
with random_locations as (
	select 
		user_id,
		case
			-- Jakarta
			when random() < 0.1 then format('{"latitude": "%s", "longitude": "%s"}', 
				-6.2 + (random() * ( -6.1 - (-6.2) )), 106.7 + (random() * ( 106.9 - 106.7 )) )::json
			-- Bogor
			when random() < 0.2 then format('{"latitude": "%s", "longitude": "%s"}', 
				-6.6 + (random() * ( -6.5 - (-6.6) )), 106.8 + (random() * ( 106.9 - 106.8 )) )::json
			-- Depok
			when random() < 0.3 then format('{"latitude": "%s", "longitude": "%s"}', 
				-6.4 + (random() * ( -6.3 - (-6.4) )), 106.7 + (random() * ( 106.8 - 106.7 )) )::json
			-- Tangerang
			when random() < 0.4 then format('{"latitude": "%s", "longitude": "%s"}', 
				-6.2 + (random() * ( -6.1 - (-6.2) )), 106.6 + (random() * ( 106.7 - 106.6 )) )::json
			-- Bekasi
			when random() < 0.5 then format('{"latitude": "%s", "longitude": "%s"}', 
				-6.3 + (random() * ( -6.2 - (-6.3) )), 106.9 + (random() * ( 107.0 - 106.9 )) )::json
			-- South Tangerang (Tangerang Selatan)
			when random() < 0.6 then format('{"latitude": "%s", "longitude": "%s"}', 
				-6.3 + (random() * ( -6.2 - (-6.3) )), 106.7 + (random() * ( 106.8 - 106.7 )) )::json
			-- Karawang
			when random() < 0.7 then format('{"latitude": "%s", "longitude": "%s"}', 
				-6.4 + (random() * ( -6.3 - (-6.4) )), 107.2 + (random() * ( 107.3 - 107.2 )) )::json
			-- Cikarang
			when random() < 0.8 then format('{"latitude": "%s", "longitude": "%s"}', 
				-6.3 + (random() * ( -6.2 - (-6.3) )), 107.1 + (random() * ( 107.2 - 107.1 )) )::json
			-- Serang
			when random() < 0.9 then format('{"latitude": "%s", "longitude": "%s"}', 
				-6.2 + (random() * ( -6.1 - (-6.2) )), 106.1 + (random() * ( 106.2 - 106.1 )) )::json
			-- Cilegon
			else format('{"latitude": "%s", "longitude": "%s"}', 
				-6.0 + (random() * ( -5.9 - (-6.0) )), 106.0 + (random() * ( 106.1 - 106.0 )) )::json
		end::json as location
	from users
)

-- Fill the columns
update users u
set latitude = cast(r.location ->> 'latitude' as float),
longitude = cast(r.location ->> 'longitude' as float)
from random_locations r
where u.user_id = r.user_id
;

-- Add 'age' column
alter table users 
add column age int;

-- Fill the column with random age between 18 and 63
update users 
set age =  floor(18 + (random() * ( 63 - 18 )));

-- Add 'skewness' to the data (right-skew for age <= 32)
update users 
set age =  floor(18 + (random()^0.6 * ( 63 - 18 ))) 
where age <= 32;

-- Add 'skewness' to the data (left-skew for age >= 49)
update users 
set age =  floor(18 + (random()^1.2 * ( 63 - 18 ))) 
where age >= 49;
```

## Building Analysis Report

I created the analysis report based on the [Metabase](https://www.metabase.com/) example dashboard as a reference. The dashboard is divided into four pages: 

1. **Overall Business Metrics**: This page displays general metrics such as total revenues, active users, and order counts.
2. **Portfolio Performance**: Here, we delve into factors affecting revenue, including top-selling products or categories and customer satisfaction.
3. **Demographics**: This section provides insights into our customers, including who they are and where they are located.
4. **Retention Rate**: The final page concludes with retention rate metrics, highlighting customer loyalty and its impact on revenue stability.

In Metabase, a dashboard consists of several “questions,” which are essentially SQL queries designed to “answer the problem.” These questions can be visualized using various options such as metrics, trends, tables, pivot tables, maps, and graphs. Each metric mentioned above requires a corresponding query. Fortunately, Metabase’s GUI simplifies this task. You can find the detailed question scripts in this [repository](https://github.com/nairkivm/store-sales-data-analysis).

## Analysis Report

<embed src="https://nairkivm.github.io/assets/doc/metabase_dashboard_241005.pdf" type="application/pdf" style="width: 100%; height: 100%"/>

> What’s the situation with this store’s business?

Overall, the store’s performance is very promising. In May 2020, the store generated a total revenue of 29 billion rupiah, marking a 46% increase from April 2020. To achieve this, they allocated 234 million rupiah for discounts, which is about 8% of the generated revenue. The number of orders and active customers also increased by 25% (9,327 orders) and 28% (8,972 users) from the previous month, respectively. The trends in orders and revenue, both overall and by each channel, have been increasing over time, except for a dip in January 2020, likely due to the end-of-year holidays. By the second month of the second quarter, they had achieved 49% of their quarterly revenue target.

> What factors contribute to this store’s revenue?

The top three best-selling products of all time are women’s accessories, men’s wear, and women’s wear. However, the top three best-selling product categories are personal care, fresh food, and men’s wear. The trends indicate that personal care sales significantly outperform other categories. From the customer perspective, the store has received a good rating, with a score of 3.47 out of 5.

> Who and where are the customers?

The customers are primarily located in the Jabodetabek area and its neighboring cities, with the majority coming from Tangerang and Tangerang Selatan. The average customer is aged between 40-45 years. There is no significant difference in the preferred categories and channels among different age groups.

> How loyal are the customers?

As of May 2020, 25% of the store’s monthly active users are repeat customers. The rate of new users is decreasing over time, meaning most active users are returning customers. From the cohort analysis and further examination,

| Month    | New User | Contribution to May 2020 | Users Returned at May 2020 |
|----------|----------|--------------------------|----------------------------|
| Jan 2019 | 117      | 49.57%                   | 58                         |
| Feb 2019 | 347      | 52.16%                   | 181                        |
| Mar 2019 | 633      | 54.50%                   | 345                        |
| Apr 2019 | 898      | 52.56%                   | 472                        |
| May 2019 | 1250     | 52.40%                   | 655                        |
| Jun 2019 | 1489     | 53.93%                   | 803                        |
| Jul 2019 | 1832     | 53.60%                   | 982                        |
| Aug 2019 | 1952     | 53.69%                   | 1048                       |
| Sep 2019 | 2018     | 54.36%                   | 1097                       |
| Oct 2019 | 1967     | 54.65%                   | 1075                       |
| Nov 2019 | 1850     | 52.27%                   | 967                        |
| Dec 2019 | 1567     | 53.86%                   | 844                        |
| Jan 2020 | 542      | 52.21%                   | 283                        |
| Feb 2020 | 480      | 49.58%                   | 238                        |
| Mar 2020 | 385      | 47.53%                   | 183                        |
| Apr 2020 | 268      | 36.18%                   | 97                         |

we can see that users in May 2020 are predominantly new users from September, October, and August 2019. It appears that customers tend to return and make purchases again after more than one month.

> Posted at 2024-10-05