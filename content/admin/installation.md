---
title: "Installation"
date: 2019-02-13T19:35:37+01:00
---

## Requirements

## Download

## Database

When using a release (you downloaded a release archive), you find an `install.sql` file which can be imported to your
webserver database through for example your providers phpMyAdmin webinterface.

If you are using the lastest main branch version you must use the `migrate` script to updates the Engelsystem:

```bash
bin/migrate up
```

## Files

### Configuration

It is highly recommended to create a new `config/config.php` file and copy the values that need to be changed over
from `config/config.default.php`.

```bash
echo '<?php
return [
  // New config goes here
];' > config/config.php
```

Alternatively you can also copy the whole `config/config.default.php` to `config/config.php` and edit
the `config/config.php` file to configure the engelsystem but that might lead to problems with further updates.

```bash
cp config/config.default.php config/config.php
```

The first and most important settings are the database connection parameters.

For more details see [configuration]({{% ref "configuration" %}}).

## Webserver

### Apache

### nginx

## First login

The Engelsystem installation provides a default user with the credentials `admin` :`asdfasdf`.
After logging in you should change them!
