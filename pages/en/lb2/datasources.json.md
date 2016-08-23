---
title: "datasources.json"
lang: en
layout: page
keywords: LoopBack
tags:
sidebar: lb2_sidebar
permalink: /doc/en/lb2/datasources.json.html
summary:
---

## Overview

Configure data sources in `/server/datasources.json`. You can set up as many data sources as you want in this file.

For example:

```javascript
{
  "db": {
    "name": "db",
    "connector": "memory"
  },
  "myDB": {
    "name": "myDB",
    "connector": "mysql",
    "host": "demo.strongloop.com",
    "port": 3306,
    "database": "demo",
    "username": "demo",
    "password": "L00pBack"
  }
}
```

To access data sources in application code, use `app.datasources._datasourceName_`.

## Standard properties

All data sources support a few standard properties. Beyond that, specific properties and defaults depend on the connector being used.

<table>
  <tbody>
    <tr>
      <th>Property</th>
      <th>Description</th>
    </tr>
    <tr>
      <td>connector</td>
      <td>
        <p>LoopBack connector to use; one of:</p>
        <ul>
          <li>memory</li>
          <li>loopback-connector-oracle or just "oracle"</li>
          <li>loopback-connector-mongodb or just "mongodb"</li>
          <li>loopback-connector-mysql or just "mysql"</li>
          <li>loopback-connector-postgresql or just "postgresql"</li>
          <li>loopback-connector-soap or just "soap"</li>
          <li>loopback-connector-mssql or just "mssql"</li>
          <li>
            <p>loopback-connector-rest or just "rest"</p>
          </li>
          <li>loopback-storage-service</li>
        </ul>
      </td>
    </tr>
    <tr>
      <td>name</td>
      <td>Name of the data source being defined.</td>
    </tr>
  </tbody>
</table>

## Properties for database connectors

To connect a model to a data source, follow these steps:

1.  Use the [data source generator](https://docs.strongloop.com/display/APIC/Data+source+generator) to create a new data source. For example: 

    ```shell
    $ apic create --type datasource
    ? Enter the data-source name: mysql-corp
    ? Select the connector for mysql: MySQL (supported by StrongLoop)
    ```

    ```shell
    $ slc loopback:datasource
    ? Enter the data-source name: mysql-corp
    ? Select the connector for mysql: MySQL (supported by StrongLoop)
    ```

    Follow the prompts to name the datasource and select the connector to use.
    This adds the new data source to [`datasources.json`](https://docs.strongloop.com/display/APIC/datasources.json).

2.  Edit `server/datasources.json` to add the necessary authentication credentials: typically hostname, username, password, and database name.

    For example:

    **server/datasources.json**

    ```javascript
    "mysql-corp": {
        "name": "mysql-corp",
        "connector": "mysql",
        "host": "your-mysql-server.foo.com",
        "user": "db-username",
        "password": "db-password",
        "database": "your-db-name"
      }
    ```

    For information on the specific properties that each connector supports, see:

    * [Cloudant connector](https://docs.strongloop.com/display/APIC/Cloudant+connector)
    * [DashDB](https://docs.strongloop.com/display/APIC/DashDB)
    * [DB2 connector](https://docs.strongloop.com/display/APIC/DB2+connector)
    * [DB2 for z/OS](https://docs.strongloop.com/pages/viewpage.action?pageId=10354945)
    * [Informix](https://docs.strongloop.com/display/APIC/Informix)
    * [Memory connector](https://docs.strongloop.com/display/APIC/Memory+connector)
    * [MongoDB connector](https://docs.strongloop.com/display/APIC/MongoDB+connector)
    * [MySQL connector](https://docs.strongloop.com/display/APIC/MySQL+connector)
    * [Oracle connector](https://docs.strongloop.com/display/APIC/Oracle+connector)
    * [PostgreSQL connector](https://docs.strongloop.com/display/APIC/PostgreSQL+connector)
    * [Redis connector](https://docs.strongloop.com/display/APIC/Redis+connector)
    * [SQL Server connector](https://docs.strongloop.com/display/APIC/SQL+Server+connector)
    * [SQLite3](https://docs.strongloop.com/display/APIC/SQLite3)
     
3.  Install the corresponding connector as a dependency of your app with `npm`, for example: 

    ```shell
    $ cd <your-app>
    $ npm install --save loopback-connector-mysql
    ```

    See [Connectors](/doc/en/lb2/Connecting-models-to-data-sources.html) for the list of connectors.

4.  Use the [model generator](https://docs.strongloop.com/display/APIC/Using+the+model+generator) to create a model.

    ```shell
    $ apic create --type model
    ? Enter the model name: myModel
    ? Select the data-source to attach myModel to: mysql (mysql)
    ? Select model's base class: PersistedModel
    ? Expose myModel via the REST API? Yes
    ? Custom plural form (used to build REST URL):
    Let's add some test2 properties now.
    ...
    ```

    ```shell
    $ slc loopback:model
    ? Enter the model name: myModel
    ? Select the data-source to attach myModel to: mysql (mysql)
    ? Select model's base class: PersistedModel
    ? Expose myModel via the REST API? Yes
    ? Custom plural form (used to build REST URL):
    Let's add some test2 properties now.
    ...
    ```

    When prompted for the data source to attach to, select the one you just created. 

    {% include note.html content="

    The model generator lists the [memory connector](https://docs.strongloop.com/display/APIC/Memory+connector), \"no data source,\"
    and data sources listed in [`datasources.json`](https://docs.strongloop.com/display/APIC/datasources.json).
    That's why you created the data source first in step 1.

    " %}

    You can also create models from an existing database.
    See [Creating models](https://docs.strongloop.com/display/APIC/Creating+models) for more information.

## Environment-specific configuration

You can override values set in `datasources.json` in the following files:

* `datasources.local.js` or `datasources.local.json`
* `datasources._env_.js` or `datasources._env_.json`, where _`env`_ is the value of `NODE_ENV` environment variable (typically `development` or `production`).
  For example, `datasources.production.json`.

{% include important.html content="

The additional files can override the top-level data-source options with string and number values only.
You cannot use objects or array values.

" %}

Example data sources:

**datasources.json**

```javascript
{
  // the key is the datasource name
  // the value is the config object to pass to
  // app.dataSource(name, config).
  db: {
    connector: 'memory'
  }
}
```

**datasources.production.json**

```javascript
{
  db: {
    connector: 'mongodb',
    database: 'myapp',
    user: 'myapp',
    password: 'secret'
  }
}
```