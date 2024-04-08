---
title: "Configuration"
date: 2019-02-13T19:39:21+01:00
weight: 20
---

For a full list of available configuration options, see your `config/config.default.php` file and its comments.

## Database Connection

Make sure you enter the 4 parameters for the MySQL Connection:

* `host`: Most often this is `localhost`, the db host to connect to
* `database`: Name of the MySQL database
* `username`: Name of the database user, who has select, update, insert and delete privileges on the database
* `password`: Password of the database user

```php
    'database'                => [
        'host'     => 'localhost',
        'database' => 'engelsystem',
        'username' => 'engelsystem',
        'password' => '<your password here>',
    ]
```

## API Key

The api key is used to protect access to the engelsystem api. It has no restrictions in lengths or character classes.

The api key has to put in the http request as request paramter `api_key`.

```php
    'api_key'                 => '<your api key here>',
```

## Maintenance mode

If you want to take the engelsystem down, set this boolean to `true`.
The content of `resources/views/layouts/maintenance.html` is then showed to the users.
No functions are available then.

This defaults to environment variable `MAINTENANCE`. If the variable is not present, it defaults to `false`.

```php
    'maintenance'             => false,
```

## App name

With app name you may rename the engelsystem to any name you want.

This defaults to environment variable `APP_NAME`. If the variable is not present, it defaults to `Engelsystem`.

```php
    'app_name'                => 'Engelsystem',
```

## Environment

Can either be set to `production` or `development`. Set to `development` to enable debugging messages.

This defaults to environment variable `ENVIRONMENT`. If the variable is not present, it defaults to `production`.

```php
    'environment'             => 'production',
```

## Footer links

With the footer links configuration you may add more html links to the app footer.
This item is an array and the keys are the link name, the value is the url.

Look at the default as sample:

```php
    'footer_items'            => [
        'FAQ'     => 'https://events.ccc.de/congress/2018/wiki/Static:Volunteers',
        'Contact' => 'mailto:ticket@c3heaven.de',
    ],
```

## Email settings

```php
    'email'                   => [
        // Can be mail, smtp, sendmail or log
        'driver' => env('MAIL_DRIVER', 'mail'),
        'from'   => [
            // From address of all emails
            'address' => env('MAIL_FROM_ADDRESS', 'noreply@engelsystem.de'),
            'name'    => env('MAIL_FROM_NAME', env('APP_NAME', 'Engelsystem'))
        ],

        'host'       => env('MAIL_HOST', 'localhost'),
        'port'       => env('MAIL_PORT', 587),
        // Transport encryption like tls
        'encryption' => env('MAIL_ENCRYPTION', null),
        'username'   => env('MAIL_USERNAME'),
        'password'   => env('MAIL_PASSWORD'),
        'sendmail'   => env('MAIL_SENDMAIL', '/usr/sbin/sendmail -bs'),
    ],
```

## Default theme

```php
    // Default theme, 1=style1.css
    'theme'                   => env('THEME', 1),
```

## Available themes

```php
    // Some available themes
    'themes'        => [
        // ...
        '7' => 'Engelsystem 35c3 dark (2018)',
        '6' => 'Engelsystem 34c3 dark (2017)',
        '5' => 'Engelsystem 34c3 light (2017)',
        '4' => 'Engelsystem 33c3 (2016)',
        '3' => 'Engelsystem 32c3 (2015)',
        '2' => 'Engelsystem cccamp15',
        '0' => 'Engelsystem light',
        '1' => 'Engelsystem dark',
    ],
```

## News pagination

```php
    // Number of News shown on one site
    'display_news'            => 10,
```

## Allow user registration

```php
    // Users are able to sign up
    'registration_enabled'    => (bool)env('REGISTRATION_ENABLED', true),
```

## Shift signup requires arrival

```php
    // Only arrived angels can sign up for shifts
    'signup_requires_arrival' => false,
```

## Shift unsubscribe timing

```php
    // Number of hours that an angel has to sign out own shifts
    'last_unsubscribe'        => 3,
```

## Password algorithm

```php
    // Define the algorithm to use for `password_verify()`
    // If the user uses an old algorithm the password will be converted to the new format
    // See https://secure.php.net/manual/en/password.constants.php for a complete list
    'password_algorithm'               => PASSWORD_DEFAULT,
```

