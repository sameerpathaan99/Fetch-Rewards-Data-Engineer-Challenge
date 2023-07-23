# Fetch-Rewards-Data-Engineer-Challenge
This challenge will focus on your ability to write a small application that can read from an AWS SQS Queue, transform that data, then write to a Postgres database. The project is run using the docker images of localstack (used to run SQS locally) and Postgresql (used for running Database locally).

## Objective: The objective of this task was to create an ETL (Extract, Transform, Load) application that performs the following operations
Extract: Read JSON data containing user login behavior from an AWS SQS Queue.

Transform: Before processing the data, mask Personal Identifiable Information (PII) fields device_id and ip in a way that allows data analysts to identify duplicate values.

Load: Write each record into a Postgres database.

## Environment Setup
1. Docker Installation: We need to download Docker Desktop using homebrew or virtually by going to through the site [Docker][https://docs.docker.com/get-docker/] . On installing the Docker Desktop, Docker-compose is also enabled. Just to go through the terminal and check if the they are available by using the commands mentioned below.
Commands Used:
**brew install docker**
**Docker --version**
**Docker-compose --version**
2. Install AWSCLI using command: **pip install awscli-local** 
3. Install Postgresql client: **brew install postgresql**  
4. Pull necessary docker images and start the docker using the following links provided in the Task
5. Create Docker-compose file (docker-compose.yml) by using vi or nano in Terminal and copy the content provided in the challenge to check whether the environment setup is ready to develop the code.

## Environment Testing 
Run the below commands to check if everything is in order:
* To check if the SQS is running locally: awslocal sqs receive-message --queue-url http://localhost:4566/000000000000/login-queue
* TO check if the postgresql is running locally: psql -d postgres -U postgres -p 5432 -h localhost -W
Then login the credentials provided and try to run a select query on **user_logins** table. We should see that the table is empty. Now our aim is to load this table with the data extract using AWS SQS from Json.

## Basic Understanding
Now to proceed with the development of the application, we can go forward easily if we are able to figure out all the below questions. SO let's answer all of these questions to get a clear picture
**How will you read messages from the queue?** The messages are read with the help of boto3 library to connect to the SQS Queue and fetch messages containing user login behavior in JSON format. 

**What type of data structures should be used?** Json, Tuple, List, Hash-Table, Dictionary are the main ones used in this project. However, it may change if we are using any programming language other than Python

**How will you mask the PII data so that duplicate values can be identified?** In this exercise, PII data such as 'device_id' and 'ip' is masked using SHA-256 hashing, which generates unique and irreversible hash codes for each value. This allows for secure storage of data while preserving the ability to identify duplicate values based on their corresponding hashed representations.

**What will be your strategy for connecting and writing to Postgres?** The strategy for connecting and writing to Postgres involves using psycopg2 Python library to establish a connection, execute SQL statements for table creation and data insertion, and finally, commit transactions to persist the masked PII data securely.

**Where and how will your application run?** The application can be run locally on the machine or in a Docker container. Since the project includes steps for using Docker to run all the components locally, it's recommended to use Docker to create an isolated environment that includes Postgres and Localstack services. we can then execute this application within this Docker environment, allowing for easy setup and testing without the need for an AWS account. Once the application is developed and tested, it can also be deployed to a cloud platform or server for production use.

## Development, Execution and Validation
### Development
1. write a executable python code - [code.ipynb][https://github.com/sameerpathaan99/Fetch-Rewards-Data-Engineer-Challenge/blob/main/code.ipynb]
2. Established connections with the AWS SQS Queue using the boto3 library and with the local PostgreSQL database using the psycopg2 library.
3. Created a helper function mask_pii to hash the 'device_id' and 'ip' fields for data security and privacy.
4. Transformed app_version field to an integer format.
5.Created a loop to continuously receive messages from the SQS queue until the queue was empty. Each message was processed (parsed and masked), and then loaded into the user_logins table in the PostgreSQL database.
6. After successful processing and loading of each message, the message was deleted from the SQS queue.
7. On completion, the connection to the PostgreSQL database was closed.

### Code Explaination
Now let's break down the provided Python script that satisfies the above requirements:
1. Set up connections: The script first sets up connections with AWS SQS (Simple Queue Service) and a local PostgreSQL database using the boto3 and psycopg2 libraries respectively.
2. Mask PII: It defines a helper function mask_pii which is used to hash (mask) the fields device_id and ip in the JSON data object. Hashing is a way to secure sensitive data - in this case, PII. In addition, the function also transforms the app_version field to an integer format by removing periods, as the Postgres database expects an integer for this field.
3. Main ETL Loop: The script then enters a loop where it continuously receives messages from the SQS queue.
   For each message:
   * The message is extracted from the queue.
   * The message (a JSON object) is processed: it is parsed and then the necessary fields are masked using the mask_pii function.
   * The processed message is then loaded into the Postgres database, specifically into the user_logins table.
   * Once a message has been successfully processed and loaded into the database, it is deleted from the SQS queue to avoid duplicate         processing. This loop continues until the SQS queue is empty.
4. Clean up: Finally, the script closes the connection to the Postgres database. The script thus provides an ETL process that extracts data from an AWS SQS Queue, transforms the data by masking specific fields, and then loads the data into a Postgres database.

### Execution and Validation
Run the Python code using Visual Studio Code: **code.ipynb** The script processed all the messages in the SQS queue and displayed the message "Queue is now empty" on completion, indicating successful operation.

Validate the Results: Checked the data insertion into the Postgres database by running the command: SELECT * FROM user_logins; in the PostgreSQL command line. This command returned all the records in the user_logins table, confirming successful data insertion.

### Conclusion
The Python script successfully implemented an ETL process by extracting data from an AWS SQS Queue, transforming the data by masking PII and adjusting data types, and then loading the data into a PostgreSQL database. The ETL process was successful, and all the data from the queue was processed and inserted into the database as per the requirement.
