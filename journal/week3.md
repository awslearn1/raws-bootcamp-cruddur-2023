# Week 3 — Decentralized Authentication

Decentralized authentication is a method of authentication that relies on a distributed network of nodes or peers to verify user identities, rather than a centralized authority. Decentralized authentication systems are based on blockchain technology and provide greater security, privacy, and control to users over their personal information and online identities.

Amazon Cognito provides authentication, authorization, and user management for your web and mobile apps. Your users can sign in directly with a user name and password, or through a third party social identity providers such as Facebook, Amazon, Google or Apple, as well as multi-factor authentication (MFA) and user sign-up and sign-in workflows.

While Cognito is a centralized authentication service provided by a trusted third-party (AWS), it does offer some features that align with the principles of decentralized authentication. For example, it allows users to maintain control over their own identities and personal information by enabling them to sign in with their own social identity providers or use their own user directories.

In addition, Cognito provides a feature called **user pools** that allows developers to create and manage their own user directories and authentication workflows. This can be seen as a step towards decentralization, as developers have more control over the authentication process and are not entirely reliant on a centralized authentication service.

# Homework Review

Followed instructions from https://github.com/omenking/aws-bootcamp-cruddur-2023/blob/week-3/journal/week3.md, watched videos, took notes carefully on the notepad, went over the videos again, compared them to my notes, and finally executed all the tasks step-wise.
Videos.

## Videos

- [Week 3 - AWS Cloud Project Bootcamp Decentralized Authenication -  Setup Cognito User Pool, Implement Custom Signin Page](https://www.youtube.com/watch?v=9obl7rVgzJw&list=PLBfufR7vyJJ7k25byhRXJldB5AiwgNnWv&index=40))
- [Week 3 - Cognito Custom Pages, Implement Custom Signup Page, Implement Custom Confirmation Page, Implement Custom Recovery Page](https://www.youtube.com/watch?v=T4X4yIzejTc&list=PLBfufR7vyJJ7k25byhRXJldB5AiwgNnWv&index=42)
- [Week 3 Congito JWT Server side Verify](https://www.youtube.com/watch?v=d079jccoG-M&list=PLBfufR7vyJJ7k25byhRXJldB5AiwgNnWv&index=42)
- [Week 3 - Exploring JWTs](https://www.youtube.com/watch?v=nJjbI4BbasU&list=PLBfufR7vyJJ7k25byhRXJldB5AiwgNnWv&index=43)

## 1. Week 3 - AWS Cloud Project Bootcamp Decentralized Authenication -  Setup Cognito User Pool, Implement Custom Signin Page

**1. Setup Cognito User Pool**

- Navigate to new Amazon Cognito in AWS
- Click on create a new user pool

### step 1 of 6

- Configure sign-in experience
- Authentication Providers ===> Cognito user pool
- Cognito user pool sign-in-options ===> email 
- User name requirements ===> leave blank
- next

### step 2 of 6

- Password Policy ===> Password policy mode ===> Cognito defaults
- Multi-factor Authentication ===> No MFA
- User account recovery ===> Self-service account delivery ===> Enable self-service account delivery
- Delivey method for user account recovery messages ===> Email only
- next

### step 3 of 6

- Configure sign-up experience ===> self-service sign-up - check Enable self-registration 
- Attribute verification and user account confirmation ===> Cognito-assisted verfification and confirmation ===> check Allow cognito to automatically send messages to verify and confirm 
- Attributes to Verify ===> Send email messages, verify email address
- Veifying attribute changes ===> check keep original attribute value active when an update is pending 
- Active attribute values when an update is pending ===> email address
- Required attributes ===> Under Additional required attributes drop-down - select name, preferred_username
- next

### step 4 of 6

- Configure message delivery ===> Email ===> Email Provider ===> checck Send email with cognito 
- SES region ===> set your aws region
- From email address ===> select ===> no-reply@verificationemail.com
- Reply to email address leave blank
- next

### step 5 of 6

- Integrate your app ===> User pool name ===> enter user pool name  - cruddur-user-pool
- Hosted authentication pages ===> Uncheck Use the Cognito Hosted UI 
- Intial app client ===> App type ===> select Public client
- App client name ===> enter name thats relevent to the project ===> cruddur
- client secret ===> select ===> Dont generate a client secret
- Advanced app client setting leave as default
- Attribute read and write permission leave as default

### step 6 of 6

- Review and create - Create user pool

**2. Install AWS Amplify**

AWS Amplify is a development platform that helps developers build web and mobile applications using AWS services. Amplify provides a set of libraries, UI components, and tools that make it easier to integrate with AWS services, including Cognito for authentication. Thus developers can create applications that give users more control over their personal data and online identities, while still leveraging the power and scalability of AWS services.




## Homework Challenges

### 1. [Medium] Decouple the JWT verify from the application code by writing a  Flask Middleware




### 2. [Hard] Decouple the JWT verify by implementing a Container Sidecar pattern using AWS’s official Aws-jwt-verify.js library




### 3. [Hard] Decouple the JWT verify process by using Envoy as a sidecar https://www.envoyproxy.io/




### 4. [Hard]  Implement a IdP login eg. Login with Amazon or Facebook or Apple.




### 5. [Easy] Implement MFA that send an SMS (text message), warning this has spend, investigate spend before considering, text messages are not eligible for AWS Credits




## Knowledge Challenges

- Security Quiz - Submitted
- Pricing Quiz - Submitted


## Cloud Technical Essays

**1.** 

**2.** 
