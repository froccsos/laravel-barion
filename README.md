# Use the Barion API with Laravel

Laravel-Barion is provides an easy way to use the Barion API with Laravel applications.
Under the hood there is just a thin wrapper to make API calls simple.
 
Forked from laravelhungary/laravel-barion. 
 
## Installation

1. Install the package using composer: 

`composer require laravelhungary/laravel-barion`

**If you're using Laravel 5.5, you're done, the following steps are being done automatically.**

2. Register the service provider in the `app.php` config file

```php
LaravelHungary\Barion\BarionServiceProvider::class,
```

3. Register the Barion facade (optional)

```php
'Barion' =>  LaravelHungary\Barion\BarionFacade::class
```

## Configuration

Laravel-Barion comes preconfigured, you only need to set your POS key in the
 .env file: 
 
 `BARION_POS_KEY=<my pos key>`
 
 The Barion environment defaults to `test.barion.com`. To use the live instead,
 set 
 
 `BARION_LIVE_ENV=true`.
 
 If you'd like to tweak the configuration values, publish 
 the config file:
 
 `artisan vendor:publish --provider="LaravelHungary\Barion\BarionServiceProvider"`

## Usage
You can either resolve the `Barion` class from the Service Container using Laravel's
dependency injection, or simply use the provided Facade.

There are two convenience methods for the two most-often used API calls:

#### Get the payment status
```php
Barion::getPaymentState('my-payment-id')
```

#### Start a Payment
```php
Barion::paymentStart([
    'PaymentType' => PaymentType::IMMEDIATE,
    'GuestCheckOut' => true,
    'FundingSources' => [FundingSource::ALL],
    'Locale' => Locale::HU,
    'Currency' => Currency::HUF,
    'Transactions' => [
        [
            'POSTransactionId' => 'ABC-1234',
            'Payee' => 'example@email.com',
            'Total' => 4990,
            'Items' => [
                [
                    'Name' => 'Example item',
                    'Description' => 'This is a sample description',
                    'Quantity' => 1,
                    'Unit' => 'db',
                    'UnitPrice' => 4990,
                    'ItemTotal' => 4990
                ]
            ]
         ]
    ]
])
```

All other API calls are accessible using either `get` or `post`:

```php
Barion::get('/api/url')
```

```php
Barion::post('/api/url', ['my-data' => 'some value'])
```

POS Key is automatically appended to each request.

## License
Laravel-Barion is open source software licensed under the [MIT License](https://opensource.org/licenses/MIT).

