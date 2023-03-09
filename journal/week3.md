# Week 3 — Decentralized Authentication

**Decentralized authentication** is a method of authentication that relies on a distributed network of nodes or peers to verify user identities, rather than a centralized authority. Decentralized authentication systems are based on blockchain technology and provide greater security, privacy, and control to users over their personal information and online identities.

**Amazon Cognito** provides authentication, authorization, and user management for your web and mobile apps. Your users can sign in directly with a user name and password, or through a third party social identity providers such as Facebook, Amazon, Google or Apple, as well as multi-factor authentication (MFA) and user sign-up and sign-in workflows.

While Cognito is a centralized authentication service provided by a trusted third-party (AWS), it does offer some features that align with the principles of decentralized authentication. For example, it allows users to maintain control over their own identities and personal information by enabling them to sign in with their own social identity providers or use their own user directories.

In addition, Cognito provides a feature called **user pools** that allows developers to create and manage their own user directories and authentication workflows. This can be seen as a step towards decentralization, as developers have more control over the authentication process and are not entirely reliant on a centralized authentication service.

- [What is Amazon Cognito](https://docs.aws.amazon.com/cognito/latest/developerguide/what-is-amazon-cognito.html)
- [Integrating Amazon Cognito with web and mobile apps](https://docs.aws.amazon.com/cognito/latest/developerguide/cognito-integrate-apps.html)


# Homework Review

Followed instructions from https://github.com/omenking/aws-bootcamp-cruddur-2023/blob/week-3/journal/week3.md, watched videos, took notes carefully on the notepad, went over the videos again, compared them to my notes, and finally executed all the tasks step-wise.
Videos.

## Videos

- [Week 3 - AWS Cloud Project Bootcamp Decentralized Authenication -  Setup Cognito User Pool, Implement Custom Signin Page](https://www.youtube.com/watch?v=9obl7rVgzJw&list=PLBfufR7vyJJ7k25byhRXJldB5AiwgNnWv&index=40))
- [Week 3 - Cognito Custom Pages, Implement Custom Signup Page, Implement Custom Confirmation Page, Implement Custom Recovery Page](https://www.youtube.com/watch?v=T4X4yIzejTc&list=PLBfufR7vyJJ7k25byhRXJldB5AiwgNnWv&index=42)
- [Week 3 Congito JWT Server side Verify](https://www.youtube.com/watch?v=d079jccoG-M&list=PLBfufR7vyJJ7k25byhRXJldB5AiwgNnWv&index=42)
- [Week 3 - Exploring JWTs](https://www.youtube.com/watch?v=nJjbI4BbasU&list=PLBfufR7vyJJ7k25byhRXJldB5AiwgNnWv&index=43)

## 1. Week 3 - AWS Cloud Project Bootcamp Decentralized Authenication -  Setup Cognito User Pool, Implement Custom Signin Page

- [Amazon Cognito user pools[(https://docs.aws.amazon.com/cognito/latest/developerguide/cognito-user-identity-pools.html)

### 1. Setup Cognito User Pool

- Navigate to new Amazon Cognito in AWS
- Click on create a new user pool

### step 1 of 6

- Configure sign-in experience
- Authentication Providers ===> Cognito user pool
- Cognito user pool sign-in-options ===> name, email 
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

### 2. Install AWS Amplify

**AWS Amplify** is a development platform that helps developers build web and mobile applications using AWS services. Amplify provides a set of libraries, UI components, and tools that make it easier to integrate with AWS services, including Cognito for authentication. Thus developers can create applications that give users more control over their personal data and online identities, while still leveraging the power and scalability of AWS services.

- [Authentication with Amplify](https://docs.amplify.aws/lib/auth/getting-started/q/platform/js/)

- cd into frontend-react-js
- This command will add aws-amplify to frontend-react-js/package.json file 

```
npm i aws-amplify --save
```

### 3. Configure Amplify

- add these env variables to frontend-js service in docker-compose.yml

```
      REACT_APP_AWS_PROJECT_REGION: "${AWS_DEFAULT_REGION}"
      REACT_APP_AWS_COGNITO_REGION: "${AWS_DEFAULT_REGION}"
      REACT_APP_AWS_USER_POOLS_ID: "<get from AWS Console>"
      REACT_APP_CLIENT_ID: "<get from AWS Console, App Intergration tab>"
      
REACT_APP_AWS_PROJECT_REGION: "${AWS_DEFAULT_REGION}"
REACT_APP_AWS_COGNITO_REGION: "${AWS_DEFAULT_REGION}"
REACT_APP_AWS_USER_POOLS_ID: "us-east-1_<Enter user pool ID from AWS Console>"
REACT_APP_CLIENT_ID: "<enter AWS App integration tab - App client and analytics CLIENT ID>"
```
- hook up our cognito pool to our code in the frontend-react-js/src/App.js

```
import { Amplify } from 'aws-amplify';

Amplify.configure({
  "AWS_PROJECT_REGION": process.env.REACT_APP_AWS_PROJECT_REGION,
  "aws_cognito_region": process.env.REACT_APP_AWS_COGNITO_REGION,
  "aws_user_pools_id": process.env.REACT_APP_AWS_USER_POOLS_ID,
  "aws_user_pools_web_client_id": process.env.REACT_APP_CLIENT_ID,
  "oauth": {},
  Auth: {
    // We are not using an Identity Pool
    // identityPoolId: process.env.REACT_APP_IDENTITY_POOL_ID, // REQUIRED - Amazon Cognito Identity Pool ID
    region: process.env.REACT_AWS_PROJECT_REGION,           // REQUIRED - Amazon Cognito Region
    userPoolId: process.env.REACT_APP_AWS_USER_POOLS_ID,         // OPTIONAL - Amazon Cognito User Pool ID
    userPoolWebClientId: process.env.REACT_APP_CLIENT_ID,   // OPTIONAL - Amazon Cognito Web Client ID (26-char alphanumeric string)
  }
});


Amplify.configure({
  "AWS_PROJECT_REGION": process.env.REACT_APP_AWS_PROJECT_REGION,
  "aws_cognito_region": process.env.REACT_APP_AWS_COGNITO_REGION,
  "aws_user_pools_id": process.env.REACT_APP_AWS_USER_POOLS_ID,
  "aws_user_pools_web_client_id": process.env.REACT_APP_CLIENT_ID,
  "oauth": {}, // (optional) - Hosted UI configuration
  Auth: {
    // We are not using an Identity Pool
    // identityPoolId: process.env.REACT_APP_IDENTITY_POOL_ID, // REQUIRED - Amazon Cognito Identity Pool ID
    region: process.env.REACT_APP_AWS_PROJECT_REGION,           // REQUIRED - Amazon Cognito Region
    userPoolId: process.env.REACT_APP_AWS_USER_POOLS_ID,         // OPTIONAL - Amazon Cognito User Pool ID
    userPoolWebClientId: process.env.REACT_APP_CLIENT_ID,   // OPTIONAL - Amazon Cognito Web Client ID (26-char alphanumeric string)
  }
});
```

### 4. Conditionally show components based on logged in or logged out

### frontend-react-js/src/pages/HomeFeedPage.js

```
import { Auth } from 'aws-amplify';

// the code is Already there
// set a state 
const [user, setUser] = React.useState(null);

// Comment out now
// import Cookies from 'js-cookie'

// remove the one from old - const checkAuth = async () => {
// copy and paste from below
// check if we are authenicated
const checkAuth = async () => {
  Auth.currentAuthenticatedUser({
    // Optional, By default is false. 
    // If set to true, this call will send a 
    // request to Cognito to get the latest user data
    bypassCache: false 
  })
  .then((user) => {
    console.log('user',user);
    return Auth.currentAuthenticatedUser()
  }).then((cognito_user) => {
      setUser({
        display_name: cognito_user.attributes.name,
        handle: cognito_user.attributes.preferred_username
      })
  })
  .catch((err) => console.log(err));
};

// the code is Already there
// check when the page loads if we are authenicated 
React.useEffect(()=>{
  loadData();
  checkAuth();
}, [])
```

- We'll want to pass user to the following components

```
// the code is Already there
<DesktopNavigation user={user} active={'home'} setPopped={setPopped} />
<DesktopSidebar user={user} />
```

### frontend-react-js/src/components/DesktopNavigation.js

- We'll rewrite **DesktopNavigation.js** so that it it conditionally shows links in the left hand column on whether you are logged in or not.
- Notice we are passing the user to ProfileInfo
- The code was already there, showing it here to notice that we are passing the user to ProfileInfo
- DO NOT COPY and PASTE THIS CODE

```
import './DesktopNavigation.css';
import {ReactComponent as Logo} from './svg/logo.svg';
import DesktopNavigationLink from '../components/DesktopNavigationLink';
import CrudButton from '../components/CrudButton';
import ProfileInfo from '../components/ProfileInfo';

export default function DesktopNavigation(props) {

  let button;
  let profile;
  let notificationsLink;
  let messagesLink;
  let profileLink;
  if (props.user) {
    button = <CrudButton setPopped={props.setPopped} />;
    profile = <ProfileInfo user={props.user} />;
    notificationsLink = <DesktopNavigationLink 
      url="/notifications" 
      name="Notifications" 
      handle="notifications" 
      active={props.active} />;
    messagesLink = <DesktopNavigationLink 
      url="/messages"
      name="Messages"
      handle="messages" 
      active={props.active} />
    profileLink = <DesktopNavigationLink 
      url="/@andrewbrown" 
      name="Profile"
      handle="profile"
      active={props.active} />
  }

  return (
    <nav>
      <Logo className='logo' />
      <DesktopNavigationLink url="/" 
        name="Home"
        handle="home"
        active={props.active} />
      {notificationsLink}
      {messagesLink}
      {profileLink}
      <DesktopNavigationLink url="/#" 
        name="More" 
        handle="more"
        active={props.active} />
      {button}
      {profile}
    </nav>
  );
}
```

### frontend-react-js/src/components/ProfileInfo.js

```
// remove Cookies from "js-cookie and replace with code below
import { Auth } from 'aws-amplify';

const signOut = async () => {
  try {
      await Auth.signOut({ global: true });
      window.location.href = "/"
  } catch (error) {
      console.log('error signing out: ', error);
  }
}
```

### frontend-react-js/src/components/DesktopSidebar.js

- Rewrote DesktopSidebar.js it conditionally shows components in case you are logged in or not.

- Remove this code
```
let trending;
  if (props.user) {
    trending = <TrendingSection trendings={trendings} />
  }

  let suggested;
  if (props.user) {
    suggested = <SuggestedUsersSection users={users} />
  }
  let join;
  if (props.user) {
  } else {
    join = <JoinSection />
  }
```

- Copy and paste this code
```
let trending;
let suggested;
let join;
if (props.user) {
    trending = <TrendingSection trendings={trendings} />
    suggested = <SuggestedUsersSection users={users} />
} else {
    join = <JoinSection />
}
```

### 5. Set API Calls to Amazon Coginto for Signin, Signup, Recovery and Forgot Password Page

- [Sign up, Sign in & Sign out](https://docs.amplify.aws/lib/auth/emailpassword/q/platform/js/)

### frontend-react-js/src/pages/SigninPage.js ===> Signin Page

- // Remove now
- // import Cookies from 'js-cookie'
- And copy and paste it from below

```
// replace cookies with auth from aws-amplify
import { Auth } from 'aws-amplify';

// Already there
// const [cognitoErrors, setCognitoErrors] = React.useState('');

const onsubmit = async (event) => {
  setErrors('')
  event.preventDefault();
    Auth.signIn(email, password)
      .then(user => {
        console.log('user',user)
        localStorage.setItem("access_token", user.signInUserSession.accessToken.jwtToken)
        window.location.href = "/"
      })
      .catch(err => {
          if (error.code == 'UserNotConfirmedException') {
      window.location.href = "/confirm"
      }
    setErrors(error.message)
  });
  return false
}
```

[commit link](https://github.com/awsmine/aws-bootcamp-cruddur-2023/commit/2966ec9b6bbae4f265881b347e40c1d254edac77)



## Homework Challenges

### 1. [Medium] Decouple the JWT verify from the application code by writing a Flask Middleware




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
