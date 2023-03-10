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

- [Week 3 - AWS Cloud Project Bootcamp Decentralized Authenication -  Setup Cognito User Pool, Implement Signin Page](https://www.youtube.com/watch?v=9obl7rVgzJw&list=PLBfufR7vyJJ7k25byhRXJldB5AiwgNnWv&index=40)
- [Week 3 - Cognito Custom Pages, Implement Signin Page, Signup Page, Confirmation Page, Recovery Page](https://www.youtube.com/watch?v=T4X4yIzejTc&list=PLBfufR7vyJJ7k25byhRXJldB5AiwgNnWv&index=41)
- [Week 3 - Cognito JWT Server side Verify](https://www.youtube.com/watch?v=d079jccoG-M&list=PLBfufR7vyJJ7k25byhRXJldB5AiwgNnWv&index=42)
- [Week 3 - Exploring JWTs](https://www.youtube.com/watch?v=nJjbI4BbasU&list=PLBfufR7vyJJ7k25byhRXJldB5AiwgNnWv&index=43)
- [Week 3 - Improving UI Contrast and Implementing CSS Variables for Theming](https://www.youtube.com/watch?v=m9V4SmJWoJU&list=PLBfufR7vyJJ7k25byhRXJldB5AiwgNnWv&index=44)



## 1. Week 3 - AWS Cloud Project Bootcamp Decentralized Authenication -  Setup Cognito User Pool, Implement Signin Page

- [Amazon Cognito user pools[(https://docs.aws.amazon.com/cognito/latest/developerguide/cognito-user-identity-pools.html)

### 1. Setup Cognito User Pool

- Navigate to new Amazon Cognito in AWS
- Click on create a new user pool

### step 1 of 6

- Configure sign-in experience
- Authentication Providers ===> Cognito user pool
- Cognito user pool sign-in-options ===> **email**
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
    region: process.env.REACT_AWS_PROJECT_REGION,              // REQUIRED - Amazon Cognito Region
    userPoolId: process.env.REACT_APP_AWS_USER_POOLS_ID,       // OPTIONAL - Amazon Cognito User Pool ID
    userPoolWebClientId: process.env.REACT_APP_CLIENT_ID,      // OPTIONAL - Amazon Cognito Web Client ID (26-char alphanumeric string)
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

- remove Cookies from "js-cookie and old const signOut code and replace with code below

```
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

## 2. Week 3 - Cognito Custom Pages, Implement Signin Page, Signup Page, Confirmation Page, Recovery Page

### 1. Set API Calls to Amazon Cognito for Signin page

- [Sign up, Sign in & Sign out](https://docs.amplify.aws/lib/auth/emailpassword/q/platform/js/)
- **frontend-react-js/src/pages/SigninPage.js**
- remove cookies from "js-cookie and old const onsubmit Signin code and replace with code below

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

- [commit link - implement cognito](https://github.com/awsmine/aws-bootcamp-cruddur-2023/commit/2966ec9b6bbae4f265881b347e40c1d254edac77)

- **docker compose up**

```
docker compose up
```

- Open up front end URL - https://3000-awsmine-awsbootcampcrud-aqywnrr242a.ws-us89b.gitpod.io

- Sign into your Cruddur account with email / password
- Got an error when authenticating - blocled by content for cognito user
- [week-3-1-signin-page-error]()

- Run this command to solve the issue

```
aws cognito-idp admin-set-user-password --username bobby --password Test1234! --user-pool-id us-east-1_jU5dTPcnI --permanent
```

```
docker compose up
```
- Open up front end URL - https://3000-awsmine-awsbootcampcrud-aqywnrr242a.ws-us89b.gitpod.io
- Sign into your Cruddur account with email / password
- [week-3-2-signin-page-My Name-handle]()

**if you want to see the user Bobby with the handle bobby**

- Edituser ===> Required attributes ===> enter name - Bobby ===> preferred_username - bobby ===> Save changes

```
docker compose up
```
- Open up front end URL - https://3000-awsmine-awsbootcampcrud-aqywnrr242a.ws-us89b.gitpod.io
- Sign into your Cruddur account with email / password
- [week-3-3-signin-page-name-Bobby-handle-bobby]()

- [commit link - implement signin](https://github.com/awsmine/aws-bootcamp-cruddur-2023/commit/f74715ba1c2304ffa18c9c3bb77b62997be92c28)

### 2. Set API Calls to Amazon Cognito for Signup and Confirmation page

- delete the user **bobby*
- we are to going to create a user from the Signup page

**Signup Page**

- **frontend-react-js/src/pages/SignupPage.js**
- Manually we should not create users. We will now create a signup page so that users can automatically signup 
- remove cookies from "js-cookie and old const onsubmit Signup code and replace with code below

```
// replace cookies with auth from aws-amplify
import { Auth } from 'aws-amplify';

// Already there
//const [cognitoErrors, setCognitoErrors] = React.useState('');

const onsubmit = async (event) => {
    event.preventDefault();
    setErrors('')
    console.log('username',username)
    console.log('email',email)
    console.log('name',name)
    try {
      const { user } = await Auth.signUp({
        username: email,
        password: password,
        attributes: {
          name: name,
          email: email,
          preferred_username: username,
        },
        autoSignIn: { // optional - enables auto sign in after user is confirmed
          enabled: true,
        }
      });
      console.log(user);
      window.location.href = `/confirm?email=${email}`
    } catch (error) {
        console.log(error);
        setErrors(error.message)
    }
    return false
  }
```

**Confirmation page**

- **frontend-react-js/src/pages/ConfirmationPage.js**
- We will now create a Confirmation page so that users will get the confirmation code for Signup 
- remove cookies from "js-cookie and old const resend_code and old const onsubmit code and replace with code below

```
// Removed old cookies method for Auth.
import { Auth } from 'aws-amplify';

const resend_code = async (event) => {
    setErrors('')
    try {
      await Auth.resendSignUp(email);
      console.log('code resent successfully');
      setCodeSent(true)
    } catch (err) {
      // does not return a code
      // does cognito always return english
      // for this to be an okay match?
      console.log(err)
      if (err.message == 'Username cannot be empty'){
        setErrors("You need to provide an email in order to send Resend Activiation Code")   
      } else if (err.message == "Username/client id combination not found."){
        setErrors("Email is invalid or cannot be found.")   
      }
    }
  }

const onsubmit = async (event) => {
    event.preventDefault();
    setErrors('')
    try {
      await Auth.confirmSignUp(email, code);
      window.location.href = "/"
    } catch (error) {
      setErrors(error.message)
    }
    return false
  }
```
**docker compose up**

```
docker compose up
```

- Open up front end URL - https://3000-awsmine-awsbootcampcrud-aqywnrr242a.ws-us89b.gitpod.io
- Join Now for Signup with a user 
- Name - Revathi Joshi
- Email - your email
- username - joshi
- Password - Test1234!
- [week-3-0-signup-user-from aws-console]()
- [week-3-1-signup-confirm-your-email]()
- Get the confirmation code from your eamil
- [week-3-2-confirm-your-email-confirmation-code]()
- [week-3-3-signup-user-Signin-to-Cruddur-account]()
- [week-3-4-signup-user-Signin-to-Cruddur-account-SUCCESSFUL]()
- [commit link - integrate signup and confirmation pages](https://github.com/awsmine/aws-bootcamp-cruddur-2023/commit/6ddecbe2aa39a205640e83b1db46334321b49aa5)


### 3. Set API Calls to Amazon Cognito for Recovery page - for Forgot Password Page

- **frontend-react-js/src/pages/RecoverPage.js**
- remove cookies from "js-cookie and old const onsubmit_send_code and old const onsubmit_confirm_code and replace with code below

```
import { Auth } from 'aws-amplify';

const onsubmit_send_code = async (event) => {
  event.preventDefault();
  setCognitoErrors('')
  Auth.forgotPassword(username)
  .then((data) => setFormState('confirm_code') )
  .catch((err) => setCognitoErrors(err.message) );
  return false
}

 const onsubmit_confirm_code = async (event) => {
    event.preventDefault();
    setErrors('')
    if (password == passwordAgain){
      Auth.forgotPasswordSubmit(username, code, password)
      .then((data) => setFormState('success'))
      .catch((err) => setErrors(err.message) );
    } else {
      setCognitoErrors('Passwords do not match')
    }
    return false
  }
```

**docker compose up**

```
docker compose up
```

- Open up front end URL - https://3000-awsmine-awsbootcampcrud-d9b4ronvag8.ws-us89b.gitpod.io/
- Signin ===> Click Forgot password
- Recover password

[PHOTO]()

- Check Reset  Code from email

[PHOTO 2]()

- Recover your password

- [PHOTO 3 SUCCESSFUL]()

- Now signin

- [PHOTO 4]()

- [commit link - implement recover password](https://github.com/awsmine/aws-bootcamp-cruddur-2023/commit/6bed4c8737aaa011df1c4c1abd2954bfa713f138)

## 3. Week 3 - Cognito JWT Server side Verify - Backend implementation of Cognito Authenticating Server Side

**1. frontend-react-js/src/pages/HomeFeedPage.js**

- Add in the `HomeFeedPage.js` a header to pass along the access token (frontend-react-js)

```
headers: {
    Authorization: `Bearer ${localStorage.getItem("access_token")}`
  },
```

**2. Update backend-flask app.py to allow headers from the frontend for Authorization**

**cd into backend-flask/app.py**

```
import os
import sys # not used in cognito, tried to log with it but didn't work in the video

cors = CORS(
  app, 
  resources={r"/api/*": {"origins": origins}},
  headers=['Content-Type', 'Authorization'], 
  expose_headers='Authorization',
  methods="OPTIONS,GET,HEAD,POST"
)
```

**3. Adding Token Verification ===> verify JWTs obtained from an Amazon Cognito user pool and extract information from it**

**backend-flask/lib/cognito_jwt_token.py**

- backend-flask/lib - Create a folder name lib on the backend-flask directory 
- backend-flask/lib/cognito_jwt_token.py - Create a file inside cognito_jwt_token.py 
- add the following code

```
import time
import requests
from jose import jwk, jwt
from jose.exceptions import JOSEError
from jose.utils import base64url_decode

class FlaskAWSCognitoError(Exception):
  pass

class TokenVerifyError(Exception):
  pass

def extract_access_token(request_headers):
    access_token = None
    auth_header = request_headers.get("Authorization")
    if auth_header and " " in auth_header:
        _, access_token = auth_header.split()
    return access_token

class CognitoJwtToken:
    def __init__(self, user_pool_id, user_pool_client_id, region, request_client=None):
        self.region = region
        if not self.region:
            raise FlaskAWSCognitoError("No AWS region provided")
        self.user_pool_id = user_pool_id
        self.user_pool_client_id = user_pool_client_id
        self.claims = None
        if not request_client:
            self.request_client = requests.get
        else:
            self.request_client = request_client
        self._load_jwk_keys()


    def _load_jwk_keys(self):
        keys_url = f"https://cognito-idp.{self.region}.amazonaws.com/{self.user_pool_id}/.well-known/jwks.json"
        try:
            response = self.request_client(keys_url)
            self.jwk_keys = response.json()["keys"]
        except requests.exceptions.RequestException as e:
            raise FlaskAWSCognitoError(str(e)) from e

    @staticmethod
    def _extract_headers(token):
        try:
            headers = jwt.get_unverified_headers(token)
            return headers
        except JOSEError as e:
            raise TokenVerifyError(str(e)) from e

    def _find_pkey(self, headers):
        kid = headers["kid"]
        # search for the kid in the downloaded public keys
        key_index = -1
        for i in range(len(self.jwk_keys)):
            if kid == self.jwk_keys[i]["kid"]:
                key_index = i
                break
        if key_index == -1:
            raise TokenVerifyError("Public key not found in jwks.json")
        return self.jwk_keys[key_index]

    @staticmethod
    def _verify_signature(token, pkey_data):
        try:
            # construct the public key
            public_key = jwk.construct(pkey_data)
        except JOSEError as e:
            raise TokenVerifyError(str(e)) from e
        # get the last two sections of the token,
        # message and signature (encoded in base64)
        message, encoded_signature = str(token).rsplit(".", 1)
        # decode the signature
        decoded_signature = base64url_decode(encoded_signature.encode("utf-8"))
        # verify the signature
        if not public_key.verify(message.encode("utf8"), decoded_signature):
            raise TokenVerifyError("Signature verification failed")

    @staticmethod
    def _extract_claims(token):
        try:
            claims = jwt.get_unverified_claims(token)
            return claims
        except JOSEError as e:
            raise TokenVerifyError(str(e)) from e

    @staticmethod
    def _check_expiration(claims, current_time):
        if not current_time:
            current_time = time.time()
        if current_time > claims["exp"]:
            raise TokenVerifyError("Token is expired")  # probably another exception

    def _check_audience(self, claims):
        # and the Audience  (use claims['client_id'] if verifying an access token)
        audience = claims["aud"] if "aud" in claims else claims["client_id"]
        if audience != self.user_pool_client_id:
            raise TokenVerifyError("Token was not issued for this audience")

    def verify(self, token, current_time=None):
        """ https://github.com/awslabs/aws-support-tools/blob/master/Cognito/decode-verify-jwt/decode-verify-jwt.py """
        if not token:
            raise TokenVerifyError("No token provided")

        headers = self._extract_headers(token)
        pkey_data = self._find_pkey(headers)
        self._verify_signature(token, pkey_data)

        claims = self._extract_claims(token)
        self._check_expiration(claims, current_time)
        self._check_audience(claims)

        self.claims = claims 
        return claims
```

**4. Update /backend-flask/requirements.txt**

```
Flask-AWSCognito
```

**5. Run command in cli to update the dependencies**

```
pip install -r requirements.txt
```

**6. update the env vars for the backend to implement verification for incognito**

**docker-compose.yml**

```
AWS_COGNITO_USER_POOL_ID: "us-east-1_FpcgxtNfc"
AWS_COGNITO_USER_POOL_CLIENT_ID: "6jbj9dn4cf8oldfrvmm5cg34t9"

```

**7. import cognito_jwt_token into app.py to be used for verification**

**backend-flask/app.py**

```
from lib.cognito_jwt_token import CognitoJwtToken, extract_access_token, TokenVerifyError

```

**8. allow the env vars to be passed into aws**

**backend-flask/app.py**

```
# under the flask
# Incognito AWS ----------
cognito_jwt_token = CognitoJwtToken(
  user_pool_id=os.getenv("AWS_COGNITO_USER_POOL_ID"), 
  user_pool_client_id=os.getenv("AWS_COGNITO_USER_POOL_CLIENT_ID"),
  region=os.getenv("AWS_DEFAULT_REGION")
)
```

**9. update with the following code**

**backend-flask/app.py**

```
@app.route("/api/activities/home", methods=['GET'])
@xray_recorder.capture('activities_home')
def data_home():
  access_token = extract_access_token(request.headers)
  try:
    claims = cognito_jwt_token.verify(access_token)
    # authenicatied request
    app.logger.debug("authenicated")
    app.logger.debug(claims)
    app.logger.debug(claims['username'])
    data = HomeActivities.run(cognito_user_id=claims['username'])
  except TokenVerifyError as e:
    # unauthenicatied request
    app.logger.debug(e)
    app.logger.debug("unauthenicated")
    data = HomeActivities.run()
  return data, 200
```

**10. Add this code for cognito user

**backend-flask/services/home_activities.py**
```
def run(cognito_user_id=None):

if cognito_user_id != None:
  extra_crud = {
    'uuid': '248959df-3079-4947-b847-9e0892d1bab4',
    'handle':  'Lore',
    'message': 'My dear brother, it the humans that are the problem',
    'created_at': (now - timedelta(hours=1)).isoformat(),
    'expires_at': (now + timedelta(hours=12)).isoformat(),
    'likes': 1042,
    'replies': []
  }
  results.insert(0,extra_crud)
```

- [PHOTO]()
- 
**11. Remove the user from local storage on signout from ProfileInfo.js

**frontend-react-js/src/components/ProfileInfo.js **

```
localStorage.removeItem("access_token")
```



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
