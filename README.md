# Graylog2 Logging for Laravel 5.x
[![License](https://poser.pugx.org/swisnl/textsnippet/license)](https://packagist.org/packages/swisnl/laravel-graylog)

## Installation

1. Run composer require for this package: `composer require swisnl/laravel-graylog`
2. Add the service provider to app.php if you don't like auto discovery: `Swis\Graylog2\Graylog2ServiceProvider`
3. Run `php artisan vendor:publish` to publish the config file to ./config/graylog2.php.
4. Configure it to your liking
5. Done!

## Logging exceptions
The default settings enable logging of exceptions. It will add the HTTP request to the GELF message, but it will not add POST values. Check the graylog2.log-requests config to enable or disable this behaviour.

### Don't report exceptions
In `app/Exceptions/Handler.php` you can define the $dontReport array with Exception classes that won't be reported to the logger. For example, you can blacklist the \Illuminate\Database\Eloquent\ModelNotFoundException. Check the [Laravel Documentation](https://laravel.com/docs/5.4/errors#the-exception-handler) about errors for more information.

### Send default additional data
Using the config, you can specify additional key => value data to be send with the GELF message. For example, you can use this to add a client ID to a message.

## Logging arbitrary data
You can instatiate the Graylog2 class to send additional GELF messages:

```
$graylog2 = app('graylog2');


// Send default log message
$graylog2->log('emergency', 'Dear Sir/Madam, Fire! Fire! Help me!. 123 Cavendon Road. Looking forward to hearing from you. Yours truly, Maurice Moss.', ['facility' => 'ICT']);


// Send custom GELF Message
$message = new \Gelf\Message();
$message->setLevel('emergency');
$message->setShortMessage('Fire! Fire! Help me!');
$message->setFullMessage('Dear Sir/Madam, Fire! Fire! Help me!. 123 Cavendon Road. Looking forward to hearing from you. Yours truly, Maurice Moss.');
$message->setFacility('ICT');
$message->setAdditional('employee', 'Maurice Moss');
$graylog2->logMessage($message);
```