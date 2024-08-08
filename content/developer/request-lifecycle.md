---
title: "Request Lifecycle"
date: 2024-04-08T22:20:00+02:00
---

The goal of this overview is to give you a basic understanding of how the Engelsystem handles a request
and to provide a way to debug problems if they occur.

## Bootstrapping

The first entry point for all requests is the `public/index.php` file.
It should be called by the webserver (nginx, Apache etc.) on all incoming requests that are not files.

It includes the `includes/engelsystem.php` which itself includes `includes/application.php` for application
bootstrapping and composer autoloading.
This also handles a potential active maintenance mode by rendering the corresponding template
from `resources/views/layouts/maintenance.html` and ending the request.

The application bootstrapping includes loading the application configuration first (`config/app.php`) and
then bootstrapping any configured providers.

Service Providers handle the initialization of all needed classes (and their instances) by first registering them
(and calling their `register` method) and afterward calling all provider `boot` methods.

* The `register` step should be used to initialize any functionality. It should not depend on other instances or
  configuration being available as they might not be loaded at that point.
* The `boot` step is called after the configuration and all instances have been registered and thus are available.
  This should be used to configure the state of a service.

Service Providers can be broadly differentiated between

* application bootstrapping like logging, exception handling, loading the configuration and its overwritten values,
  detecting the runtime environment.
* initializing the request and response handling like session, authentication, validation, translations etc.
* additional services like providing the version, mailers and others.

## Dispatching

After the application is bootstrapped, the request is initialized and handed over to
the [PSR-15](https://www.php-fig.org/psr/psr-15/)
Request `Dispatcher` which handles the request by applying the middlewares configured in the app config.

### Middlewares

Middlewares are used to handle anything that changes the request or response.
For that they must implement the `MiddlewareInterface`.
A Middleware can update the request before it is handled by other parts of the application and
change parts of the response before it is sent to the requesting client.

Middlewares are used to interpret and set headers, filter the input and verify the csrf token on post requests,
analyze which route from `config/routes.php` is requested, handle the session and call the `RequestHandler`.

When the `RouteDispatcher` middleware can't find a matching route it uses the `LegacyMiddleware` to
resolve any remaining old pages located inside the `includes/` folder.

### Request Handler

The request handler loads the route information to get the controller that should be used to handle a given request.
It checks for needed permissions (when configured in the controller) and calls the defined callback.

## Events

Some functionality like sending messages is decoupled from the normal flow by using events.
A list can be found in the application configuration.
