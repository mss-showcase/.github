# Magnificent Seven showcase

This will be the org of a pet project that I'm going to make to practice the usage and configuration of AWS services using terraform.

The planned application will collect the stock data of the Magnificent Seven periodically and will put the data into an S3 bucket.

After that a data processor lambda will do some calculation and place it to either S3 or some other service (TBD)

And finally, there will be a React(-Native) application project that will produce (I hope) a unified client application for mobile / web.

```
[EventBridge cron]
       ↓
  ┌────────────┐
  │ stock-data │──────────────┐
  └────────────┘              │
                              ▼
                         [S3 bucket]
                              │
                        ┌─────┴─────┐
                        ▼           ▼
        ┌────────────────────┐   [cache-flush]
        │ stock-data-to-dynamo │     Lambda
        └────────────────────┘
                        │
                        ▼
                 [DynamoDB ticks]
                        │
                        ▼
                [main-backend Lambda]
                  - GET /tickers
                  - GET /tickers/:symbol
                        │
                        ▼
                  [API Gateway]
                        │
                        ▼
         ┌───────────────────────────────┐
         │           Clients             │
         │  ┌──────────────────────────┐ │
         │  │  frontend (web - React)  │ │
         │  ├──────────────────────────┤ │
         │  │  mobile app (ReactNative)│ │
         │  └──────────────────────────┘ │
         └───────────────────────────────┘

```
