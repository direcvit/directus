# Deploy A Directus App

[Directus](https://directus.io/) is and open source app that wraps your new or existing SQL database with a realtime GraphQL + REST API for developers, and an intuitive admin app for non-technical users. It supports PostgreSQL, MySQL, SQLite, Microsoft SQL Server, OracleDB, MariaDB, AWS Aurora and more.

Directus mirrors your custom database, with your schema and content stored pure and unaltered. When it comes time to ingest, fetch, or update your data, you can use our REST+GraphQL API, JavaScript SDK, or even pure SQL. Directus gives you plenty of access options to choose from.

## Prepare the Directus package

Create a new directory, and add a package.json by running the following command:
```
npm init -y
```

Add the `build` and `start` aliases script to the package.json:
```
{
  ...
  "scripts": {
    "start": "directus start",
    "build": "npx directus bootstrap",
    "test": "echo \"Error: no test specified\" && exit 1"
  }
  ...
}
```

Install Directus by running the following command:
```
npm install directus
```

The package.json content will look like this:
```
{
  "name": "directus-app",
  "version": "1.0.0",
  "description": "",
  "main": "index.js",
  "scripts": {
    "start": "directus start",
    "build": "npx directus bootstrap",
    "test": "echo \"Error: no test specified\" && exit 1"
  },
  "keywords": [],
  "author": "",
  "license": "ISC",
  "dependencies": {
    "directus": "^9.0.0-rc.51"
  }
}
```

Push the package.json to your GitHub repository.

## Add new NodeJS SSR app

On a new server, add a new site, select **NodeJS SSR** as the app type.

![NodeJS SSR](https://user-images.githubusercontent.com/6209240/112153603-01111d00-8c16-11eb-95f8-44b8c4c42b27.png)


Once the site has been successfully added, go to the site's settings and click the **NGINX Config** tab.

Since Directus, by default, is configured to run on port 8055, we will need to update the `proxy_pass` setting to reflect that.

Find the `proxy_pass` setting and set the port number to `8055`.

![Proxy Pass](https://user-images.githubusercontent.com/6209240/112153675-18e8a100-8c16-11eb-9acf-00cc459df5fc.png)

> Note that the port number for the app when you look at the site details will continue to display as the port number Cleavr automatically assigned. This will not impact deploying Directus.

Click **Update**.

## Add a database

On the same server you added the site, click on the database section to install a new MySQL (or optionally Postgres) database.

Once the database server is installed, add a new database. Remember the database name and database user credentials.

![Database](https://user-images.githubusercontent.com/6209240/112153719-269e2680-8c16-11eb-9dcc-94be5187aa47.png)

## Setup the web app

In the web app section, select **Complete Setup** for the web app that was created for the site created earlier.

### Enter repository

On the **Code Repository** tab, enter the following:

**Version Control Provider**: GitHub

**Repository**: direcvit/directus

**Branch**: main

![Repository](https://user-images.githubusercontent.com/6209240/112153803-42a1c800-8c16-11eb-9326-28c2edab8679.png)

Click **Update**.

> You will need a GitHub VC provider account created for this step.

### Set up NPM Build

Click on the NPM Build tab and fill in the following:

Entry Porint: npm

Arguments: start

Build Command: npm run build

![NPM Build](https://user-images.githubusercontent.com/6209240/112153868-54836b00-8c16-11eb-9881-b9f513c9124c.png)

Click **Update**.

### Add env variables

Click on the Environments section and add in the following environment variables:

```
PORT=8055
PUBLIC_URL=/

DB_CLIENT=mysql
DB_HOST=127.0.01
DB_PORT=3306
DB_DATABASE=directus
DB_USER=dbuser
DB_PASSWORD=pswd1234

RATE_LIMITER_ENABLED=true
RATE_LIMITER_STORE=memory
RATE_LIMITER_POINTS=50
RATE_LIMITER_DURATION=1

CACHE_ENABLED=false

STORAGE_LOCATIONS=local
STORAGE_LOCAL_DRIVER=local
STORAGE_LOCAL_ROOT=./uploads

KEY=xxxxxxx-xxxxxx-xxxxxxxx-xxxxxxxxxx
SECRET=xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx
ACCESS_TOKEN_TTL=15m
REFRESH_TOKEN_TTL=7d
REFRESH_TOKEN_COOKIE_SECURE=false
REFRESH_TOKEN_COOKIE_SAME_SITE=lax

EXTENSIONS_PATH=./extensions

EMAIL_FROM=no-reply@directus.io
EMAIL_TRANSPORT=sendmail
EMAIL_SENDMAIL_NEW_LINE=unix
EMAIL_SENDMAIL_PATH=/usr/sbin/sendmail
```

![Env Variables](https://user-images.githubusercontent.com/6209240/112153961-69f89500-8c16-11eb-9b14-450fe74be3c8.png)

Replace the environment variables with the appropriate information for your database that you setup earlier as well as the random key and secret.

See [Directus Environment Variables](https://docs.directus.io/reference/environment-variables/) for all available variables.

Click on **Sync**.

## Deploy!

Once you have everything configured, deploy! 🚀

After deployment, check the `Build Assets` logs. You will get the initial email and password to login to your Directus app.

![Initial Directus Admin Details](https://user-images.githubusercontent.com/6209240/112154457-e12e2900-8c16-11eb-9390-e977aee66295.png)

Use the email and password to login to your Directus App. You can change the credentials later.

![Directus Admin Login](https://user-images.githubusercontent.com/6209240/112154111-87c5fa00-8c16-11eb-8e6c-f00174d8fc57.png)

![Directus Collections](https://user-images.githubusercontent.com/6209240/112154121-8a285400-8c16-11eb-82db-6602caca5e8f.png)
