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

If you downloaded a release archive, it includes an `install.sql` file that you can import into your database
using phpMyAdmin or a similar tool.

If you're using the latest main branch, use the `migrate` script to set up or update the database:

```bash
bin/migrate up
```

## Files

### Configuration

We recommend creating a `config/config.php` file containing only the settings you want to change:

```bash
echo '<?php
return [
  // Your configuration overrides go here
];' > config/config.php
```

Alternatively, you can copy `config/config.default.php` to `config/config.php` and edit it directly,
though this may cause issues when upgrading to newer versions.

```bash
cp config/config.default.php config/config.php
```

The first and most important settings are the database connection parameters.

For more details see [configuration]({{% ref "configuration" %}}).

## Webserver

### Apache

### nginx

## First login

The default installation includes an admin account with the credentials `admin` / `asdfasdf`.
**Change these immediately after logging in!**
