# Postman-BigQuery
# BigQuery Streaming API with Postman
## How to create and execute BigQuery streaming requests from Postman
BigQuery is a petabyte scale, serverless data warehouse that comes with a built-in query engine. In traditional data warehousing approach, you extract, perform various transformations and load data into a data warehouse. BigQuery uses columnar storage format, where you can simply load or stream your data and start querying. You can query terabytes of data in a matter of seconds.
Postman is a powerful tool for API exploration and testing. In this blog we will see how to use Postman to send data to BigQuery in real time using BigQuery streaming API. You can also use Postman to call BigQuery streaming endpoint to explore and understand BigQuery's streaming functionality.
Postman calling BigQuery APIPrerequisites
Google Cloud Subscription: If you don't have Google Cloud Platform (GCP) subscription, create a free account before you begin (Free Tier gives you 12-month free trial with $300 credit to learn about Google Cloud services)
BigQuery Table: Follow the quick start steps to create BigQuery table.
Postman Application: Download and setup postman application
Create OAuth Credentials
Google uses OAuth 2.0 protocol for authentication and authorization. You need a valid key to send requests to BigQuery streaming API endpoints. We have to get OAuth 2.0 client credentials from Google cloud API console.
Login to Google cloud console and Navigate to APIs & Services.

2. In the APIs & Services, choose BigQuery API from the list of APIs enabled in your project.
3. Click "create credentials" to create OAuth 2.0 client ID.
4. Choose "Create OAuth client ID" and update the settings.
Application Type: Web Application
Name: Postman Client (Choose any names)
Authorized redirect URIs: https://bigquery.googleapis.com
This will create OAuth ID. Save these credentials (JSON) in your machine. We will use these credentials in Postman to generate a token for every request to the BigQuery streaming API.
Generating Token in Postman
In the authorization section, click on "get new access token".

2. Configure the following parameters:
Token Name: Any
Grant Type: Authorization Code
Callback URL: https://bigquery.googleapis.com
Auth URL: https://accounts.google.com/o/oauth2/auth
Access Token URL: https://oauth2.googleapis.com/token
Client ID: Copy Google cloud OAuth credentials from GCP console or from the json file downloaded from the OAuth console.
Client Secret: Copy from Google cloud OAuth credentials console or from the json file downloaded from the OAuth console.
Scope: https://www.googleapis.com/auth/bigquery
Client Authentication: Send client credentials in body
3. Click on "request token".
4. This will take you to Google cloud console page for login and authentication. Enter your credentials. An access token will be generated and you will be redirected to the Postman:
5. This token will expire in 3600 seconds. You can refresh the token periodically by clicking the "get new access token" in Postman. A new access token will be generated, click on "use token".
6. Navigate to "Headers" section and enter "Content-Type" in Key and "application/json" in Value.
7. Navigate to "Body" section and enter the JSON request body. You will construct the body with the data to be sent to BigQuery.
Follow this link to construct the JSON request body: https://cloud.google.com/bigquery/docs/reference/rest/v2/tabledata/insertAll#request-body
JSON request body structure will look like this:
{
  "kind": string,
  "skipInvalidRows": boolean,
  "ignoreUnknownValues": boolean,
  "templateSuffix": string,
  "rows": [
    {
      "insertId": string,
      "json": {
        object
      }
    }
  ]
}
Sample streaming request:
{ "kind": "bigquery#tableDataInsertAllResponse", "rows": [ { "json": {"StationID": "3793", "name": "Rio Grande & 28th", "status": "
active", "latitude":30.29333, "longitude": 
-97.74412, "location": "(30.29333, -97.74412)"}}]}
8. In the POST section, enter the BigQuery streaming service URL as follows: https://bigquery.googleapis.com/bigquery/v2/projects/{projectId}/datasets/{datasetId}/tables/{tableId}/insertAll
9. Click "send" request to execute the call.
Postman will use the elements (Authorization, Headers, Body) to send data to BigQuery in real time. You can use collection run in Postman to stream data to BigQuery.
Thanks for reading :) I hope this article helps to quickly setup and explore BigQuery APIs with Postman.
If you are interested in learning about BigQuery, here is a great book: https://www.oreilly.com/library/view/google-bigquery-the/9781492044451/
References
BigQuery: The Definitive Guide
by Valliappa Lakshmanan and Jordan Tigani O'Reilly, Nov 2019
BigQuery streaming API documentation: https://cloud.google.com/bigquery/streaming-data-into-bigquery
