# compose-redis-helloworld-nodejs overview

compose-redis-helloworld-nodejs is a sample IBM Cloud application which shows you how to connect to an IBM Compose for Redis for IBM Cloud service using Node.js.

## Running the app on IBM Cloud

1. If you do not already have an IBM Cloud account, [sign up here][IBMCloud_signup_url]

2. [Download and install IBM Cloud CLI](https://console.bluemix.net/docs/cli/reference/bluemix_cli/download_cli.html).

  The IBM Cloud CLI tool tool is what you'll use to communicate with IBM Cloud from your terminal or command line.

3. Connect to IBM Cloud in the command line tool and follow the prompts to log in.

  ```
  bx login
  ```

  **Note:** If you have a federated user ID, use the `bx login --sso` command to log in with your single sign on ID.

4. Make sure you are targetting the correct IBM Cloud org and space.

  ```
  bx target --cf
  ```

  Choose from the options provided. If you have already created the service, use the same options here as you used when creating the service.

5. Clone the app to your local environment from your terminal using the following command:

  ```
  git clone https://github.com/IBM-IBM Cloud/compose-redis-helloworld-nodejs.git
  ```

6. `cd` into this newly created directory

7. Update the `manifest.yml` file.

  - Change the `host` value to something unique. The host you choose will determinate the subdomain of your application's URL:  `<host>.mybluemix.net`.
  - Change the `name` value. The value you choose will be the name of the app as it appears in your IBM Cloud dashboard.

  If you have already created a Compose for Redis service in IBM Cloud, update the `service` value in `manifest.yml` to match the name of your service. If you don't already have a Compose for Redis service in IBM Cloud, you can create one now using the `create-service` command.

  - **Note :** The Compose for Redis service does not offer a free plan. For details of pricing, see the _Pricing Plans_ section of the [Compose for Redis service][compose_for_redis_url] in IBM Cloud.

  - You will need to specify the service plan that your service will use, which can be _Standard_ or _Enterprise_. This readme file assumes that you will use the _Standard_ plan. To use the _Enterprise_ plan you will need to create an instance of the Compose Enterprise service first. Compose Enterprise is a service which provides a private isolated cluster for your Compose databases. For information on Compose Enterprise and how to provision your app into a Compose Enterprise cluster, see the [Compose Enterprise for IBM Cloud help](https://console.ng.bluemix.net/docs/services/ComposeEnterprise/index.html).

  To create your service:

  ```
  bx cf create-service compose-for-redis Standard my-compose-for-redis-service
  ```

8. Push the app to IBM Cloud. When you push the app it will automatically be bound to the service.

  ```
  bx cf push
  ```

Your application is now running at `<host>.mybluemix.net`.

The node-redis-helloworld app displays the contents of an _examples_ database. To demonstrate that the app is connected to your service, add some words to the database. The words are displayed as you add them, with the most recently added words displayed first.

## Code Structure

| File | Description |
| ---- | ----------- |
|[**server.js**](server.js)|Establishes a connection to the database using credentials from VCAP_ENV and handles create and read operations on the database. |
|[**main.js**](public/javascripts/main.js)|Handles user input for a PUT command and parses the results of a GET command to output the contents of the database.|

The app uses a PUT and a GET operation:

- PUT
  - takes user input from [main.js](public/javascript/main.js)
  - uses the `client.hset()` method to add the user input to the _words_ hash

- GET
  - uses `client.hgetall()` method to retrieve the contents of the _words_ hash
  - returns the response of the database command to [main.js](public/javascript/main.js)

[compose_for_redis_url]: https://console.ng.bluemix.net/catalog/services/compose-for-redis/
[IBMCloud_signup_url]: https://ibm.biz/compose-for-redis-signup
[cloud_foundry_url]: https://github.com/cloudfoundry/cli
