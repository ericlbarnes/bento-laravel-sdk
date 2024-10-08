# Bento SDK for Laravel
[![Tests](https://github.com/bentonow/bento-laravel-sdk/actions/workflows/tests.yml/badge.svg?branch=main)](https://github.com/bentonow/bento-laravel-sdk/actions/workflows/tests.yml)


🍱 Simple way to send email in your Laravel projects!

👋 To get personalized support, please tweet @bento or email jesse@bentonow.com!

🐶 Battle-tested by [High Performance SQLite](https://highperformancesqlite.com/) (a Bento customer)!

⚡️ For event tracking and importing data into Bento please use our [Bento PHP SDK](https://github.com/bentonow/bento-php-sdk#Installation-Laravel).

❤️ Thank you [@aarondfrancis](https://github.com/aarondfrancis) for your contribution.
❤️ Thank you [@ziptied](*https%3A//github.com/ziptied*) for your contribution.

## Requirements

* [PHP 8.0+](https://php.net/releases/)
* [Laravel 10.0+](https://www.laravel.com)
* [Bento API Keys](https://app.bentonow.com/account/teams)

## Setup
#### Installation
You can install the package via composer:
```bash
composer require bentonow/bento-laravel-sdk
```

#### Configuration
First publish the configuration file
```bash
php artisan vendor:publish --tag bentonow
```

Next create a new mailer definition within your application's `config/mail.php` configuration file:
```php
'bento' => [
  'transport' => 'bento',
],
```
(Side note: we recommend using Laravel Mail Viewer when working in development (https://laravel-news.com/laravel-mail-viewer). 

Finally, visit your [bento profile](https://app.bentonow.com/account) and generate your API keys.

Add the API keys to your `.env` and set your **mail_mailer** to use Bento:
```*dotenv*
BENTO_PUBLISHABLE_KEY="{Publishable Key}"
BENTO_SECRET_KEY="{Secret Key}"
BENTO_SITE_UUID="{Site Key UUID}"
MAIL_MAILER="bento"
```

For additional information please refer to [Laravel Mail documentation](https://laravel.com/docs/mail)

# Usage 

Along with sending transactional email via Bento this library also gives you access Bento's REST API which makes it beautifully simple to work with.

### Tracking Events
```php
use Bentonow\BentoLaravel\BentoConnector;
use Bentonow\BentoLaravel\Requests\CreateEvents;
use Bentonow\BentoLaravel\DataTransferObjects\EventData;

$bento = new BentoConnector();

$data = collect([
  new EventData(
    type: "$completed_onboarding",
    email: "test@example.com",
    fields: [
      "first_name" => "John",
      "last_name" => "Doe"
    ]
  )
]);

$request = new CreateEvents($data);
$response = $bento->send($request);
return $data->json();
```

### Importing Subscribers
```php
use Bentonow\BentoLaravel\DataTransferObjects\ImportSubscribersData;
  use Bentonow\BentoLaravel\BentoConnector;
  use Bentonow\BentoLaravel\Requests\ImportSubscribers;

  $bento = new BentoConnector();

  $data = collect([
    new ImportSubscribersData(
      email: "test@test.com",
      first_name: "John",
      last_name: "doe",
      tags: ["lead", "mql"],
      removeTags: ["customers"],
      fields: ["role" => "ceo"]
    ),
  ]);

  $request = new ImportSubscribers($data);
  $response = $bento->send($request);
  return $response->json();
```

... and so much more! 

Visit our [API Documentation](https://docs.bentonow.com) to learn more. You can also read about basic [usage in our docs](https://docs.bentonow.com/quickstart#common-setups-patterns).

## Support and Feedback

In case you find any bugs, submit an issue directly here in GitHub.

Official API documentation is at [https://docs.bentonow.com](*https://docs.bentonow.com*)

## License

[The MIT License (MIT)](*LICENSE.md*)
