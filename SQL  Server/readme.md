
### we will follow the some steps to build the best portfolio as a data analyst:

1. Create a Database
2. Query and analyze data with SQL
3. Integrate Power BI with a Database
4. Create Data Visualizations Using Power BI

# 1. Create a Database
Let’s start by creating a database in SSMS (SQL Server Management Studio) for further analysis of Hotel Booking Data.

We will create a database by following these steps:

  - Open SQL Server Management Studio and you will see a new window on your screen.
![Screenshot 2024-05-27 203548](https://github.com/Manish7272/Hotel-Revenue-Analysis-Dashboard/assets/71213166/4b5fade3-44f3-4d57-94b1-dc0ea15254c1)



  - Copy the Server Name for later use, then click on Connect.
  <br>
  
  - Now on SSMS window you will see the following options.
![Screenshot 2024-05-27 203832](https://github.com/Manish7272/Hotel-Revenue-Analysis-Dashboard/assets/71213166/dee4a8bf-62a7-48a5-9a74-628c607e864f)



  - Right click on Databases and select New Database from the drop down.
  - ![Screenshot 2024-05-27 203910](https://github.com/Manish7272/Hotel-Revenue-Analysis-Dashboard/assets/71213166/c40563c8-5b96-45ed-95ce-21449901b61a)



  - Assign a Database Name in New Database window and click OK.
![Screenshot 2024-05-27 204041](https://github.com/Manish7272/Hotel-Revenue-Analysis-Dashboard/assets/71213166/8c6ddcbe-2b77-40f4-bd0a-9a63bc8a375f)



  - Expand the Databases and you can see Project database is created.

![Screenshot 2024-05-27 204105](https://github.com/Manish7272/Hotel-Revenue-Analysis-Dashboard/assets/71213166/1230412d-17b0-4b19-a52f-dc8c019b631e)



  - In the Databases pane, right-click the Project database and select Import Data from Task drop down list.
![Screenshot 2024-05-27 204158](https://github.com/Manish7272/Hotel-Revenue-Analysis-Dashboard/assets/71213166/02599870-d598-49b4-a0f4-1252534ed8e1)



  - In the Import Data dialog box, select the data source that you want to import from.
  - Browse data file or enter path.
  - Choose the excel version you want to use and click Next.
![Screenshot 2024-05-27 204444](https://github.com/Manish7272/Hotel-Revenue-Analysis-Dashboard/assets/71213166/080b3e31-4e91-4281-b888-78db05a1bdfc)


  
Note: If you are facing the following error on clicking Next just download Microsoft Access Database Engine 2016.
![Screenshot 2024-05-27 215419](https://github.com/Manish7272/Hotel-Revenue-Analysis-Dashboard/assets/71213166/bd1519a2-3920-4580-b365-5b115166584e)



  - Select a destination and verify the server and database name.
![Screenshot 2024-05-27 215537](https://github.com/Manish7272/Hotel-Revenue-Analysis-Dashboard/assets/71213166/fb6c505a-e47d-4516-ba02-1f3f612869c2)


  - Select tables that you want to import.
![Screenshot 2024-05-27 204818](https://github.com/Manish7272/Hotel-Revenue-Analysis-Dashboard/assets/71213166/8cf2fc8f-3804-403d-a6cb-0472e37b6fa5)



  - Now, when you run it immediately, you will see that all the data is imported into the environment.
![Screenshot 2024-05-27 210058](https://github.com/Manish7272/Hotel-Revenue-Analysis-Dashboard/assets/71213166/49337a5c-c2ec-41db-8638-927febe5ffc4)

![Screenshot 2024-05-27 210143](https://github.com/Manish7272/Hotel-Revenue-Analysis-Dashboard/assets/71213166/f6b5095f-9f88-45d0-b65e-6a25ce278803)



  - You can then view the data in the Project database by expanding the Tables node in the Databases pane.
![Screenshot 2024-05-27 210211](https://github.com/Manish7272/Hotel-Revenue-Analysis-Dashboard/assets/71213166/ecf5e5b4-187b-4f70-b4bc-8620ccb3dc41)

![Screenshot 2024-05-27 210311](https://github.com/Manish7272/Hotel-Revenue-Analysis-Dashboard/assets/71213166/d7341ae8-8255-4e3f-aa96-03ab16f2bcd2)





<br>
<br>
<br>
<br>


# 2. Querying data 
We have now prepared our data tables in the database for SQL commands. Let’s now apply some commands to explore the data.


We have now prepared our data tables in the database for SQL commands. Let’s now apply some commands to explore the data.

### Fetching Data from Tables
To fetch data to view from the tables, we will use the following command. The wildcard ‘*’ operator is used to retrieve all the data from the table.


    select * from dbo.['2018$']
    select * from dbo.['2019$']
    select * from dbo.['2020$']


After executing the commands, the data will be displayed like this:
![Screenshot 2024-05-27 210906](https://github.com/Manish7272/Hotel-Revenue-Analysis-Dashboard/assets/71213166/67fcddf3-caff-4695-bc3b-b8b1b9f816a5)



Combining the Data
To combine the data from the three tables, we simply use the UNION operator among these commands.



    select * from dbo.['2018$']
    union
    select * from dbo.['2019$']
    union
    select * from dbo.['2020$']
    
![Screenshot 2024-05-27 211139](https://github.com/Manish7272/Hotel-Revenue-Analysis-Dashboard/assets/71213166/0c7eccc4-4511-4f7d-b880-398aacddfeec)










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
![Screenshot 2024-05-27 211316](https://github.com/Manish7272/Hotel-Revenue-Analysis-Dashboard/assets/71213166/815fc15e-95cb-43e6-a042-6de10754eaf7)



Let’s bring another column arrival_date_year from the data and then calculate the sum of revenue while grouping the data by year.

    select 
    arrival_date_year
    sum((stays_in_week_nights + stays_in_weekend_nights) * adr)
    as revenue from hotels group by arrival_date_year



Now, in the following table, we can see that the revenue increased from 2018 to 2019 but then decreased again in 2020.
![Screenshot 2024-05-27 211524](https://github.com/Manish7272/Hotel-Revenue-Analysis-Dashboard/assets/71213166/94b2879f-7f31-43ed-ad74-1f75ac332e31)



We can also determine the revenue trend by hotel type by grouping the data by hotel and then seeing which hotels have generated the most revenue.

    select 
    arrival_date_year, hotel,
    sum((stays_in_week_nights + stays_in_weekend_nights) * adr)
    as revenue from hotels group by arrival_date_year, hotel
![Screenshot 2024-05-27 211606](https://github.com/Manish7272/Hotel-Revenue-Analysis-Dashboard/assets/71213166/b1f09663-6783-45f0-b563-a70c497f5681)



## Q.2: Should we increase our parking lot size?
To answer this question, we will focus on the car_parking_spaces and number of guests staying in the hotel. So, let’s do it by applying the following SQL query.

    select
    arrival_date_year, hotel,
    sum((stays_in_week_nights + stays_in_weekend _nights) * adr) as renenue,
    concat (round((sum(required_car_parking_spaces)/sum(stays_in_week_nights +
    stays_in_weekend_nights)) * 100, 2), '%') as parking_percentage
    from hotels group by arrival_date_year, hotel


In the next table we can observe that we have enough space for parking. So, there is no need to increase our parking lot size.
![Screenshot 2024-05-27 220227](https://github.com/Manish7272/Hotel-Revenue-Analysis-Dashboard/assets/71213166/70d1ed39-38f1-4c17-82ea-6a31cb46e387)



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

![Screenshot 2024-05-27 214751](https://github.com/Manish7272/Hotel-Revenue-Analysis-Dashboard/assets/71213166/c74ab057-630e-4d58-9b2d-e448806532da)



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
![Screenshot 2024-05-27 212711](https://github.com/Manish7272/Hotel-Revenue-Analysis-Dashboard/assets/71213166/72a3f279-b4f5-429f-baa2-1bd4ccb1cb57)



  - Now you can see that we have brought in our SQL information that we need.
  - Hit Load button to load the data.
![Screenshot 2024-05-27 212729](https://github.com/Manish7272/Hotel-Revenue-Analysis-Dashboard/assets/71213166/335510a9-9e16-4306-93e9-f5accc388d2f)



# Q.3: What trends can we see in the data?
We have created some visuals using Power BI that show some possible trends. Here are a few of them:

1. Revenue increased from 2018 to 2019, but it began to decrease from 2019 to 2020.
2. The average daily rate (ADR) has increased from 2019 to 2020, from $99.53 to $104.47.
3. Total number of nights booked by customers decreased from 2019 to 2020.
4. The discount percentage offered by the hotel has increased from 2019 to 2020 to attract more customers.

# DashBoard




