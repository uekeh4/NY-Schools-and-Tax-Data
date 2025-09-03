# NY-Schools-and-Tax-Data
This project, a comprehensive data analysis of NYC public schools, leverages a relational database created in PostgreSQL and hosted on AWS RDS. The database was populated by integrating two distinct datasets: one containing NYC public school information and another with New York State taxpayer records, joined by ZIP code. Using Python, I conducted a location-based analysis to map over 1,000 schools across more than 150 ZIP codes to their respective boroughs and school districts. This enabled a granular spatial and socioeconomic analysis. By querying the database, I calculated key metrics such as average neighborhood income, schools per neighborhood, and regional tax patterns, ultimately identifying and visualizing significant disparities in educational access and income distribution throughout New York City.

### The Data
#### About the Data
- The first dataset is from the NYC Department of Education containing information on schools. This can include the neighborhood of 
- The second dataset comes from the IRS, detailing tax information for different zip codes in New York State. This includes number of returns, average amount per returns, and zip codes


### Objectives
The main objective of this project is to create a robust data pipeline that can be used to query and analyze the relationship between income levels and school availability in New York City. The process involves:

- Extracting raw csv data

- Transforming and cleaning the data using Python's Pandas library.

- Loading the cleaned data into a relational database (PostgreSQL) using Python.

- Analyzing the data with SQL queries to gain insights into neighborhood demographics and educational infrastructure.


### Schema Directory
This table describes the final schema of the data loaded into the PostgreSQL database, which is designed to be efficient for querying and analysis.

#### schools		
| Feature | Definition | DataType |
|---|---|---|
| BEDS	| A unique identifier for each school | Primary Key	str |
| School_Name |	The name of the school | str |
| School_Type	| The management type of the school |	str |
| Grade_Levels | The grades served by the school | str |
| Address |	The physical address of the school | str |
| Neighborhood | The neighborhood the school is located in | str |
| zip_id | A foreign key referencing the tax_ny table | Foreign key str |

#### returns		
| Feature | Definition | DataType |
|---|---|---|
| zip_id | A unique identifier for each zip code | Primary Key str |
| Number_Returns	| The number of tax returns filed in the zip code |	int |
|Number_Returns_Taxable_Income | The number of tax returns with taxable income | int |

#### tax_ny
| Feature | Definition | DataType |
|---|---|---|
| Zipcode |	The actual zip code |	str |
| Avg_AGI | The average adjusted gross income for the zip code | int |
| Avg_Total_Income | The average total income for the zip code | int |
| zip_id | A unique identifier for each zip code | Primary Key  str |


### Outcome
The project successfully created a clean, relational database that facilitates powerful analysis. The SQL queries demonstrated key insights:

__Query 3:__ Found that lower-income areas have a higher concentration of schools per capita than higher-income areas, suggesting a disparity in educational access.

__Query 4:__ Identified that neighborhoods with a high average income per tax return, such as `Kingsbridge Heights` and `Queens Village`, primarily have Department of Education schools.

__Query 5:__ Revealed that the neighborhoods with the most schools are not necessarily the highest-income areas. For example, `East Concourse-Concourse Village` and `East New York` have high school counts despite lower average incomes compared to some other neighborhoods.


### Methods and Technology
Languages: Python and SQL.

- Data Format: The project started with raw CSV data from Google Sheets.

- Data Processing: Python's Pandas library was used for the Extract and Transform phases. This involved:

  - Extracting data directly from Google Sheets URLs.

  - Cleaning and filtering the data to remove irrelevant columns (e.g., State, Principal), redundant information (Number of returns vs. Number of returns with total income), and incomplete rows (NAs).

  - Standardizing column names and data types to ensure consistency.

  - Normalizing the data into three separate tables (tax_ny, returns, and schools) to reduce redundancy and optimize the database structure. A zip_id was created to serve as a common key for linking the tables.

- Database Loading: Python was used to connect to a PostgreSQL database. The cleaned Pandas DataFrames were converted into tuples and loaded into the respective tables using psycopg2's executemany function, which is efficient for bulk data insertion.

- Analysis: After loading, SQL queries were used to join the tables and perform aggregations and calculations, such as finding the average income per return and counting schools per neighborhood.

### Conclusion and Future Work
This project demonstrates a complete ETL pipeline that transforms raw, disparate data into a structured and analyzable database. The analysis provided valuable insights into the distribution of educational resources in relation to income levels across different NYC neighborhoods.

__Query 3:__ Reveals a clear pattern of educational disparity, where lower-income areas have a higher density of schools per tax return, likely due to public policy or historical development.

__Query 4:__ Shows that high-income neighborhoods predominantly have Department of Education schools, which could be a topic for further investigation into resource allocation within the public school system.

__Query 5:__ Confirms that the number of schools is not directly correlated with neighborhood income, indicating that school density is influenced by factors other than wealth.

##### Future Insights
Future work could involve integrating more data sources, such as student performance metrics, teacher-to-student ratios, or school funding data, to build a more comprehensive model. Advanced analysis could include machine learning models to predict educational outcomes based on neighborhood demographics and school infrastructure.
