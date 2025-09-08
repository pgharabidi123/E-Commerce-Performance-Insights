# E-Commerce-Performance-Insights
# Problem Statement

In today’s competitive e-commerce landscape, businesses face challenges in understanding user behavior, tracking customer journeys, and optimizing conversions. Although vast amounts of user data are generated daily through website sessions, clicks, and purchases, these raw data points often fail to provide actionable insights unless systematically analyzed.

This project focuses on analyzing e-commerce user activity data collected between January 2024 – June 2024, covering three main datasets:
Users dataset – demographic and type (new vs. returning users)
Sessions dataset – time, frequency, and device details of user sessions
Actions dataset – customer interactions such as page views, product clicks, add-to-cart events, and purchases

The goal is to design an interactive Power BI dashboard that helps answer critical business questions such as:

1. What are the overall traffic, conversion, and bounce patterns across users, devices, and geographies?
2. How do user journeys progress from page views to purchases, and where do most users drop off?
3. What are the peak hours, days, and traffic sources driving the highest engagement?
4. How do new users behave differently from returning users, and what impact does this have on cart abandonment?
5. Which countries and devices contribute the most to conversions?

By addressing these questions, the dashboard aims to enable data-driven decision making for:

1. Reducing cart abandonment
2. Improving customer acquisition and retention strategies
3. Optimizing marketing channels and traffic sources
4. Enhancing user experience across devices

# Dataset

**1. Users.csv** – Contains demographic and type information of users (new vs. returning) to analyze audience segments.

**2. Sessions.csv** – Logs details of user sessions such as timing, duration, device, and source for behavioral analysis.

**3. Actions.csv** – Tracks user interactions across the funnel (page views, clicks, add-to-cart, purchases) to measure engagement and conversions.

# Steps:

**1. Load data into Power BI:**
Open Power BI - Get Data - Load the three CSV files (Users.csv, Sessions.csv, Actions.csv) - Convert binary to tables and ensure proper headers.

**2. Data Transformation:**
1. Created an Action Date Table to analyze user interactions such as Page View, Product Click, Add to Cart, Purchase Completed across time for time-series analysis by Year, Month, Quarter, Day and Weekday.
- DAX Formula:
Action_Date_Table = CALENDAR(MIN(Actions[Action_Timestamp]),MAX(Actions[Action_Timestamp]))
WeekDay = FORMAT('Action_Date_Table'[Date],"ddd")

2. Created an Session Date Table to analyze user session across time for time-series analysis by Year, Quarter, Month, Weekday, and Day
- DAX Formula:
Session_Date_Table = CALENDAR(MIN(Sessions[Session_Start]),MAX(Sessions[Session_Start]))

3. User Demographics Transformation:
- Classify users into New and Returning categories.
- Segment by Gender and Country for detailed behavioral analysis

4. Device Classification:
- Standardize device categories into Mobile, Tablet, and Desktop for consistent reporting.

**3. Manage Relationship:**
Create a schema relationship between tables
<img width="1025" height="738" alt="Screenshot 2025-09-07 215652" src="https://github.com/user-attachments/assets/94350a92-42d3-44ae-8851-d4b4e6471ed5" />

**4. Create Measures:**
1. **Total Session** = COUNTROWS(Sessions)

2. **Total Purchases** = 
CALCULATE(COUNTROWS(Actions), 
    Actions[Action_Type] = "Purchase Completed")

3. **Add to Cart** = 
CALCULATE(COUNTROWS(Actions), Actions[Action_Type] = "Add to Cart")

4. **Cart Abandonment %** = 
DIVIDE([Add to Cart] - [Total Purchases], [Add to Cart])

5. **Total Action** =
COUNTROWS(Actions)

6. **Conversion Rate(%)** = 
DIVIDE([Total Purchases], [Total Session])

7. **Bounce Rate%** = 
DIVIDE ( [Bounced Sessions], COUNTROWS(Sessions), 0 )

8. **Avg Duration (mins)** =
AVERAGE(Sessions[Session_Duration(mins)])

9. **Page View** =
DIVIDE(COUNTROWS(FILTER(Actions,Actions[Action_Type]="Page View")),[Total Action]) 

10. **Product Click** =
DIVIDE(COUNTROWS(FILTER(Actions,Actions[Action_Type]="Product Click")),[Total Action]) 

11. **Add to Cart Cnt** = 
DIVIDE(COUNTROWS(FILTER(Actions,Actions[Action_Type]="Add to Cart")),[Total Action]) 

12. **Tot Purchase** =
DIVIDE(COUNTROWS(FILTER(Actions,Actions[Action_Type]="Purchase Completed")),[Total Action]) 

13. **New Users (flag)** =
CALCULATE(DISTINCTCOUNT(Users[User_ID]),Users[New_Visitor]="Yes") 

13. **Returning Users (flag)** =
CALCULATE( DISTINCTCOUNT( Users[User_ID] ), Users[New_Visitor] = "No" )
