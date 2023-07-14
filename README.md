# Integrate Databricks to Azure SQL with MSAL using secret and certificates

MSAL provides different APIs depending on the client type being used. You may refer to the following [GitHub page](https://github.com/AzureAD/microsoft-authentication-library-for-python/wiki/Microsoft-Authentication-Client-Libraries/) for a detailed list of various APIs offered. We will use MSAL to authenticate with AAD and integrate our Databricks with SQL.

![Databricks to Azure SQL using MSAL](images/1_YundNu4G3oPsuhd7xWAj-A.png)

In the preceding diagram, the Databricks Notebook:

1. Acquires a token by using application secret credentials.
2. Uses the token to make requests of the resource (Azure SQL DB)

## App Registration

To register the application, navigate to Azure Active Directory and then click on App registration on the side panel. It’ll open up the App registration screen. When creating a new application, I always use a naming convention that is representative of what will be created, and in the example below I concatenate the application name, with the short name for the resource in Azure (Service Principal).

<app>-<resource short name> which results in databricks-spn being created.

![App Registration](images/registerapp.png)
