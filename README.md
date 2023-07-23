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

## Development & Project Run






