# Magnificent Seven showcase

This will be the org of a pet project that I'm going to make to practice the usage and configuration of AWS services using terraform.

The planned application will collect the stock data of the Magnificent Seven periodically and will put the data into an S3 bucket.

After that a data processor lambda will do some calculation and place it to either S3 or some other service (TBD)

And finally, there will be a React(-Native) application project that will produce (I hope) a unified client application for mobile / web.

```mermaid
flowchart TB
  subgraph "Alpha Vantage"
    alpha[Alpha Vantage REST API]
  end

  subgraph "AWS Cloud"
    cron[EventBridge cron]
    stock[stock-data Lambda]
    s3[S3 bucket]
    proc[stock-data-to-dynamo Lambda]
    db[DynamoDB ticks]
    backend[main-backend Lambda]
    api[API Gateway]
    
    cron --> |schedules| stock
    alpha --> |polling| stock
    stock --> |write json file| s3
    s3 --> |read json file| proc
    proc --> |write data to tables| db
    db --> |read data| backend
    backend --> |serve requests| api
  end

  subgraph "Clients"
    webfrontend[web app]
    mobilefrontend[mobile app]
  end

  api --> webfrontend
  api --> mobilefrontend
