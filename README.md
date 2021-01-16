# General Electric - Data Engineering Experience

#### Using data engineering to combine full-flight engine data, part manufacturing data, and airport location data to determine the distance travelled for each airplane and creating a run chart and KPI (key performance indicators) tables based off of the Aviation data.

![alt text](https://github.com/vinejain/GE-data-engineering-dataiku/blob/main/certificate.jpg)

##### GE Aviation, and many other MNCs use the concept of a “Data Lake”.  A Data Lake is a single database instance that contains data from different sources, for example at GE Aviation everything from financial, delivery (of parts and engines), supplier, engine data, customer data, and so on will be present in their Data Lake. The advantage of using a Data Lake allows the developer to make a single connection string to the Data Lake, and as long as the developer has permissions to see the data – they are able to immediately start creating data-driven insights in a centralized repository of data. 

#### Task: create a single data set that combines all the 8 tables into a single table. In this final form, we can then use this table to either create a prediction of RUL or visualize the data for easy user consumption. 

#### Tools: We are provided with an instance using the Dataiku tool.
#### To access the Dataiku platform, open Dataiku in your internet browser using the link below.

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


