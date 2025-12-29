---
title: "Request Lifecycle"
date: 2024-04-08T22:20:00+02:00
---

This overview explains how the Engelsystem handles HTTP requests, which can help when debugging issues.

## Bootstrapping

All requests enter through `public/index.php`, which your web server (nginx, Apache, etc.)
should route to for any URL that doesn't correspond to a static file.

This file includes `includes/engelsystem.php`, which in turn loads `includes/application.php`
for bootstrapping and Composer autoloading. If maintenance mode is active, the template at
`resources/views/layouts/maintenance.html` is rendered and the request ends.

Bootstrapping loads the application configuration (`config/app.php`) and initializes the configured
service providers. Service providers initialize all required classes in two phases:

1. **Register**: Called first for all providers via their `register` method
2. **Boot**: Called after all providers are registered via their `boot` method

* **Register**: Use this to bind services to the container. Don't rely on other services or configuration
  being available yet, as they may not be loaded.
* **Boot**: Called after all services are registered. Use this to configure services that depend on
  other registered instances.

Service providers generally fall into three categories:

* **Application bootstrapping**: Logging, exception handling, configuration loading, environment detection
* **Request/response handling**: Session, authentication, validation, translations
* **Additional services**: Version info, mailers, etc.

## Dispatching

After the application is bootstrapped, the request is initialized and handed over to
the [PSR-15](https://www.php-fig.org/psr/psr-15/)
Request `Dispatcher` which handles the request by applying the middlewares configured in the app config.

### Middlewares

Middlewares process requests and responses by implementing `MiddlewareInterface`.
They can modify the request before it reaches the controller and modify the response before
it's sent to the client.

Middlewares handle tasks like:
- Setting and interpreting headers
- Filtering input and verifying CSRF tokens on POST requests
- Matching routes from `config/routes.php`
- Managing sessions
- Invoking the request handler

If the `RouteDispatcher` middleware can't find a matching route, it falls back to the
`LegacyMiddleware` to handle legacy pages in the `includes/` folder.

### Request Handler

The request handler determines which controller should handle the request based on route configuration.
It verifies required permissions (if configured in the controller) and invokes the appropriate callback.

## Events

Some functionality, like sending messages, is decoupled from the main request flow using events.
Available events are listed in the application configuration.
