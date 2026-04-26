---
title: "Plugins"
date: 2026-04-25T12:11:00+02:00
---

{{% notice warning %}}
Plugins are currently a **beta** feature and interfaces might change without notice.
{{% /notice %}}

Engelsystem Plugins can extend the core functionality and add new features to the Engelsystem.  
They are normally installed by copying the directory or cloning their repo to `resources/plugins/`.

## Structure

Every Plugin needs a `composer.json` definition to configure its state,
it must be located in a directory that matches the plugins name.
The plugin name should contain a prefix unique to its creating entity.

The general structure should be:

```
┐ DemoPlugin
├── composer.json
├── lang
│   ├── de_DE.po
│   └── en_US.po
├── src
│   ├── Controller.php
│   ├── DemoPlugin.php
│   ├── EventHandler.php
│   ├── Middleware.php
│   ├── Migrations
│   │   └── 2026_01_03_000000_demo_plugin_init.php
│   └── ServiceProvider.php
└── views
    ├── admin
    │   └── config
    │       └── settings.twig
    └── demo-plugin
        └── page.twig
```

### composer.json (required)

The `composer.json` contains a plugins general metadata and configuration.
It must match the requirements of the [composer schema](https://getcomposer.org/doc/04-schema.md#properties),
dependency resolution is not yet supported.

Example:

```json
{
  "type": "engelsystem-plugin",
  "name": "demo/plugin",
  "description": "This is a demo plugin",
  "version": "0.0.1-beta",
  "license": "LGPL-2.0-or-later",
  "homepage": "https://engelsystem.de",
  "authors": [
    {
      "name": "Demo Author",
      "email": "test@example.com"
    }
  ],
  "autoload": {
    "psr-4": {
      "Demo\\Plugin\\": "src/"
    }
  },
  "extra": {
    "providers": [
      "Demo\\Plugin\\ServiceProvider"
    ],
    "middleware": [
      "Demo\\Plugin\\Middleware"
    ],
    "event_handlers": {
      "news.created": "Demo\\Plugin\\EventHandler",
      "news.updated": "Demo\\Plugin\\EventHandler"
    },
    "config_options": {
      "event": {
        "config": {
          "demo_option": {
            "type": "string",
            "default": "Demo config option!"
          }
        }
      }
    },
    "routes": {
      "/demo": "Demo\\Plugin\\Controller@handle",
      "/demo/save": [
        "Demo\\Plugin\\Controller@save",
        "POST"
      ]
    }
  }
}
```

#### type (required)

The type must be `engelsystem-plugin`.

#### name (required)

The name must contain a prefix of the entity creating the Plugin.

#### description

Description of the plugin, shown in the backend.

#### version

The current version of the plugin, updates are installed when changed. Defaults to `0.0.0`

#### license

A license should be provided to clarify allowed plugin usage. Defaults to `proprietary`.

#### homepage

Links to the plugins homepage.

#### authors

A list of plugin authors

#### autoload

Autoloader configuration for the plugin code.
Multiple namespaces are possible, at the moment we only support `psr-4`.
If no autoloader is defined a base of the plugin name in the plugins root directory is assumed.

#### extra

General plugin configuration to extend the Engelsystem.

##### providers

A list of service providers to allow extension / wrapping of classes using the application container
and configuration of the plugin resources.

##### middleware

A list of middlewares to be loaded on every request.

##### event_handlers

Event handlers mapping to hook on any Engelsystem changes.

##### config_options

Allows the creation and extension of engelsystem config options to simplify plugin configuration.
Syntax is equivalent to the Engelsystems `config/app.php`.

##### routes

A mapping of routes and their equivalent controller endpoints, configuration equivalent to `config/routes.php`.

### lang/

This directory should contain the plugins language files,
directory structure is equivalent to the Engelsystems `resources/lang`.  
Translations should use a plugin specific prefix

### src/

It is recommended to use `src/` as the base directory for any PHP classes.

### src/[PluginName].php

The plugin specific class must extend `\Engelsystem\Plugins\Plugin`,
its namespace must be from the first autoload entry and can be used for a
more advanced / dynamic setup of options as it provides all information from `composer.json`.  
It can also be used to handle events when the plugin state changes:

* `install()`
* `uninstall()`
* `update()`
* `enable()`
* `disable()`

When loaded (on every request):

* `boot()`

Routes setup:

* `loadRoutes()`

### src/Migrations/

Base directory for all plugin migrations.
Namespace must be the first defined in `composer.json`, same format as Engelsystem `db/migrations`.
Migrations should be as permissive as possible as other plugins might create data structures too.
They should not remove data that might be referenced by the engelsystem or other plugins.
The Plugin must work without all migrations run
(migrations are run on user button press witch not necessarily coincides with the file update).

### views/

Directory for templates. Added templates should be put in a directory matching the plugin name.
Plugins can extend any core template or templates of other plugins (when loaded after another),
multi-inheritance is possible to append content to a block from multiple plugins.
