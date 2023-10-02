# OAuth Testing

[![license](https://img.shields.io/github/license/interlok-testing/testing_oauth.svg)](https://github.com/interlok-testing/testing_oauth/blob/develop/LICENSE)
[![Actions Status](https://github.com/interlok-testing/testing_oauth/actions/workflows/gradle-build.yml/badge.svg)](https://github.com/interlok-testing/testing_oauth/actions/workflows/gradle-build.yml)

Project tests interlok-oauth features

## What it does

This project is very simple and contains only one channel with four workflows.
Each workflows expose `/api/{provider}` REST style API where provider is one of salesforce, generic, gcloud or azure.
Calling one of the endpoint will make an oauth authentication against the provider, retrieve a token and use this token to get some data from the provider.

The generic provider is using Salesforce details so the date returned by `/api/salesforce` and `/api/generic` should be the same.

## Prerequisite

We assume that you have a configured account for Salesforce, GCloud and Azure with all the required OAuth permissions set up.

### Salesforce

You need to create a Connected App in your Salesforce account. More help on Salesforce developers site [here](https://developer.salesforce.com/docs/atlas.en-us.api_rest.meta/api_rest/intro_oauth_and_connected_apps.htm) and [here](https://help.salesforce.com/articleView?id=sf.connected_app_overview.htm&type=5) and [here](https://help.salesforce.com/articleView?id=sf.remoteaccess_oauth_flows.htm&type=5).

### GCloud

You need to create a service account, more help [here](https://cloud.google.com/compute/docs/access/service-accounts).
Then you should add the right permissions (Service Account User role and Owner role).
And finally you should generate a [GCloud key json file](https://cloud.google.com/iam/docs/creating-managing-service-account-keys).

### Azure

You need to register a new app, there is an example [here](https://docs.microsoft.com/en-us/graph/notifications-integration-app-registration) and add the two following API Permissions of type _Application_:
 - _Directory.Read.All_
 - _User.Read.All_
 
Don't forget to grant the permissions.

## Getting started

Add a new _variables-local.properties_ file in _/src/main/interlok/config_ with:

```properties
salesforceClientId=Your Salesforce Client Id
salesforceClientSecret=Your Salesforce Client Secret
salesforceUsername=Your Salesforce Username
salesforcePassword=Your Salesforce Password
salesforceSecurityToken=Your Salesforce Security Token
gcloudKeyFile=file://localhost/./config/gloud-key-local.json
gcloudProjectId=Your GCloud Project Id
azureClientId=Your Azure Client Id
azureClientSecret=Your Azure Client Secret
azureTenantId=Your Azure Tenant Id
```

Add a new _gloud-key-local.json_ file in _/src/main/interlok/config_ which should be generated from the GCloud Service Account Key page (https://cloud.google.com/iam/docs/creating-managing-service-account-keys).

Then start Interlok

* `./gradlew clean build`
* `(cd ./build/distribution && java -jar lib/interlok-boot.jar)`

## Testing

```shell
# Test Salesforce
curl "http://localhost:8080/api/salesforce"
# Test Generic (Salesforce)
curl "http://localhost:8080/api/generic"
# Test GCloud
curl "http://localhost:8080/api/gcloud"
# Test Azure
curl "http://localhost:8080/api/azure"
```
