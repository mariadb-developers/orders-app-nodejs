# Orders

**Orders** is a web application that introduces you to the power, performance, and simplicity of [MariaDB](https://mariadb.com/products/) by simulating online eCommerce (ordering) traffic.

<p align="center" spacing="10">
    <kbd>
        <img src="media/demo.gif" />
    </kbd>
</p>

This application is made of two parts:

* Client
    - web UI that communicates with REST endpoints available through an API app (see below).
    - is a React.js project located in the [client](src/client) folder.
* API
    - uses the [MariaDB Node.js Connector](https://github.com/mariadb-corporation/mariadb-connector-nodejs) to connect to MariaDB.
    - is a Node.js project located in the [api](src/api) folder.

This README will walk you through the steps for getting the Orders web application up and running using MariaDB.

# Table of Contents
1. [Requirements](#requirements)
2. [Getting started with MariaDB](#mariadb)
3. [Get the code](#code)
4. [Create the database and table](#schema)
3. [Getting started](#get-started)
    1. [Configure](#configure-app)
    2. [Build and run the API app](#build-run-api)
    3. [Build and run the Client app](#build-run-client)
4. [Requirements to run the app](#requirements)
5. [Support and contribution](#support-contribution)
6. [License](#license)

## Requirements <a name="requirements"></a>

This sample application requires the following to be installed/enabled on your machine:

* [Node.js (v. 12+)](https://nodejs.org/docs/latest-v12.x/api/index.html)
* [NPM (v. 6+)](https://docs.npmjs.com/)
* [MariaDB command-line client](https://mariadb.com/products/skysql/docs/clients/mariadb-clients/mariadb-client/) (optional), used to connect to MariaDB database instances.

## 1.) Getting Started with MariaDB <a name="mariadb"></a>

[MariaDB](https://mariadb.com) is a community-developed, commercially supported relational database management system, and the database you'll be using for this application.

If you don't have a MariaDB database up and running you can find more information on how to download, install and start using a MariaDB database in the [MariaDB Quickstart Guide](https://github.com/mariadb-developers/mariadb-getting-started).

## 2.) Get the code <a name="code"></a>

Download this code directly or use [git](git-scm.org) (through CLI or a client) to retrieve the code using `git clone`:

```
$ git clone https://github.com/mariadb-developers/orders-app-nodejs.git
```

## 3.) Create the database and table  <a name="schema"></a>

[Connect to your MariaDB database](https://mariadb.com/products/skysql/docs/clients/) and execute the following:

```
$ mariadb --host host_address --port #### --user user_name -p**** < schema.sql
```

or executing the SQL within (schema.sql)(schema.sql) directly

```sql
CREATE DATABASE orders;

CREATE TABLE orders.orders (
  description varchar(25) 
) ENGINE=InnoDB;
```

## 4.) Configure, Build and Run the App <a name="app"></a>

This application is made of two parts:

* Client
    - web UI that communicates with REST endpoints available through an API app (see below).
    - is a React.js project located in the [client](src/client) folder.
* API
    - uses the [MariaDB Node.js Connector](https://github.com/mariadb-corporation/mariadb-connector-nodejs) to connect to MariaDB.
    - is a Node.js project located in the [api](src/api) folder.

### a.) Configure the app <a name="configure-app"></a>

Configure the MariaDB connection by [adding an .env file to the Node.js project](https://github.com/mariadb-corporation/mariadb-connector-nodejs/blob/master/documentation/promise-api.md#security-consideration). Add the .env file [here](src/api) (the `api` folder).

Example implementation:

```
DB_HOST=<host_address>
DB_PORT=<port_number>
DB_USER=<username>
DB_PASS=<password>
DB_NAME=orders
```

**Configuring db.js**

The environmental variables from `.env` are used within the [db.js](src/api/db.js) for the MariaDB Node.js Connector configuration pool settings:

```javascript
var mariadb = require('mariadb');
require('dotenv').config();

var pools = [
  mariadb.createPool({
    host: process.env.DB_HOST, 
    user: process.env.DB_USER, 
    password: process.env.DB_PASS,
    port: process.env.DB_PORT,
    database: process.env.DB_NAME,
    multipleStatements: true
  })
];
```

**Configuring db.js for the MariaDB cloud database service [SkySQL](https://mariadb.com/products/skysql/)**

MariaDB SkySQL requires SSL additions to connection. It's as easy as 1-2-3 (steps below).

```javascript
var mariadb = require('mariadb');
require('dotenv').config();

// 1.) Access the Node File System package
const fs = require("fs");

// 2.) Retrieve the Certificate Authority chain file (wherever you placed it - notice it's just in the Node project root here)
const serverCert = [fs.readFileSync("skysql_chain.pem", "utf8")];

var pools = [
  mariadb.createPool({
    host: process.env.DB_HOST, 
    user: process.env.DB_USER, 
    password: process.env.DB_PASS,
    port: process.env.DB_PORT,
    database: process.env.DB_NAME,
    connectionLimit: 5,
    // 3.) Add an "ssl" property to the connection pool configuration, using the serverCert const defined above
    ssl: {
      ca: serverCert
    }
  })
];
```

### b.) Build and run the app <a name="build-run-api"></a>

To start and run the API application you need to execute the following commands within the [api root folder](src/api).

1. Install the Node.js packages (dependendies) for the app.

```bash
$ npm install
```

2. Run the the app, which will expose API endpoints via http://localhost:8080.

```bash 
$ npm start
``` 

### c.) Build and run the [UI (Client) app](src/client) <a name="build-run-client"></a>

Once the API project is running you can now communicate with the exposed endpoints directly (via HTTP requests) or with the application UI, which is contained with the [client](src/client) folder of this repo.

To start and run the API application you need to execute the following commands within the [client root folder](src/client).

1. Install the Node.js packages (dependendies) for the app.

```bash
$ npm install
```

2. Run the the app, which will be available via a browser at http://localhost:3000.

```bash 
$ npm start
``` 

## Support and Contribution <a name="support-contribution"></a>

Please feel free to submit PR's, issues or requests to this project project directly.

If you have any other questions, comments, or looking for more information on MariaDB please check out:

* [MariaDB Developer Hub](https://mariadb.com/developers)
* [MariaDB Community Slack](https://r.mariadb.com/join-community-slack)

Or reach out to us diretly via:

* [developers@mariadb.com](mailto:developers@mariadb.com)
* [MariaDB Twitter](https://twitter.com/mariadb)

## License <a name="license"></a>
[![License](https://img.shields.io/badge/License-MIT-blue.svg?style=plastic)](https://opensource.org/licenses/MIT)
