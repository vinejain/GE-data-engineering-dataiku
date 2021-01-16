# General Electric - Data Engineering Experience

#### Using data engineering to combine full-flight engine data, part manufacturing data, and airport location data to determine the distance travelled for each airplane and creating a run chart and KPI (key performance indicators) tables based off of the Aviation data.

![alt text](https://github.com/vinejain/GE-data-engineering-dataiku/blob/main/certificate.jpg)

##### GE Aviation, and many other MNCs use the concept of a “Data Lake”.  A Data Lake is a single database instance that contains data from different sources, for example at GE Aviation everything from financial, delivery (of parts and engines), supplier, engine data, customer data, and so on will be present in their Data Lake. The advantage of using a Data Lake allows the developer to make a single connection string to the Data Lake, and as long as the developer has permissions to see the data – they are able to immediately start creating data-driven insights in a centralized repository of data. 

#### Task: create a single data set that combines all the 8 tables into a single table. In this final form, we can then use this table to either create a prediction of RUL or visualize the data for easy user consumption. 

#### Tools: We are provided with an instance using the Dataiku tool. To access the platform, open Dataiku in your internet browser using the link below.

http://18.224.5.133:11000/

### Data Engineering Pipeline

![alt text](https://github.com/vinejain/GE-data-engineering-dataiku/blob/main/recipe-till-inner-join-final-table.jpg)


- Importing the 8 data tables:
```
- 	 av_engine_data_aic_psql
- 	 av_engine_data_axm_psql
- 	 av_engine_data_fron_psql
- 	 av_engine_data_pgt_psql
- 	 av_manufacturing_supply_chain_psql 
- 	 av_bom_manufacturing_psql
- 	 av_esn_rul_psql
- 	 av_lkp_airport_codes_t_psql
```

- Project Dashboard

![alt text](https://github.com/vinejain/GE-data-engineering-dataiku/blob/main/step-0.jpg)

- Importing the data sets creates a copy from the remote database into your local Dataiku session

![alt text](https://github.com/vinejain/GE-data-engineering-dataiku/blob/main/step-1.jpg)

- Union below tables to consolidate data
```
- 	 av_engine_data_aic_psql
- 	 av_engine_data_axm_psql
- 	 av_engine_data_fron_psql
- 	 av_engine_data_pgt_psql
```

- In table av_engine_data_aic create a formula to overwrite the column t24 with the new value of (t24 + 459.67). The reason we are doing this to only one table, is because this airline stored their t24 column in Rankine, the other airlines kept their temps in a standard format. We are storing the resulting data in the EC2 “dataiku-access” connection.

![alt text](https://github.com/vinejain/GE-data-engineering-dataiku/blob/main/step-2.2-run.jpg)

- nf column: slightly right skewed

![alt text](https://github.com/vinejain/GE-data-engineering-dataiku/blob/main/step-2.5-some-skewness-towards-right.jpg)

- ps30-column: some skewness towards right

![alt text](https://github.com/vinejain/GE-data-engineering-dataiku/blob/main/step-2.6-ps30-column.jpg)

- t30-column: almost a normal distribution

![alt text](https://github.com/vinejain/GE-data-engineering-dataiku/blob/main/step-2.7-t30-column.jpg)

- nc-column: right skewed

![alt text](https://github.com/vinejain/GE-data-engineering-dataiku/blob/main/step 2.8-nc-column.jpg)

- Calculate departure & destination latitude/longitudes for each row in the consolidated flight table. Join on destination_icao and airport_icao. Rename the latitude/longitude columns from the airport lookup table to be destination_latitude and destination_longitude. Join on depart_icao and airport_icao and rename lats and longs.

- Calculate each flight’s total distance and LPT temperature. 

```
Column Name: 
distance_between_airport_miles

Formula:
7917.5/2*atan2(sqrt(pow(cos(destination_latitude*3.14159/180)*sin(abs(destination_longitude*3.14159/180-depart_longitude*3.14159/180)),2)+pow(cos(depart_latitude*3.14159/180)*sin(destination_latitude*3.14159/180)-sin(depart_latitude*3.14159/180)*cos(destination_latitude*3.14159/180)*cos(abs(destination_longitude*3.14159/180-depart_longitude*3.14159/180)),2)),sin(depart_latitude*3.14159/180)*sin(destination_latitude*3.14159/180)+cos(depart_latitude*3.14159/180)*cos(destination_latitude*3.14159/180)*cos(abs(destination_longitude*3.14159/180-depart_longitude*3.14159/180)))

Column Name: 
t50 (total temperature at low pressure turbine – LPT outlet)

Formula:
if(t50<1410, t50,1410+2*(t50-1410))

```


- Building a supporting KPI table

- INNER JOIN* the manufacturing tables to create a table that has the KPIs (Key Performance Indicators) of each part and the engine serial number (or ESN) they associate with. Datasets used:  av_manufacturing_supply_chain & av_bom_manufacturing. Join on PN (Part Number) and SN (Serial Number).

![alt text](https://github.com/vinejain/GE-data-engineering-dataiku/blob/main/step-5-recipe.jpg)
![alt text](https://github.com/vinejain/GE-data-engineering-dataiku/blob/main/step-5-build.jpg)

- Join the new KPI table to the consolidated flights table
- Join final table with table from step 5 to get remaining useful life (RUL) for each engine. (RUL – shows the number of cycles remaining until an engine needs to be overhauled)

