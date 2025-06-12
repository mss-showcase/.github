## Hi there 👋

Welcome to the MSS Showcase GitHub Organization!

🙋‍♀️ **About Us**  
We are building a modular, AWS-based microservice showcase project focused on stock market data processing and analysis. Our project demonstrates best practices using AWS Lambda, DynamoDB, S3, EventBridge, API Gateway, and modern frontend technologies like React and React Native — all carefully optimized to run within the AWS Free Tier.

🌈 **Contributing**  
We welcome contributions from the community! Whether it’s improving our Lambdas, enhancing Terraform infrastructure code, or building out frontend apps, your input helps us grow. Check our repositories for open issues and feel free to submit pull requests or open discussions.

👩‍💻 **Resources**  
- The [MSS Showcase GitHub org](https://github.com/mss-showcase) contains separate repositories for build, deploy workflows, Lambda functions, and frontend apps.  
- Our infrastructure is defined via Terraform with upsert workflows.  
- The Lambdas are mostly Node.js based, with GitHub Actions automating build and deploy pipelines.  

🍿 **Fun Fact**  
Our team runs on coffee and coding snacks while crafting scalable serverless solutions!  

# ** The planned architecture **

```mermaid
flowchart TB
  subgraph "External"
    alpha[Alpha Vantage REST API]
  end

  subgraph "AWSCloud"
    subgraph "Event scheduling"
      cron[EventBridge cron]
    end
    
    subgraph "Lambdas"
      stock[stock-data Lambda]
      proc[stock-data-to-dynamo Lambda]
      backend[main-backend Lambda]
    end

    subgraph "Storage"
      s3data[S3 data bucket]
      s3build[S3 build data bucket]
    end

    subgraph "DynamoDB"
      dynamo[DynamoDB ticks]
    end

    subgraph "CloudFront or bucket cache"
      distribution[static file cache]
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
  backend -->|API requests| api
  distribution --> |read & cache files| s3data

  gha_build -->|upload artifacts| s3build
  gha_deploy -->|terraform apply| AWSCloud

  api -->|API requests| webfrontend[web app]
  api -->|API requests| mobilefrontend[mobile app]

  webfrontend -->|static files| distribution
```

# A screenshot from the current frontend

![webapp screenshot](mss-webapp.png)

As you can see the terraform scripts of the mss-infra project in this phase can reliably 

 * create the necessary buckets (for build and for data)
 * deploy each lambda to AWS
 * set up the correct roles, permissions and policies in AWS IAM for the lambdas
 * configure the AWS gateway

# TODOs

 * The webapp deployment - it requires a new S3 bucket and static hosting
 * The React Native webapp - I am using to use Expo for that and the EAS build server in github builds - it needs to be finished