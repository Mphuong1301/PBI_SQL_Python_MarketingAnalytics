# Marketing Analytics Business Case
## I. Project Overview:
ShopEasy, an online retail business, is facing reduced customer engagement and conversion rates despite launching several new online marketing campaigns. They are reaching out to you to help conduct a detailed analysis and identify areas for improvement in their marketing strategies.

**Key Points:**
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
**Process:**
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

![Image](https://github.com/user-attachments/assets/361deb19-b397-48ff-93c2-744531752d9a)

#### 1.2 Categorize products based on their price (Low, Medium, High)

```sh
SELECT 
    ProductID,  
    ProductName,  
    Price,  

    CASE
        WHEN Price < 50 THEN 'Low'  -- If the price is less than 50, categorize as 'Low'
        WHEN Price BETWEEN 50 AND 200 THEN 'Medium'  -- If the price is between 50 and 200 (inclusive), categorize as 'Medium'
        ELSE 'High'  -- If the price is greater than 200, categorize as 'High'
    END AS PriceCategory 

FROM 
    dbo.products;
```

![Image](https://github.com/user-attachments/assets/5258efcb-25ed-47a4-a336-a24e779e0bfc)

#### 1.3 Clean and normalize the engagement_data table

```sh
SELECT 
    EngagementID,  
    ContentID,  
	CampaignID,  
    ProductID, 
    UPPER(REPLACE(ContentType, 'Socialmedia', 'Social Media')) AS ContentType,  -- Replaces "Socialmedia" with "Social Media" and then converts all ContentType values to uppercase
    LEFT(ViewsClicksCombined, CHARINDEX('-', ViewsClicksCombined) - 1) AS Views,  -- Extracts the Views part from the ViewsClicksCombined column by taking the substring before the '-' character
    RIGHT(ViewsClicksCombined, LEN(ViewsClicksCombined) - CHARINDEX('-', ViewsClicksCombined)) AS Clicks,  -- Extracts the Clicks part from the ViewsClicksCombined column by taking the substring after the '-' character
    Likes,  -- Selects the number of likes the content received
    FORMAT(CONVERT(DATE, EngagementDate), 'dd.MM.yyyy') AS EngagementDate
FROM 
    dbo.engagement_data  -- Specifies the source table from which to select the data
WHERE 
    ContentType != 'Newsletter';  
```

![Image](https://github.com/user-attachments/assets/7742e008-0426-44ec-bf7b-558f3e6a9918)

#### 1.4 Clean whitespace issues in the ReviewText column

```sh
SELECT 
    ReviewID,  
    CustomerID,  
    ProductID, 
    ReviewDate,  
    Rating,  
    -- Cleans up the ReviewText by replacing double spaces with single spaces to ensure the text is more readable and standardized
    REPLACE(ReviewText, '  ', ' ') AS ReviewText
FROM 
    dbo.customer_reviews;
```
![Image](https://github.com/user-attachments/assets/47ad5d86-38be-4bae-9fb7-cf2e052d3ae2)

#### 1.5 Identify and tag duplicate records in 

```sh
WITH DuplicateRecords AS (
    SELECT 
        JourneyID,  
        CustomerID,  
        ProductID,  
        VisitDate, 
        Stage,
        Action, 
        Duration, 
        -- Use ROW_NUMBER() to assign a unique row number to each record within the partition defined below
        ROW_NUMBER() OVER (
            PARTITION BY CustomerID, ProductID, VisitDate, Stage, Action  
            ORDER BY JourneyID  
        ) AS row_num  
    FROM 
        dbo.customer_journey  
)

-- Select all records from the CTE where row_num > 1, which indicates duplicate entries
    
SELECT *
FROM DuplicateRecords
ORDER BY JourneyID

SELECT 
    JourneyID, 
    CustomerID,  
    ProductID,  
    VisitDate, 
    Stage,  
    Action,  
    COALESCE(Duration, avg_duration) AS Duration  
FROM 
    (SELECT 
            JourneyID,  
            CustomerID,  
            ProductID,  
            VisitDate,  
            UPPER(Stage) AS Stage,  
            Action, 
            Duration,  
            AVG(Duration) OVER (PARTITION BY VisitDate) AS avg_duration,  
            ROW_NUMBER() OVER (
                PARTITION BY CustomerID, ProductID, VisitDate, UPPER(Stage), Action  
                ORDER BY JourneyID  
            ) AS row_num  
        FROM 
            dbo.customer_journey 
    ) AS subquery  
WHERE 
    row_num = 1;
```

![Image](https://github.com/user-attachments/assets/e533aea8-3ded-47a3-b20a-a7ebf61c725e)

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

