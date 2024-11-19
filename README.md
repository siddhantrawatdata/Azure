# AirBnB CDC Data Pipeline

## Introduction
We have customer and Bookings data for AirBnB. The customer data is ingested into the ADLS on a half-hourly basis, the booking data is ingested in the cosmosDB by an external Application. 
For the test purposes we are generating data in the cosmosDB by the use of a Python script. 

# Block diagram


# Data flow
- Files containing the customer data are ingested to ADLS (customer_yyyy_mm_dd_hhmiss.csv)
- The mock data generation pipeline will insert the bookings data in the cosmosDB
- ### ADF- Pipeline to load the customer dimension data
  - Get the list of metadata of the files from the ADLS
  - loop over the files and copy data into the synapse table
    - upsert data to the synapse table
    - move the data from the working folder to the archive
    - delete the file from the working folder  
- ### ADF- Pipeline to load the bookings data
  - Source the bookings data from CosmosDB
  - Filter out the bad records (Records whose check-in-date is more than the check-out-date)
  - Derive the required columns from already existing columns by applying the expressions
  - Lookup transformation to check if the booking_id coming from the cosmosDB is already existing in the bookings fact table
    - The Ids that already existing gets updated and the Ids that are new are inserted
  - Finally we insert the data in Synapse Table and use a stored procedure for Truncating and reloading aggregated data in booking table
- ### ADF - Pipeline to run the customer data and booking data sequentially

### ADF- Pipeline to load the customer dimension data
![image](https://github.com/user-attachments/assets/17e1ed45-f558-4391-9744-e4a6f9bad24a)

### ADF- Pipeline to load the bookings data
![image](https://github.com/user-attachments/assets/65db62bc-2b82-488b-b371-1d8d8d5fddf1)

![image](https://github.com/user-attachments/assets/88e5a816-ff0f-4278-b19e-5a149b254c61)
### ADF - Pipeline to run the customer data and booking data sequentially
![image](https://github.com/user-attachments/assets/8aeaf24e-25c1-4591-b008-b2c25dc8ce0a)



