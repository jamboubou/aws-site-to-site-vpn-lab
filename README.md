# VPN Site to Site connection via AWS Transit Gateway Lab

In this lab, you will setup a VPN site to site connection between a simulated onprem site in AWS and an AWS VPC.

We will use in this lab [AWS Transit Gateway](https://aws.amazon.com/transit-gateway/) VPN support to link a simulated on prem environment with an AWS VPC.
For the on prem VPN Gateway we will use [strongSwan VPN solution](https://www.strongswan.org/)

See the diagram below for a depiction of the complete architecture.

![Architecture](images/vpn-lab-architecture.png)

## Modules

This lab is split into multiple modules. Each module builds upon the previous module. You must complete each module before proceeding to the next.

1. **Environment Deployment** - In this module, you will create a Cognito User Pool for identity management and user authentication and will integrate it with a pre-existing WildRydes React JS Web Application. You will also configure Cognito Identity Pools, which provides the ability to assume an Identity and Access Management (IAM) role from within an application.

2. **VPN Configuration** - In this module, you will add a serverless backend to our Wild Rydes application leveraging API Gateway and Lambda. You will then enable authentication and authorization on your API to secure the backend to only accept valid, authorized requests.

3. **VPN Gateway Deployment** - In this module, you will expand your Wild Rydes application by enabling profile management and profile photo management capabilities. Amazon Cognito will be used to store your user's profile information and attributes whereas Amazon S3 will store your user's profile pictures, with a link to the photo stored in the user's profile information.

## Getting Started

You may now proceed to [Module 1 - User Authentication](./1_UserAuthentication).
