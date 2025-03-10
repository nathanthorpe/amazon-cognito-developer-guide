# Adding social identity providers to a user pool<a name="cognito-user-pools-social-idp"></a>

Your web and mobile app users can sign in through social identity providers \(IdP\) like Facebook, Google, Amazon, and Apple\. With the built\-in hosted web UI, Amazon Cognito provides token handling and management for all authenticated users\. This way, your backend systems can standardize on one set of user pool tokens\. You must enable the hosted UI to integrate with supported social identity providers\. When Amazon Cognito builds your hosted UI, it creates OAuth 2\.0 endpoints that Amazon Cognito and your OIDC and social IdPs use to exchange information\. For more information, see the [Amazon Cognito user pools Auth API reference](https://docs.aws.amazon.com/cognito/latest/developerguide/cognito-userpools-server-contract-reference.html)\.

You can add a social IdP in the AWS Management Console, or you can use the AWS CLI or Amazon Cognito API\. 

![\[Authentication overview with social sign-in\]](http://docs.aws.amazon.com/cognito/latest/developerguide/)![\[Authentication overview with social sign-in\]](http://docs.aws.amazon.com/cognito/latest/developerguide/)![\[Authentication overview with social sign-in\]](http://docs.aws.amazon.com/cognito/latest/developerguide/)

**Note**  
Sign\-in through a third party \(federation\) is available in Amazon Cognito user pools\. This feature is independent of federation through Amazon Cognito identity pools \(federated identities\)\.

**Topics**
+ [Prerequisites](#cognito-user-pools-social-idp-prerequisites)
+ [Step 1: Register with a social IdP](#cognito-user-pools-social-idp-step-1)
+ [Step 2: Add a social IdP to your user pool](#cognito-user-pools-social-idp-step-2)
+ [Step 3: Test your social IdP configuration](#cognito-user-pools-social-idp-step-3)

## Prerequisites<a name="cognito-user-pools-social-idp-prerequisites"></a>

Before you begin, you need the following:
+ A user pool with an app client and a user pool domain\. For more information, see [Create a user pool](https://docs.aws.amazon.com/cognito/latest/developerguide/cognito-user-pool-as-user-directory.html)\.
+ A social IdP\.

## Step 1: Register with a social IdP<a name="cognito-user-pools-social-idp-step-1"></a>

Before you create a social IdP with Amazon Cognito, you must register your application with the social IdP to receive a client ID and client secret\.

### To register an app with Facebook<a name="cognito-user-pools-social-idp-step-1-facebook"></a>

1. Create a [developer account with Facebook](https://developers.facebook.com/docs/facebook-login)\.

1. [Sign in](https://developers.facebook.com/) with your Facebook credentials\.

1. From the **My Apps** menu, choose **Create New App**\.

1. Enter a name for your Facebook app, and then choose **Create App ID**\.

1. On the left navigation bar, choose **Settings**, and then **Basic**\.

1. Note the **App ID** and the **App Secret**\. You will use them in the next section\.

1. Choose **\+ Add Platform** from the bottom of the page\.

1. Choose **Website**\.

1. Under **Website**, enter the path to the sign\-in page for your app into **Site URL**\.

   ```
   https://<your_user_pool_domain>/login?response_type=code&client_id=<your_client_id>&redirect_uri=https://www.example.com
   ```

1. Choose **Save changes**\.

1. Enter the path to the root of your user pool domain into **App Domains**\.

   ```
   https://<your-user-pool-domain>
   ```

1. Choose **Save changes**\.

1. From the navigation bar choose **Add Product** and choose **Set up** for the **Facebook Login** product\.

1. From the navigation bar choose **Facebook Login** and then **Settings**\.

   Enter the path to the `/oauth2/idpresponse` endpoint for your user pool domain into **Valid OAuth Redirect URIs**\.

   ```
   https://<your-user-pool-domain>/oauth2/idpresponse
   ```

1. Choose **Save changes**\.

### To register an app with Amazon<a name="cognito-user-pools-social-idp-step-1-amazon"></a>

1. Create a [developer account with Amazon](https://developer.amazon.com/login-with-amazon)\.

1. [Sign in](https://developer.amazon.com/lwa/sp/overview.html) with your Amazon credentials\.

1. You need to create an Amazon security profile to receive the Amazon client ID and client secret\.

   Choose **Apps and Services** from navigation bar at the top of the page and then choose **Login with Amazon**\.

1. Choose **Create a Security Profile**\.

1. Enter a **Security Profile Name**, a **Security Profile Description**, and a **Consent Privacy Notice URL**\.

1. Choose **Save**\.

1. Choose **Client ID** and **Client Secret** to show the client ID and secret\. You will use them in the next section\.

1. Hover over the gear icon and choose **Web Settings**, and then choose **Edit**\.

1. Enter your user pool domain into **Allowed Origins**\.

   ```
   https://<your-user-pool-domain>
   ```

1. Enter your user pool domain with the `/oauth2/idpresponse` endpoint into **Allowed Return URLs**\.

   ```
   https://<your-user-pool-domain>/oauth2/idpresponse
   ```

1. Choose **Save**\.

### To register an app with Google<a name="cognito-user-pools-social-idp-step-1-google"></a>

For more information about OAuth 2\.0 in the Google Cloud platform, see [Learn about authentication & authorization](https://developers.google.com/workspace/guides/auth-overview) in the Google Workspace for Developers documentation\.

1. Create a [developer account with Google](https://developers.google.com/identity)\.

1. Sign in to the [Google Cloud Platform console](https://console.cloud.google.com/home/dashboard)\.

1. From the top navigation bar, choose **Select a project**\. If you already have a project in the Google platform, this menu displays your default project instead\.

1. Select **NEW PROJECT**\.

1. Enter a name for your product and then choose **CREATE**\.

1. On the left navigation bar, choose **APIs and Services**, then **Oauth consent screen**\.

1. Enter App information, an **App domain**, **Authorized domains**, and **Developer contact information**\. Your **Authorized domains** must include `amazoncognito.com` and the root of your custom domain, for example `example.com`\. Choose **SAVE AND CONTINUE**\.

1. 1\. Under **Scopes**, choose **Add or remove scopes**, and choose, at minimum, the following OAuth scopes\.

   1. `.../auth/userinfo.email`

   1. `.../auth/userinfo.profile`

   1. openid

1. Under **Test users**, choose **Add users**\. Enter your email address and any other authorized test users, then choose **SAVE AND CONTINUE**\.

1. Expand the left navigation bar again, and choose **APIs and Services**, then **Credentials**\. 

1. Choose **CREATE CREDENTIALS**, then **OAuth client ID**\.

1. Choose an **Application type** and give your client a **Name**\.

1. Under **Authorized JavaScript origins**, choose **ADD URI**\. Enter your user pool domain\.

   ```
   https://<your-user-pool-domain>
   ```

1. Under **Authorized redirect URIs**, choose **ADD URI**\. Enter the path to the `/oauth2/idpresponse` endpoint of your user pool domain\.

   ```
   https://<your-user-pool-domain>/oauth2/idpresponse
   ```

1. Choose **CREATE**\.

1. Securely store the values the Google displays under **Your client ID** and **Your client secret**\. Provide these values to Amazon Cognito when you add a Google IdP\.

### To register an app with Apple<a name="cognito-user-pools-social-idp-step-1-apple"></a>

For more information about setting up Sign in with Apple, see [Configuring Your Environment for Sign in with Apple](https://developer.apple.com/documentation/sign_in_with_apple/configuring_your_environment_for_sign_in_with_apple) in the Apple Developer documentation\.

1. Create a [developer account with Apple](https://developer.apple.com/programs/enroll/)\.

1. [Sign in](https://developer.apple.com/account/#/welcome) with your Apple credentials\.

1. On the left navigation bar, choose **Certificates, Identifiers & Profiles**\.

1. On the left navigation bar, choose **Identifiers**\.

1. On the **Identifiers** page, choose the **\+** icon\.

1. On the **Register a New Identifier** page, choose **App IDs**, and then choose **Continue**\.

1. On the **Select a type** page, choose **App**, then choose **Continue**\.

1. On the **Register an App ID** page, do the following:

   1. Under **Description**, enter a description\.

   1. Under **App ID Prefix**, enter a **Bundle ID**\. Make a note of the value under **App ID Prefix**\. You will use this value after you choose Apple as your identity provider in [Step 2: Add a social IdP to your user pool](#cognito-user-pools-social-idp-step-2)\.

   1. Under **Capabilities**, choose **Sign In with Apple**, and then choose **Edit**\.

   1. On the **Sign in with Apple: App ID Configuration** page, choose to set up the app as either primary or grouped with other App IDs, and then choose **Save**\.

   1. Choose **Continue**\.

1. On the **Confirm your App ID** page, choose **Register**\.

1. On the **Identifiers** page, choose the **\+** icon\.

1. On the **Register a New Identifier** page, choose **Services IDs**, and then choose **Continue**\.

1. On the **Register a Services ID** page, do the following:

   1. Under **Description**, type a description\.

   1. Under **Identifier**, type an identifier\. Make a note of this Services ID as you will need this value after you choose Apple as your identity provider in [Step 2: Add a social IdP to your user pool](#cognito-user-pools-social-idp-step-2)\.

   1. Choose **Continue**, then **Register**\.

1. Choose the Services ID you just create from the Identifiers page\.

   1. Select **Sign In with Apple**, and then choose **Configure**\.

   1. On the **Web Authentication Configuration** page, select the app ID that you created earlier as the **Primary App ID**\. 

   1. Choose the **\+** icon next to **Website URLs**\. 

   1. Under **Domains and subdomains**, enter your user pool domain without an `https://` prefix\.

      ```
      <your-user-pool-domain>
      ```

   1. Under **Return URLs**, enter the path to the `/oauth2/idpresponse` endpoint of your user pool domain\.

      ```
      https://<your-user-pool-domain>/oauth2/idpresponse
      ```

   1. Choose **Next**, and then **Done**\. You don't need to verify the domain\.

   1. Choose **Continue**, and then choose **Save**\.

1. On the left navigation bar, choose **Keys**\.

1. On the **Keys** page, choose the **\+** icon\.

1. On the **Register a New Key** page, do the following:

   1. Under **Key Name**, enter a key name\. 

   1. Choose **Sign In with Apple**, and then choose **Configure**\.

   1. On the **Configure Key** page and select the app ID that you created earlier as the **Primary App ID**\. Choose **Save**\.

   1. Choose **Continue**, and then choose **Register**\.

1. On the **Download Your Key** page, choose **Download** to download the private key and note the **Key ID** shown, and then choose **Done**\. You will need this private key and the **Key ID** value shown on this page after you choose Apple as your identity provider in [Step 2: Add a social IdP to your user pool](#cognito-user-pools-social-idp-step-2)\.

## Step 2: Add a social IdP to your user pool<a name="cognito-user-pools-social-idp-step-2"></a>

------
#### [ Original console ]

**To configure a user pool social IdP with the AWS Management Console**

1. Go to the [Amazon Cognito console](https://console.aws.amazon.com/cognito/home)\. If prompted, enter your AWS credentials\.

1. Choose **Manage User Pools**\.

1. Choose an existing user pool from the list, or [create a user pool](https://docs.aws.amazon.com/cognito/latest/developerguide/cognito-user-pool-as-user-directory.html)\.

1. On the left navigation bar, choose **Identity providers**\.

1. Choose a social IdP: **Facebook**, **Google**, **Login with Amazon**, or **Apple**\.

1. Choose from the following steps, based on your choice of social IdP:
   + **Google** and **Login with Amazon** — Enter the **app client ID** and **app client secret** generated in the previous section\.
   + **Facebook** — Enter the **app client ID** and **app client secret** generated in the previous section, and then choose an API version \(for example, version 2\.12\)\. We recommend that you choose the latest possible version, as each Facebook API has a lifecycle and discontinuation date\. Facebook scopes and attributes can vary between API versions\. We recommend that you test your social identity log in with Facebook to make sure that federation works as you intend\.
   + **Sign In with Apple** — Enter the **Services ID**, **Team ID**, **Key ID**, and **private key** generated in the previous section\.

1. Enter the names of the scopes that you want to authorize\. Scopes define which user attributes \(such as `name` and `email`\) you want to access with your app\. For Facebook, these should be separated by commas\. For Google and Login with Amazon, they should be separated by spaces\. For Sign in with Apple, select the check boxes for the scopes you want access to\.    
[\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/cognito/latest/developerguide/cognito-user-pools-social-idp.html)

   Your app user is asked to consent to providing these attributes to your app\. For more information about their scopes, see the documentation from Google, Facebook, Login with Amazon, or Sign in with Apple\. 

   With Sign in with Apple, the following are user scenarios where scopes might not be returned:
   + An end user encounters failures after leaving Apple’s sign in page \(can be from Internal failures within Amazon Cognito or anything written by the developer\)
   + The service ID identifier is used across user pools and other authentication services
   + A developer adds additional scopes after the end user has signed in before \(no new information is retrieved\)
   + A developer deletes the user and then the user signs in again without removing the app from their Apple ID profile

1. Choose **Enable** for the social IdP that you are configuring\.

1. Choose **App client settings** from the navigation bar\.

1. Select your social IdP as one of the **Enabled Identity Providers** for your user pool app\.

1. Enter your callback URL into **Callback URL\(s\)** for your user pool app\. This is the URL to which your user will be redirected after a successful authentication\.

   ```
   https://www.example.com
   ```

1. Choose **Save changes**\.

1. On the **Attribute mapping** tab, add mappings for at least the required attributes, typically `email`, as follows:

   1. Select the check box to choose the Facebook, Google, or Amazon attribute name\. You can also type the names of additional attributes that are not listed in the Amazon Cognito console\.

   1. Choose the destination user pool attribute from the drop\-down list\.

   1. Choose **Save changes**\.

   1. Choose **Go to summary**\.

------
#### [ New console ]

**To configure a user pool social IdP with the AWS Management Console**

1. Go to the [Amazon Cognito console](https://console.aws.amazon.com/cognito/home)\. If prompted, enter your AWS credentials\.

1. Choose **User Pools**\.

1. Choose an existing user pool from the list, or [create a user pool](https://docs.aws.amazon.com/cognito/latest/developerguide/cognito-user-pool-as-user-directory.html)\.

1. Choose the **Sign\-in experience** tab\. Locate **Federated sign\-in** and then select **Add an identity provider**\.

1. Choose a social IdP: **Facebook**, **Google**, **Login with Amazon**, or **Sign in with Apple**\.

1. Choose from the following steps, based on your choice of social IdP:
   + **Google** and **Login with Amazon** — Enter the **app client ID** and **app client secret** generated in the previous section\.
   + **Facebook** — Enter the **app client ID** and **app client secret** generated in the previous section, and then choose an API version \(for example, version 2\.12\)\. We recommend that you choose the latest possible version, as each Facebook API has a lifecycle and discontinuation date\. Facebook scopes and attributes can vary between API versions\. We recommend that you test your social identity log in with Facebook to make sure that federation works as you intend\.
   + **Sign In with Apple** — Enter the **Services ID**, **Team ID**, **Key ID**, and **private key** generated in the previous section\.

1. Enter the names of the **Authorized scopes** you want to use\. Scopes define which user attributes \(such as `name` and `email`\) you want to access with your app\. For Facebook, these should be separated by commas\. For Google and Login with Amazon, they should be separated by spaces\. For Sign in with Apple, select the check boxes for the scopes you want access to\.    
[\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/cognito/latest/developerguide/cognito-user-pools-social-idp.html)

   Your app user is prompted to consent to providing these attributes to your app\. For more information about social provider scopes, see the documentation from Google, Facebook, Login with Amazon, or Sign in with Apple\. 

   With Sign in with Apple, the following are user scenarios where scopes might not be returned:
   + An end user encounters failures after leaving Apple’s sign in page \(can be from Internal failures within Amazon Cognito or anything written by the developer\)
   + The service ID identifier is used across user pools and/or other authentication services
   + A developer adds additional scopes after the end user has signed in before \(no new information is retrieved\)
   + A developer deletes the user and then the user signs in again without removing the app from their Apple ID profile

1. Map attributes from your IdP to your user pool\. For more information, see [Specifying Identity Provider Attribute Mappings for Your User Pool](https://docs.aws.amazon.com/cognito/latest/developerguide/cognito-user-pools-specifying-attribute-mapping.html)\.

1. Choose **Create**\.

1. From the **App client integration** tab, choose one of the **App clients** in the list and **Edit hosted UI settings**\. Add the new social IdP to the app client under **Identity providers**\.

1. Choose **Save changes**\.

------

## Step 3: Test your social IdP configuration<a name="cognito-user-pools-social-idp-step-3"></a>

You can create a login URL by using the elements from the previous two sections\. Use it to test your social IdP configuration\.

```
https://<your_user_pool_domain>/login?response_type=code&client_id=<your_client_id>&redirect_uri=https://www.example.com
```

You can find your domain on the user pool **Domain name** console page\. The client\_id is on the **App client settings** page\. Use your callback URL for the **redirect\_uri** parameter\. This is the URL of the page where your user will be redirected after a successful authentication\.

**Note**  
Amazon Cognito cancels authentication requests that do not complete within 5 minutes, and redirects the user to the hosted UI\. The page displays a `Something went wrong` error message\.