![image_squidhome@2x.png](http://i.imgur.com/RIvu9.png)

# PostgreSQL Sails/Waterline Adapter

[![Build Status](https://travis-ci.org/FernandoFranco/sails-postgresql-pp.svg?branch=master)](https://travis-ci.org/FernandoFranco/sails-postgresql-pp)
[![npm version](https://badge.fury.io/js/sails-postgresql-pp.svg)](https://badge.fury.io/js/sails-postgresql-pp)

A [Waterline](https://github.com/balderdashy/waterline) adapter for PostgreSQL. May be used in a [Sails](https://github.com/balderdashy/sails) app or anything using Waterline for the ORM.
This is a modified branch of the [sails-postgresql v0.11.4](https://github.com/balderdashy/sails-postgresql/tree/0.11.x)

## Install

Install is through NPM.

```bash
$ npm install sails-postgresql-pp
```

## Configuration

The following config options are available along with their default values:

```javascript
config: {
  database: 'databaseName',
  host: 'localhost',
  user: 'root',
  password: '',
  port: 5432,
  poolSize: 10,
  ssl: false
};
```
Alternatively, you can supply the connection information in URL format:
```javascript
config: {
  url: 'postgres://username:password@hostname:port/database',
  ssl: false
};
```


We are also testing features for future versions of waterline in postgresql. One of these is case sensitive string searching. In order to enable this feature today you can add the following config flag:

```javascript
postgresql: {
  url: 'postgres://username:password@hostname:port/database',
  wlNext: {
    caseSensitive: true
  }
}
```

## Model Level Config

You can use model level config options to specify a `schema` to use. This is done by adding the `meta` key `schemaName`.

```javascript
module.exports = Waterline.Collection.extend({
  tableName: 'user',
  meta: {
    schemaName: 'foo'
  },

  identity: 'user',
  connection: 'myAwesomeConnection',

  attributes: {
    name: 'string'
  }
});
```

## Change Model Schema in runtime

You can change schema name with a similar code:

```javascript
Model.metas({schemaName: 'bar'}).find().exec(function (err, records) {
  if (err) return console.error(err);
  console.log('Change Schema :: ', records);
});
```

## Clone Schema Tables in new or existing Schema

You can clone a specific model like this:

```javascript
Model.cloneToSchema({newSchema: 'bar'}).exec(function (err, records) {
  if (err) return console.error(err);
  console.log('Clone Schema Success :: ', records);
});
```

Or yout clone all tables in the schema like this:

```javascript
var sailsPostgreSqlPpAdapter = sails.adapters['sails-postgresql-pp'];
var sailsConfigModelsConnection = sails.config.models.connection;

sailsPostgreSqlPpAdapter.cloneToSchema(sailsConfigModelsConnection, null, {
  oldSchema: 'foo',
  newSchema: 'bar'
}, function (err, resolve) {
  if (err) return console.error(err);
  console.log('Clone Schema Success :: ', { oldSchema: 'foo', newSchema: 'bar'});
});
```

## Testing

Test are written with mocha. Integration tests are handled by the [waterline-adapter-tests](https://github.com/balderdashy/waterline-adapter-tests) project, which tests adapter methods against the latest Waterline API.

To run tests:

```bash
$ npm test
```

## About Waterline

Waterline is a new kind of storage and retrieval engine.  It provides a uniform API for accessing stuff from different kinds of databases, protocols, and 3rd party APIs.  That means you write the same code to get users, whether they live in mySQL, LDAP, MongoDB, or Facebook.

To learn more visit the project on GitHub at [Waterline](https://github.com/balderdashy/waterline).
