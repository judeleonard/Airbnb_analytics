## Airb

## Project Overview
........

### Tech stack and functions
- `DBT`: For data cleaning and transformation
- `Amazon s3`: Cloud storage
- `Snowflakes`: Cloud data warehouse
- `Preset`: BI tool for developing dashboard

### Data Modeling Workflow
![]()

### Workflow Architecture
![]()

### How to run the project

- Create a project folder and cd into this folder

- Run `make setup` on your terminal to setup dbt in your current directory

- Run `make init` to inialize dbt structure for your project   

- Setup the data warehouse, grant the neccessary permissions and connect it to dbt in the profiles.yml in your directory.
 This can be retrieved using the following command 
        `cat ~/.dbt/profiles.yml`

    setting up data warehouse

    ```sql
    -- Use an admin role
    USE ROLE ACCOUNTADMIN;

    -- Create the `transform` role
    CREATE ROLE IF NOT EXISTS transform;
    GRANT ROLE TRANSFORM TO ROLE ACCOUNTADMIN;

    -- Create the default warehouse if necessary
    CREATE WAREHOUSE IF NOT EXISTS COMPUTE_WH;
    GRANT OPERATE ON WAREHOUSE COMPUTE_WH TO ROLE TRANSFORM;

    -- Create the `dbt` user and assign to role
    CREATE USER IF NOT EXISTS dbt
    PASSWORD=''
    LOGIN_NAME='dbt'
    MUST_CHANGE_PASSWORD=FALSE
    DEFAULT_WAREHOUSE='COMPUTE_WH'
    DEFAULT_ROLE='transform'
    DEFAULT_NAMESPACE='AIRBNB.RAW'
    COMMENT='DBT user used for data transformation';
    GRANT ROLE transform to USER dbt;

    -- Create our database and schemas
    CREATE DATABASE IF NOT EXISTS AIRBNB;
    CREATE SCHEMA IF NOT EXISTS AIRBNB.RAW;

    -- Set up permissions to role `transform`
    GRANT ALL ON WAREHOUSE COMPUTE_WH TO ROLE transform; 
    GRANT ALL ON DATABASE AIRBNB to ROLE transform;
    GRANT ALL ON ALL SCHEMAS IN DATABASE AIRBNB to ROLE transform;
    GRANT ALL ON FUTURE SCHEMAS IN DATABASE AIRBNB to ROLE transform;
    GRANT ALL ON ALL TABLES IN SCHEMA AIRBNB.RAW to ROLE transform;
    GRANT ALL ON FUTURE TABLES IN SCHEMA AIRBNB.RAW to ROLE transform;

    ```

- Run `make test` to run all the implemented tests including the singular tests, generic tests and validation using dbt-expectation

- Run `make docs` to generate the project documentation and `make serve` to deploy this documentation to a webserver accessed at port __8080__



### Dashboard

[Dashboard](https://vimeo.com/808298157/ba56509b05)



### Resources:
- Learn more about dbt [in the docs](https://docs.getdbt.com/docs/introduction)
- Check out [Discourse](https://discourse.getdbt.com/) for commonly asked questions and answers
- Join the [chat](https://community.getdbt.com/) on Slack for live discussions and support
- Find [dbt events](https://events.getdbt.com) near you
- Check out [the blog](https://blog.getdbt.com/) for the latest news on dbt's development and best practices

