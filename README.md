# SQL Case Study: Analysing Unicorn Companies

## Medium Article Published
- [Beauty of Custom-Made Filters](https://medium.com/@goylusun/ozzys-sql-intricacies-exploration-series-1-deploying-custom-made-filter-on-a-unicorn-companies-219db68809b6)

## Table of Contents

- [Exploratory Data Analysis (EDA)](#Exploratory_Data_Analysis)
- [Findings](#Findings)
- [Recommendations](#Recommendations)

### Case Study Overview
---

This case study asks data analysts to support an angel investment firm by discovering top 3 performing industries in terms of **the highest number of unicorn companies that they have had up to now**. In the world of finance, "*the term unicorn refers to a privately held startup company with a value of over $1 billion*."

<p align="center">
  <img src="https://github.com/OzzyGoylusun/SQL.-Case-Study-Analysing-Unicorn-Companies/blob/main/Visuals/Financial%20Unicorn.png"
 alt="Financial Unicorn" width="750">
</p>


As well as discovering what those top industries are, an exploration work of average valuation in billions and number of unicorns per these top 3 for the years 2019, 2020 and 2021 was also undertaken. 

These activities were in order to asist the organisation with pre-emptively identifying trends towards from which industries more and more high-growth companies could most likely breed in future.


### Data Source

The database, named after *Unicorns*, utilised for this project is hosted at DataCamp's proprietary servers, and as such, no data import work was required. 

Fundamentally, the database consists of the following four tables:

<div style="display: flex; justify-content: space-between;">
  <img width="410" alt="Dates DB" src="https://raw.githubusercontent.com/OzzyGoylusun/SQL.-Case-Study-Analysing-Unicorn-Companies/main/Visuals/Dates%20DB.png"> 
  <img width="410" alt="Funding DB" src="https://raw.githubusercontent.com/OzzyGoylusun/SQL.-Case-Study-Analysing-Unicorn-Companies/main/Visuals/Funding%20DB.png">
</div>

<div style="display: flex; justify-content: space-between;">
  <img width="410" alt="Industries DB" src="https://github.com/OzzyGoylusun/SQL.-Case-Study-Analysing-Unicorn-Companies/blob/main/Visuals/Industries%20DB.png"> 
  <img width="410" alt="Companies DB" src="https://github.com/OzzyGoylusun/SQL.-Case-Study-Analysing-Unicorn-Companies/blob/main/Visuals/Companies%20DB.png">
</div>


### Tools

- PostgreSQL Server - Data Analysis
  - [Download Here](https://www.postgresql.org/download/)
  


### Data Cleaning/Preparation

Within the scope of this case study, no data cleaning/handling tasks were needed to conduct.



### Exploratory_Data_Analysis

EDA predominantly involved exploring the Unicorns database to answer key questions, such as:

1. What are the top three mysterious industries based upon how many unicorn companies they have possessed up to date?
2. What are the average valuations in billions for these top 3 industries, separately calculated for the years 2019, 2020 and 2021?
3. How many unicorn companies do the top 3 industries possess, solely for the years 2019, 2020 and 2021?


### Data Analysis

In my opinion, the most challenging aspect of this EDA work was to recognise that without obtaining those top 3 industries' names first,
it would make it almost an impossible mission to achieve the requested outcome.

Accordingly, I decided to create a table-shaped filter below only accountable for obtaining those names:

```sql
WITH FILTER_FOR_TOP3_PERFORMING_INDUSTRIES AS(

SELECT INDUSTRY, COUNT(COMPANY_ID)
FROM QUALIFIED_UNICORN_COMPANY_INFO --That's where I compiled all the required information
GROUP BY 1 
ORDER BY 2 DESC
LIMIT 3 --We're only interested in Top 3 industries' names per # of Unicorns that they possess to date

)
```

Without this filter, our major custom-made table responsible for fetching the necessary bits from each table would also be heavily
damaged.

Here below, that's where we bring our table-shaped filter into action:

```sql
SELECT DISTINCT INDUSTRY, 
	YEAR,
	COUNT(COMPANY_ID) AS NUM_UNICORNS,
	ROUND(AVG(VALUATION) / 1000000000, 2) AS AVERAGE_VALUATION_IN_BILLIONS 
				
FROM QUALIFIED_UNICORN_COMPANY_INFO --That's where I compiled all the required information, including only the years
--2019, 2020 and 2021 data required by the case study

WHERE INDUSTRY IN( --The WHERE Statement to execute our custom-made filter
	SELECT INDUSTRY
	FROM FILTER_FOR_TOP3_PERFORMING_INDUSTRIES --Our filter table
	)

GROUP BY INDUSTRY, YEAR
ORDER BY YEAR DESC, 3 DESC
```

### Findings

Our data analysis results are summarised as follows:

1. All resulting top 3 industries are in one way or another tech-related, as follows:

	|Top 3 Industries|
	|--------|
	|Fintech|
	|Internet Software & Services|
	|E-Commerce & Direct-to-Consumer|
    
2. Compared to 2019 and 2020, all these three industries in 2021 experienced three-to-six-fold increases in the total number of unicorns that they gained.
   
3. The increasing number of unicorns for each industry however caused the sectors at stake to go down in average valuations.



### Recommendations

Based on the analysis, I recommend the following actions:

- Concentrate most efforts on these resulting industries for the purpose of discovering more potential unicorn companies ahead of other corporations.
- Take into account that increasing number of unicorns seems to have a negative correlation with corresponding industries' average valuation in billions.


### References

1. [DataCamp](https://www.datacamp.com/)
2. [CyberSec Unicorns by Gené Teare](https://news.crunchbase.com/venture/unicorn-board-october-2023-cybersecurity/)


