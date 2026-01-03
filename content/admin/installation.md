---
title: "Installation"
date: 2019-02-13T19:35:37+01:00
weight: 10
---

## Requirements

System requirements can be found in the repository's
[README](https://github.com/engelsystem/engelsystem?tab=readme-ov-file#requirements).

## Download

Download instructions are available in the project's
[README](https://github.com/engelsystem/engelsystem?tab=readme-ov-file#download).

## Database

If you downloaded a release archive, it includes an `initial-install.sql` file that you can import into your database
using your web server providers phpMyAdmin webinterface or a similar tool.

If you're using the latest main branch, use the `migrate` script to set up and update the database:

```bash
bin/migrate up
```

## Files

### Configuration

After the initial database [configuration]({{% ref "configuration" %}}) in `config/config.php`
we recommend changing all other settings using the Engelsystem web interface as setting them in the config file
may cause issues when upgrading to newer versions.

```bash
echo "<?php
return [
    'database' => [
        'host' => 'localhost',
        'database' => 'engelsystem',
        'username' => 'engelsystem',
        'password' => '<your password here>',
    ],
    // Your configuration overrides could go here
];" > config/config.php
```

For more details see the [configuration]({{% ref "configuration" %}}) page.

## Webserver

### Apache

### nginx

## First login

The default installation includes an admin account with the credentials `admin` / `asdfasdf`.
**Change these immediately after logging in!**
