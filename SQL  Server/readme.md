
### we will follow the some steps to build the best portfolio as a data analyst:

1. Create a Database
2. Query and analyze data with SQL
3. Integrate Power BI with a Database
4. Create Data Visualizations Using Power BI

# 1. Create a Database
Let’s start by creating a database in SSMS (SQL Server Management Studio) for further analysis of Hotel Booking Data.

We will create a database by following these steps:

  - Open SQL Server Management Studio and you will see a new window on your screen.

pic 1 ----------------

  - Copy the Server Name for later use, then click on Connect.
  <br>
  
  - Now on SSMS window you will see the following options.

pic ---------------

  - Right click on Databases and select New Database from the drop down.
pic -----------


  - Assign a Database Name in New Database window and click OK.

  - pic -------------------

  - Expand the Databases and you can see Project database is created.
pic --------------

  - In the Databases pane, right-click the Project database and select Import Data from Task drop down list.
pic - ---------------


  - In the Import Data dialog box, select the data source that you want to import from.
  - Browse data file or enter path.
  - Choose the excel version you want to use and click Next.
  - pic --4 ------------

Note: If you are facing the following error on clicking Next just download Microsoft Access Database Engine 2016.

pic - import export error


  - Select a destination and verify the server and database name.
pic ----------


  - Select tables that you want to import.
pic --------------

  - Now, when you run it immediately, you will see that all the data is imported into the environment.
pic ----------------

  - You can then view the data in the Project database by expanding the Tables node in the Databases pane.
pic ------------


# 2. Querying data
We have now prepared our data tables in the database for SQL commands. Let’s now apply some commands to explore the data.


We have now prepared our data tables in the database for SQL commands. Let’s now apply some commands to explore the data.

### Fetching Data from Tables
To fetch data to view from the tables, we will use the following command. The wildcard ‘*’ operator is used to retrieve all the data from the table.


    select * from dbo.['2018$']
    select * from dbo.['2019$']
    select * from dbo.['2020$']


After executing the commands, the data will be displayed like this:
pic ----------


Combining the Data
To combine the data from the three tables, we simply use the UNION operator among these commands.



    select * from dbo.['2018$']
    union
    select * from dbo.['2019$']
    union
    select * from dbo.['2020$']
img -=-------




# 3. Exploratory Data Analysis (EDA)
Now, we are going to apply EDA on the data and try answer the following questions.

1. Is our hotel revenue growing yearly?
2. Should we increase our parking lot size?
3. What trends can we see in the data?

Before answering the questions, we will first create a single temporary table hotels that combines all the data using following code for easier access and analysis.


    with hotels as(
    select * from dbo.['2018$']
    union
    select * from dbo.['2019$']
    union
    select * from dbo.['2020$'])
    
    select * from hotels



## Q.1: Is our hotel revenue growing yearly?
In our dataset we don’t have revenue, but we do have adr (Average Daily Rate), stays_in_week_nights, and stays_in _weekend_nights. So, we will create a new column revenue by using the data of these three columns as follows.

    select 
      (stays_in_week_nights + stays_in_weekend_nights) * adr
      as revenue from hotels

The revenue column is here.
img -----------


Let’s bring another column arrival_date_year from the data and then calculate the sum of revenue while grouping the data by year.

    select 
    arrival_date_year
    sum((stays_in_week_nights + stays_in_weekend_nights) * adr)
    as revenue from hotels group by arrival_date_year



Now, in the following table, we can see that the revenue increased from 2018 to 2019 but then decreased again in 2020.

img -----------



We can also determine the revenue trend by hotel type by grouping the data by hotel and then seeing which hotels have generated the most revenue.

    select 
    arrival_date_year, hotel,
    sum((stays_in_week_nights + stays_in_weekend_nights) * adr)
    as revenue from hotels group by arrival_date_year, hotel

img --------------


## Q.2: Should we increase our parking lot size?
To answer this question, we will focus on the car_parking_spaces and number of guests staying in the hotel. So, let’s do it by applying the following SQL query.

    select
    arrival_date_year, hotel,
    sum((stays_in_week_nights + stays_in_weekend _nights) * adr) as renenue,
    concat (round((sum(required_car_parking_spaces)/sum(stays_in_week_nights +
    stays_in_weekend_nights)) * 100, 2), '%') as parking_percentage
    from hotels group by arrival_date_year, hotel


In the next table we can observe that we have enough space for parking. So, there is no need to increase our parking lot size.

img -------------

---------------------------------------------------------------------------------------------------------------------------------------------------

# 4. Create Data Visualizations Using Power BI
Before moving to Power BI, we need to preprocess some columns.

Using SQL, we will perform two left join queries on the data.

First Left Join: Combines the hotels table with the market_segment table by matching the market_segment column in the “hotels” table with the market_segment.market_segment column.

Second Left Join: Combines the hotels table with the meal_cost table by matching the meal column in the hotels table and the meal_cost.meal column.



    with hotels as(
    select * from dbo.['2018$']
    union
    select * from dbo.['2019$']
    union
    select * from dbo.['2020$'])
    
    select * from hotels
    left join dbo.market_segment$
    on hotels.market_segment = market_segment$.market_segment
    left join dbo.meal_cost$
    on meal_cost$.meal = hotels.meal


Now open Power Bi on your PC to connect data base.

Click on Get Data then select SQL Server.
img ------------------------

  - A new window titled SQL Server Database will appear.
  - Enter your server’s name and database name in the respective fields.
  - Expand the Advanced options and paste the following code in the “SQL text” field.
  - And click Ok.




        with hotels as(
        select * from dbo.['2018$']
        union
        select * from dbo.['2019$']
        union
        select * from dbo.['2020$'])
    
        select * from hotels
        left join dbo.market_segment$
        on hotels.market_segment = market_segment$.market_segment
        left join dbo.meal_cost$
        on meal_cost$.meal = hotels.meal

img ----------




  - Now you can see that we have brought in our SQL information that we need.
  - Hit Load button to load the data.

img -------


# Q.3: What trends can we see in the data?
We have created some visuals using Power BI that show some possible trends. Here are a few of them:

1. Revenue increased from 2018 to 2019, but it began to decrease from 2019 to 2020.
2. The average daily rate (ADR) has increased from 2019 to 2020, from $99.53 to $104.47.
3. Total number of nights booked by customers decreased from 2019 to 2020.
4. The discount percentage offered by the hotel has increased from 2019 to 2020 to attract more customers.

# DashBoard





