## Password min. length

```php
    // The minimum length for passwords
    'min_password_length'     => 8,
```

## Enable goodies/t-shirts

```php
    // Resembles the Goodie Type. There are three options:
    // 'none' => no goodie at all
    // 'goodie' => a goodie which has no sizing options
    // 'tshirt' => goodie that is called tshirt and has sizing options
    'goodie_type'      => 'tshirt',
```

## Freeloading

```php
    // Number of shifts to freeload until angel is locked for shift signup.
    'max_freeloadable_shifts' => 2,
```

## Timezone

```php
    // Local timezone
    'timezone'                => env('TIMEZONE', 'Europe/Berlin'),
```

## Nighshift bonus

```php
    // Multiply 'night shifts' and freeloaded shifts (start or end between 2 and 6 exclusive) by 2
    'night_shifts'            => [
        'enabled'    => true, // Disable to weigh every shift the same
        'start'      => 2,
        'end'        => 8,
        'multiplier' => 2,
    ],
```

## Voucher calculation

```php
    // Voucher calculation
    'voucher_settings'        => [
        'initial_vouchers'   => 0,
        'shifts_per_voucher' => 1,
        'hours_per_voucher' => 2,
        // 'Y-m-d' formatted
        'voucher_start' => null,
    ],
```

## Available languages

```php
    // Available locales in /locale/
    'locales'                 => [
        'de_DE.UTF-8' => 'Deutsch',
        'en_US.UTF-8' => 'English',
    ],
```

## Default language

```php
    // The default locale to use
    'default_locale'          => env('DEFAULT_LOCALE', 'en_US.UTF-8'),
```

## Available t-shirt sizes

```php
    // Available T-Shirt sizes, set value to null if not available
    'tshirt_sizes'            => [
        'S'    => 'Small Straight-Cut',
        'S-G'  => 'Small Fitted-Cut',
        'M'    => 'Medium Straight-Cut',
        'M-G'  => 'Medium Fitted-Cut',
        'L'    => 'Large Straight-Cut',
        'L-G'  => 'Large Fitted-Cut',
        'XL'   => 'XLarge Straight-Cut',
        'XL-G' => 'XLarge Fitted-Cut',
        '2XL'  => '2XLarge Straight-Cut',
        '3XL'  => '3XLarge Straight-Cut',
        '4XL'  => '4XLarge Straight-Cut',
    ],
```

## Session management

```php
    // Session config
    'session'                 => [
        // Supported: pdo or native
        'driver' => env('SESSION_DRIVER', 'pdo'),

        // Cookie name
        'name'   => 'session',
 
        // Lifetime in days
        'lifetime' => env('SESSION_LIFETIME', 30),
    ],
```

## Trusted proxies

```php
    // IP addresses of reverse proxies that are trusted, can be an array or a comma separated list
    'trusted_proxies'         => env('TRUSTED_PROXIES', ['127.0.0.0/8', '::ffff:127.0.0.0/8', '::1/128']),
```

## Additional http headers

```php
    // Add additional headers
    'add_headers'             => (bool)env('ADD_HEADERS', true),
    // To disable a header in the config.php, you can set its value to null
    'headers'                 => [
        'X-Content-Type-Options'  => 'nosniff',
        'X-Frame-Options'         => 'sameorigin',
        'Referrer-Policy'         => 'strict-origin-when-cross-origin',
        'Content-Security-Policy' => 'default-src \'self\'; '
            . ' style-src \'self\' \'unsafe-inline\'; img-src \'self\' data:;',
        'X-XSS-Protection'        => '1; mode=block',
        'Feature-Policy'          => 'autoplay \'none\'',
        //'Strict-Transport-Security' => 'max-age=7776000',
        //'Expect-CT' => 'max-age=7776000,enforce,report-uri="[uri]"',
    ],
```

## Engelsystem credits

```php
    // A list of credits
    'credits'                 => [
        'Contribution' => 'Please visit [engelsystem/engelsystem](https://github.com/engelsystem/engelsystem) if '
            . 'you want to to contribute, have found any [bugs](https://github.com/engelsystem/engelsystem/issues) '
            . 'or need help.'
    ]
```
