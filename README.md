# Integrate Databricks to Azure SQL with MSAL using secret and certificates

MSAL provides different APIs depending on the client type being used. You may refer to the following [GitHub page](https://github.com/AzureAD/microsoft-authentication-library-for-python/wiki/Microsoft-Authentication-Client-Libraries/) for a detailed list of various APIs offered. We will use MSAL to authenticate with AAD and integrate our Databricks with SQL.

![Databricks to Azure SQL using MSAL](images/1_YundNu4G3oPsuhd7xWAj-A.png)

In the preceding diagram, the Databricks Notebook:

1. Acquires a token by using application secret credentials.
2. Uses the token to make requests of the resource (Azure SQL DB)

## App Registration

To register the application, navigate to Azure Active Directory and then click on App registration on the side panel. It’ll open up the App registration screen. When creating a new application, I always use a naming convention that is representative of what will be created, and in the example below I concatenate the application name, with the short name for the resource in Azure (Service Principal) which results in databricks-spn being created.

![App Registration](images/registerapp.png)

Once created, copy the Application ID value. You will then have to generate a secret, which is achieved by clicking on the generate a secret link in the sidebar. Generate a secret and copy the value as it will not be visible the next time you go back to the page.

## Write values to Key Vault

We write both the client Id and secret to Key Vault for a number of reasons.

1. The secret is sensitive and like a storage key or password, we don’t want this to be hardcoded or exposed anywhere in our application.
2. Normally we would have an instance of Databricks and Key Vault per environment and when we come to referencing the secrets, we want the secrets names to remain the same, so the code in our Databricks notebooks referencing the secrets doesn’t need to be modified when we deploy to different environments.

In the key vault, generate secrets that represent the values from the App Registration.

1. DatabricksSpnId (Client Id)
2. DatabricksSpnSecret (Client Secret)

## Azure SQL

The app still needs permission to log into Azure SQL and access the objects within it. You’ll need to Create that user (service principal) in the database and then grant it permissions on the underlying objects. In the example below I have given the service principal select permission on the dbo schema.

![Azure SQL](images/azuresql.png)
