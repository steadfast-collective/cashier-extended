<p align="center"><img src="https://laravel.com/assets/img/components/logo-cashier.svg"></p>

<p align="center">
<a href="https://travis-ci.org/laravel/cashier"><img src="https://travis-ci.org/laravel/cashier.svg" alt="Build Status"></a>
<a href="https://packagist.org/packages/laravel/cashier"><img src="https://poser.pugx.org/laravel/cashier/d/total.svg" alt="Total Downloads"></a>
<a href="https://packagist.org/packages/laravel/cashier"><img src="https://poser.pugx.org/laravel/cashier/v/stable.svg" alt="Latest Stable Version"></a>
<a href="https://packagist.org/packages/laravel/cashier"><img src="https://poser.pugx.org/laravel/cashier/license.svg" alt="License"></a>
</p>

## Introduction

Laravel Cashier provides an expressive, fluent interface to [Stripe's](https://stripe.com) subscription billing services. It handles almost all of the boilerplate subscription billing code you are dreading writing. In addition to basic subscription management, Cashier can handle coupons, swapping subscription, subscription "quantities", cancellation grace periods, and even generate invoice PDFs.

Cashier Extended improves upon the core of Laravel Cashier by adding a store of Charges made, and additional methods to query them, as well as webhooks to keep this updated.

## Official Documentation

Documentation for Laravel Cashier can be found on the [Laravel website](https://laravel.com/docs/billing), there are several changes to make when using Laravel Cashier Extended, which are detailed below.

## Installation

## Installation

To get started install the package from composer:

```bash
composer require steadfastcollective/cashier-extended
```

The package will automatically register the service provider and facade.

Next publish the migrations with:

```bash
php artisan vendor:publish --provider="SteadfastCollective\CashierExtended\CashierExtendedServiceProvider" --tag="migrations"
```

and then run them:

```bash
php artisan migrate
```

As noted before, we have made a couple of changes to how you use the Cashier package these are detailed below.

The Billable trait has been updated and should use the new namespace:

```php
<?php

// use Laravel\Cashier\Billable;
use SteadfastCollective\CashierExtended\Billable;
```

The following webhooks have been registered to keep your data up to date:

* charge.expired
* charge.failed
* charge.refund.updated
* charge.refunded
* charge.succeeded
* charge.updated

to make use of the new webhooks you will need to update your `routes\Web.php` route file:

```php
<?php

// Route::post(
//     'stripe/webhook',
//     '\App\Http\Controllers\WebhookController@handleWebhook'
// );

Route::post(
    'stripe/webhook',
    '\SteadfastCollective\CashierExtended\Http\Controllers\WebhookController@handleWebhook'
);
```

If you have extended or would like to extend the WebhookController and add addtional webhooks you can create the following file `app\Http\Controllers\WebhookController.php`:

```php
<?php

namespace App\Http\Controllers;

use SteadfastCollective\CashierExtended\Http\Controllers\WebhookController as CashierController;

class WebhookController extends CashierController
{
    /**
     * Handle invoice payment succeeded.
     *
     * @param  array  $payload
     * @return \Symfony\Component\HttpFoundation\Response
     */
    public function handleInvoicePaymentSucceeded($payload)
    {
        // Handle The Event
    }
}
```

Then be sure to update your `routes\Web.php` file:

```php
<?php

Route::post(
    'stripe/webhook',
    '\App\Http\Controllers\WebhookController@handleWebhook'
);
```

### Testing

``` bash
composer test
```

### Changelog

Please see [CHANGELOG](CHANGELOG.md) for more information what has changed recently.

## Contributing

Please see [CONTRIBUTING](CONTRIBUTING.md) for details.

### Security

If you discover any security related issues, please email daniel@steadfastcollective.com instead of using the issue tracker.

## Credits

- [Daniel Jones](https://github.com/steadfastcollective)
- [All Contributors](../../contributors)

## License

The MIT License (MIT). Please see [License File](LICENSE.md) for more information.
