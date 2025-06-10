# Magnificent Seven showcase

This will be the org of a pet project that I'm going to make to practice the usage and configuration of AWS services using terraform.

The planned application will collect the stock data of the Magnificent Seven periodically and will put the data into an S3 bucket.

After that a data processor lambda will do some calculation and place it to either S3 or some other service (TBD)

And finally, there will be a React(-Native) application project that will produce (I hope) a unified client application for mobile / web.

```mermaid
flowchart TB
  subgraph "External"
    alpha[Alpha Vantage REST API]
  end

  subgraph "AWS Cloud"
    subgraph "Event scheduling"
      cron[EventBridge cron]
    end
    
    subgraph "Lambdas"
      stock[stock-data Lambda]
      proc[stock-data-to-dynamo Lambda]
      cache[cache-flush Lambda]
      backend[main-backend Lambda]
    end

    subgraph "Storage"
      s3data[S3 data bucket]
      s3build[S3 build data bucket]
    end

    subgraph "DynamoDB"
      dynamo[DynamoDB ticks]
    end

    subgraph "CloudFront"
      distribution[CloudFront CDN cache]
    end

    api[API Gateway]
  end

  subgraph "Github CI/CD"
    gha_build[GitHub Build Actions]
    gha_deploy[GitHub Deploy Actions]
  end

  alpha -->|polling| stock
  cron -->|schedule| stock
  stock -->|write json| s3data
  s3data -->|read json| proc
  proc -->|write to table| dynamo
  dynamo -->|read| backend
  backend -->|serve API| api
  distribution --> |read & cache| s3data

  gha_build -->|upload artifacts| s3build
  gha_deploy -->|terraform apply| s3build
  gha_deploy -->|terraform apply| Lambdas & infra

  api -->|serve| webfrontend[web app]
  api -->|serve| mobilefrontend[mobile app]

  webfrontend -->|static files| distribution
