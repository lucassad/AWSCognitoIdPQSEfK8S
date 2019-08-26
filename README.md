# Use AWS Cognito Use Pool as an IdP for Qlik Sense Enterprise for Kubernetes

## Create a Manage Use Pool 
1) Go to <https://console.aws.amazon.com/cognito> and select "Manage User Pools"
2) Click on "Create a user pool"
3) Set a name for you user pool and click on "Step trough settings" then move for the next step
4) Select the Authentication Method - Username / Email address or phone number and, set email and name as required attributes then move for the next step
5) Set the password strenght, allow / do not allow users to sign themselves up and, how long a password set by an admistration will expire then move for the next step
6) Set if you want to enable Multi-Factor Authentication (MFA), attrubutes that should be verified and, if you want to create a role to allow Amazon Congnito send SMS messages then move for the next step
7) Customize email settings (email addresss, custom verification messages and custom ivitation messages) then move for the next step
8) Customize tags then move for the next step
9) Set if you want to remember your user's devices then move for the next step
10) Create a new app client clicking on "Add an app client", set your app client name like: QSEfK8S and, enable "Generate client secret", enable "Enable sign-in API for server-based authentication (ADMIN_NO_SRP_AUTH)" and "Enable username-password (non-SRP) flow for app-based authentication (USER_PASSWORD_AUTH)" options
11) Click on Create app client, then move for the next step 
12) You can skip triggers
13) Review and Create pool

## Create a domain name
In the console under "App integration" click on "Domain name", check a domain availability and save changes.

## Setup App client Settings
In the console under "App integration" click on "App client settings":

1) Select all "Enabled Identity Provicers"
2) Type your Callback URL
>     Example for Minikube Scenario
>       https://elastic.example:32443/login/callback
3) Select "Authorization code grant" and "Implicit grant" over "Allowed OAuth Flows"
4) Select "openid" and "aws.cognito.signin.user.admin" over "Allowed OAuth Scopes" and save changes

## Get App Client Id and Secret
In the console under "General Settings" click on "App Clients", click on "Show Details" to get all settings.

## Get Pool Id
In the console click on "General Settings" and get the Pool Id value.

## Values YAML file for Qlik Sense Enterprise for Kubernetes

You should add the following values inside your YAML file and change the values in curly brackets {} for your own values.

```yaml
edge-auth:
  oidc:
    redirectUri: {redirectUrl}

identity-providers:
  secrets:
    idpConfigs:
      - discoveryUrl: "https://cognito-idp.{region}.amazonaws.com/{userPoolId}/.well-known/openid-configuration"
        clientId: "{clientId}"
        clientSecret : "{clientSecret}"
        realm: "cognito"
        hostname: "{hostname}"
        primary: true
        scope: openid
```
Example:

```yaml
edge-auth:
  oidc:
    redirectUri: https://elastic.example:32443/login/callback

identity-providers:
  secrets:
    idpConfigs:
      - discoveryUrl: "https://cognito-idp.us-east-1.amazonaws.com/us-east-1_ABC1234/.well-known/openid-configuration"
        clientId: "25a3r3syah18fhkigfadban2pa"
        clientSecret : "ipoppat3li456ssdo14pnaulrrj521q3b2p456ds0vihi"
        realm: "cognito"
        hostname: "elastic.example"
        primary: true
        scope: openid
```