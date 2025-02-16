# Marketing Analytics Business Case
## I. Project Overview:
ShopEasy, an online retail business, is facing reduced customer engagement and conversion rates despite launching several new online marketing campaigns. They are reaching out to you to help conduct a detailed analysis and identify areas for improvement in their marketing strategies.

** Key Points:
- Reduced Customer Engagement: The number of customer interactions and engagement with the site and marketing content has declined.
- Decreased Conversion Rates: Fewer site visitors are converting into paying customers.
- High Marketing Expenses: Significant investments in marketing campaigns are not yielding expected returns.
- Need for Customer Feedback Analysis: Understanding customer opinions about products and services is crucial for improving engagement and conversions.

## II. Objective goals
- Increase Conversion Rates: Highlight key stages where visitors drop off and suggest improvements to improve it.
- Enhance Customer Engagement: Determine which types of content drive the highest engagement and analyze interaction levels with different types of marketing content to inform better content strategies.
- Improve Customer Feedback Scores: Understand common senses in customer reviews and identify recurring positive and negative feedback to guide product and service improvements.

## III. Tools & Technologies
Data Processing & Analysis: Python, Excel.

Data Visualization: Matplotlib, Power BI.

Sentiment Analysis: Natural Language Processing (NLP) libraries in Python.

## IV. Methodology

This project followed a structured data analysis process involving SQL, Python, and Power BI for end-to-end insights and recommendations.

### 1. Data Analysis with SQL
Process:
SQL was used to clean, preprocess, and analyze the raw data. Optimized queries were written to extract meaningful insights, such as identifying seasonal trends, conversion rates, and product performance.
#### 1.1 Merge dim_customers with dim_geography to show the geographic information

![Image](https://github.com/user-attachments/assets/3f81bd9c-70df-49a4-99ff-d890b9b8bc06)

```sh
SELECT 
    c.CustomerID,  
    c.CustomerName,  
    c.Email,  
    c.Gender, 
    c.Age,  
    g.Country,  
    g.City  
FROM 
    dbo.customers as c  
LEFT JOIN
-- RIGHT JOIN
-- INNER JOIN
-- FULL OUTER JOIN
    dbo.geography g  
    c.GeographyID = g.GeographyID;  -- Joins the two tables on the GeographyID field to match customers with their geographic information
```

![Image](https://github.com/user-attachments/assets/7742e008-0426-44ec-bf7b-558f3e6a9918)

Result:
Aggregated and filtered large datasets to calculate metrics like conversion rates, customer engagement, and average ratings. Used advanced SQL techniques such as CTEs, JOINS, and WINDOW FUNCTIONS to streamline the analysis process.


###2. Data Processing with Python
Process:
Python (in VS Code) was used to connect to the SQL server, automate data extraction, and perform additional data wrangling. Python libraries such as pandas, numpy, and matplotlib were utilized for deeper analysis and initial visualization.

Result:
Connected to the SQL database using libraries like pyodbc or sqlalchemy. Used Python scripts to handle and manipulate large datasets efficiently. Performed sentiment analysis on customer reviews using NLP libraries.

###3. Data Visualization with Power BI
Process:
Power BI was employed to create interactive and visually appealing dashboards for presenting insights. These dashboards were designed for easy stakeholder understanding and decision-making.

Result:
Integrated SQL server data directly into Power BI for real-time analysis. Built visualizations for trends in customer engagement, conversion rates, and sentiment analysis. Designed intuitive, actionable dashboards that showcased key performance metrics.

## VI. DAX Measures 
- Conversion Rate: Percentage of website visitors who make a purchase.
- Customer Engagement Rate: Level of interaction with marketing content (clicks, likes, comments).
- Average Order Value (AOV): Average amount spent by a customer per transaction.
- Customer Feedback Score: Average rating from customer reviews.




## V. Outcomes
Increase conversion rates through targeted product campaigns.

Drive higher customer engagement with optimized and engaging content strategies.

Improve overall customer satisfaction and ratings by addressing key pain points.

