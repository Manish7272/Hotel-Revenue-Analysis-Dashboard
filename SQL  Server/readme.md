
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






















