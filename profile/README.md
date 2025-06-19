## Hi there 👋

Welcome to the MSS Showcase GitHub Organization!

### About this project
I am building a modular, AWS-based microservice showcase project focused on stock market data processing and analysis. My project demonstrates best practices using AWS Lambda, DynamoDB, S3, EventBridge, API Gateway, and modern frontend technologies like React — all carefully optimized to run within the AWS Free Tier.

The application in AWS is fetching the data from Alpha Vantage with the applicable time window (30 mins) that can keep this project in the free plan - so in case of sideways market you will see a near horizontal line, so it is not an error of the code - you have to zoom in.

To make this even funnier, I did this with (near) zero AWS, Github Action, Terraform, Node/express or React knowledge - I have been using ChatGPT and copilot with vscode for this project exclusively. At least I've learned a few things while troubleshooting :)

## Resources
- The [MSS Showcase GitHub org](https://github.com/mss-showcase) contains separate repositories for (github action + terraform) deploy scripts, configured a CloudFront Distribution (but that is too complex to configure and it is too , deploy much for this showcase - therefore I will switch to s3 static web hosting for now), Lambda functions, and a simple frontend app.
- My infrastructure is defined via Terraform with upsert workflows.
- The Lambdas are at now Node.js based, with GitHub Actions automating build and deploy pipelines.  

## A screenshot from the current frontend

![webapp screenshot](mss-webapp.png)

## The current architecture

![Architecture](mss-showcase-arch.png)

## Where I am now?

As you can see the terraform scripts of the mss-infra-core and mss-infra project in this phase can reliably 

 * create the necessary buckets (for build and for data)
 * deploy each lambda to AWS
 * set up the correct roles, permissions and policies in AWS IAM for the lambdas
 * configure the AWS gateway
 * The webapp deployment - it required a new S3 bucket and static hosting (CloudFront configuration has been dropped for the sake of simplicity)

## TODOs

 * Some prediction - sentiment analysis of articles from RSS feeds, TA analysis, funda analysis, maybe some generative AI prompt based opinion - and a conclusion buy or sell? will see what can be done
 * The mobile app - instead of the Expo app, a new mobile app will be created (technology to be decided) 

## Give it a try!

You can try this webapp in AWS until approximately 2026.06.01, or until AWS starts charging for hosting—whichever comes first.

http://mss-webhosting-bucket.s3-website.eu-north-1.amazonaws.com/
