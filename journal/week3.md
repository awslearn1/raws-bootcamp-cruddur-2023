# Week 3 — Decentralized Authentication

Decentralized authentication is a newer concept where users can gain access to online services using verifiable credentials. Say you wanted to access an online banking service. Instead of submitting ID documents, you could submit a Verifiable Credential from a government body to prove your identity.

This removes the need for the bank to request and store your information before granting you access. And you won’t need an account to login in the future since the service provider can issue a Verifiable Credential for ongoing verification. Logging into the site would be as simple as connecting your digital wallet!

Decentralized authentication is a method of authentication that relies on a distributed network of nodes or peers to verify user identities, rather than a centralized authority. Decentralized authentication systems are based on blockchain technology and provide greater security, privacy, and control to users over their personal information and online identities.


# Homework Review

Followed instructions from https://github.com/omenking/aws-bootcamp-cruddur-2023/blob/week-3/journal/week3.md, watched videos, took notes carefully on the notepad, went over the videos again, compared them to my notes, and finally executed all the tasks step-wise.
Videos.

## Videos

- [Week 3 - AWS Cloud Project Bootcamp Decentralized Authenication -  Setup Cognito User Pool, Implement Custom Signin Page](https://www.youtube.com/watch?v=9obl7rVgzJw&list=PLBfufR7vyJJ7k25byhRXJldB5AiwgNnWv&index=40))
- [Week 3 - Cognito Custom Pages, Implement Custom Signup Page, Implement Custom Confirmation Page, Implement Custom Recovery Page](https://www.youtube.com/watch?v=T4X4yIzejTc&list=PLBfufR7vyJJ7k25byhRXJldB5AiwgNnWv&index=42)
- [Week 3 Congito JWT Server side Verify](https://www.youtube.com/watch?v=d079jccoG-M&list=PLBfufR7vyJJ7k25byhRXJldB5AiwgNnWv&index=42)
- [Week 3 - Exploring JWTs](https://www.youtube.com/watch?v=nJjbI4BbasU&list=PLBfufR7vyJJ7k25byhRXJldB5AiwgNnWv&index=43)

## 1. Week 3 - AWS Cloud Project Bootcamp Decentralized Authenication -  Setup Cognito User Pool, Implement Custom Signin Page

- Navigate to new Amazon Cognito in AWS
- Click on create a new user pool

### step 1 of 6

- Configure sign-in experience
- Authentication Providers ===> Cognito user pool
- Cognito user pool sign-in-options ===> username, email 
- User name requirements ===> leave blank
- next

### step 2 of 6

- Password Policy ===> Password policy mode ===> Cognito defaults
- Multi-factor Authentication ===> No MFA
- User account recovery ===> Enable self account recovery
- Delivey method for user account recovery messages ===> Email only
- next

### step 3 of 6

- Configure sign-up experience ===> self-service sign-up - check Enable self-registration 
- Attribute verification and user account confirmation ===> Cognito-assisted verfification and confirmation ===> check Allow cognito to automatically send messages to verify and confirm 
- Attributes to Verify ===> Send email messages, verify email address
- Veifying attribute changes ===> check keep original attribute value when an update is pending 
- Active attribute values when an update is pending ===> email address
- Required attributes ===> Under Additional required attributes drop-down - select name 
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
- Intial app client ===> App type ===> select --> Public client
- App client name ===> enter name thats relevent to the project ===> cruddur
- Client secret ===> select ===> Dont generate a client secret
- Advanced app client setting leave as default
- Attribute read and write permission leave as default

### step 6 of 6

- Review and create - Create user pool



