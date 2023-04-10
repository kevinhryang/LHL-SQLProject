# Final-Project-Transforming-and-Analyzing-Data-with-SQL

## Project/Goals
First project of the Lighthouse Labs Data Science bootcamp. Focused on preliminary data exploration. Goal is to clean and transform the data while following some set of quality assurance guidelines in order to gain a working knowledge of the dataset.

## Process
### Look at the data
Open up the csv files in some text editor and look for patterns, possible datatypes for attributes. Begin to outline any QA guidelines that could be relevant. Try to connect datasets by common attributes or keys, and devise a header naming convention to keep things uniform.
### Load the data
Bring the data into pgAdmin following the naming conventions previously decided upon.
### Clean the data
Look for duplicate information between rows, columns, and tables. Decide which duplicates to keep, and which to delete. Impute null values where it makes obvious sense, leave as null if it doesn't.
### Transform the data
If needed, create new tables or split existing ones to simplify structure. Alter data into more useful or workable formats.
### Answer Questions
Finally begin formal data exploration and answer questions using SQL queries.

## Results
This seems to be a record of web traffic and sales for some international company that delivers a variety of products. It spans a year, beginning at August 1, 2016. 

All visits to the website are recorded, tracking visitors, what page they viewed, whether they purchased a product, and in what quantities. Every product is also listed, along with its name, unit price, quantity in stock, total number ordered, and sentiment scores. 

## Challenges 
On the technical side, pgAdmin crashed. A lot. Also, analytics.csv was a huge file that couldn't be viewed easily. One way to make sense of it was to pull out the first 100 lines or so. Another way was to use bash commands to view specific columns that seemed almost empty.

> cat analytics.csv | cut -d ',' -f13

As well, there were a few attributes that were difficult to impute. Filling in a value could be misleading, and so NULL seemed to be a fine stand in until more domain knowledge was acquired.

## Future Goals
- split all_sessions into multiple tables, possibly consolidating information from other tables to get rid of unnecessary entries
- figure out how to do one to one ERD relationships in pgadmin
- fix lingering consistency issues between foreign/primary key data types
- come up with a system for imputing the remaining null values in the database