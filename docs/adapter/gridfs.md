---
layout: default
title: MongoDB GridFS Adapter
permalink: /docs/adapter/gridfs/
redirect_from: /v2/docs/adapter/gridfs/
---

## Installation

```bash
composer require league/flysystem-gridfs:^3.0
```

## About

Interacting with MongoDB GridFS through Flysystem can be done
by using the `League\Flysystem\GridFS\GridFSAdapter`.

Read more about the MongoDB PHP Library in the [official documentation](https://www.mongodb.com/docs/php-library/).

## Simple usage:

```php
$client = new MongoDB\Client('mongodb://localhost:27017/');

// The internal adapter
$adapter = new League\Flysystem\GridFS\GridFSAdapter(
    // GridFS Bucket
    $client->selectDatabase('flysystem')->selectGridFSBucket()
);

// The FilesystemOperator
$filesystem = new League\Flysystem\Filesystem($adapter);
```

## Advanced usage:

```php
$client = new MongoDB\Client('mongodb://localhost:27017/');

// The internal adapter
$adapter = new League\Flysystem\GridFS\GridFSAdapter(
    // GridFS Bucket
    $client->selectDatabase('flysystem')->selectGridFSBucket([
        // Bucket name in the MongoDB database
        'bucketName' => 'project_files'
    ])
    // Optional path prefix
    'path/prefix',
);

// The FilesystemOperator
$filesystem = new League\Flysystem\Filesystem($adapter);
```

## Versioning:

In GridFS, file names are metadata to file objects identified by unique MongoDB `ObjectID`.
There may be more than one file with the same name, they are called "revisions":
- Reading a file reads the last revision of this file name
- Writing to a file name creates a new revision for this file name
- Renaming a file renames all the revisions of this file name
- Deleting a file deletes all the revisions of this file name

The GridFS Adapter for Flysystem does not provide access to a specific revision of a filename,
you must use the [GridFS API](https://www.mongodb.com/docs/php-library/current/tutorial/gridfs/)
if you need to work with revisions.
