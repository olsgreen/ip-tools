# PHP IP Tools

[![Build Status](https://travis-ci.org/detain/php-ip-tools.svg?branch=master)](https://travis-ci.org/detain/php-ip-tools)
[![Latest Stable Version](https://img.shields.io/packagist/v/Detain/ip-tools.svg)](https://packagist.org/packages/detain/ip-tools)
[![Total Downloads](https://img.shields.io/packagist/dt/Detain/ip-tools.svg)](https://packagist.org/packages/detain/ip-tools)
[![Downloads Month](https://img.shields.io/packagist/dm/Detain/ip-tools.svg)](https://packagist.org/packages/detain/ip-tools)
[![License](https://img.shields.io/packagist/l/Detain/ip-tools.svg)](https://github.com/detain/php-ip-tools/blob/master/LICENSE.md)


Universal IP Tools for manipulation on IPv4 and IPv6.

## Require this package with Composer
Install this package through [Composer](https://getcomposer.org/).
Edit your project's `composer.json` file to require
`detain/ip-tools`.

Create *composer.json* file:
```js
{
	"name": "yourproject/yourproject",
	"type": "project",
	"require": {
		"detain/ip-tools": "~1.1.0"
	}
}
```
And run composer update

**Or** run a command in your command line:

```
composer require detain/ip-tools
```

## Usage
```php
<?php
$loader = require __DIR__.'/vendor/autoload.php';

use Detain\IPTools\Ip;

// Validating
$status = Ip::isValid('192.168.1.1'); // true

$status = Ip::isValid('192.168.1.256'); // false


// ip2long, long2ip

/// IPv4
$long = Ip::ip2long('192.168.1.1'); // 3232235777

$dec = Ip::long2ip('3232235777'); // 192.168.1.1

/// IPv6
$long = Ip::ip2long('fe80:0:0:0:202:b3ff:fe1e:8329'); // 338288524927261089654163772891438416681

$dec = Ip::long2ip('338288524927261089654163772891438416681', true); // fe80::202:b3ff:fe1e:8329


// Matching

/// IPv4
$status = Ip::match('192.168.1.1', '192.168.1.*'); // true

$status = Ip::match('192.168.1.1', '192.168.*.*'); // true

$status = Ip::match('192.168.1.1', '192.168.*.*'); // true

$status = Ip::match('192.168.1.1', '192.168.0.*'); // false


$status = Ip::match('192.168.1.1', '192.168.1/24'); // true

$status = Ip::match('192.168.1.1', '192.168.1.1/255.255.255.0'); // true

$status = Ip::match('192.168.1.1', '192.168.0/24'); // false

$status = Ip::match('192.168.1.1', '192.168.0.0/255.255.255.0'); // false


$status = Ip::match('192.168.1.5', '192.168.1.1-192.168.1.10'); // true

$status = Ip::match('192.168.5.5', '192.168.1.1-192.168.10.10'); // true

$status = Ip::match('192.168.5.5', '192.168.6.1-192.168.6.10');


$status = Ip::match('192.168.1.1', array('122.128.123.123', '192.168.1.*', '192.168.123.124')); // true

$status = Ip::match('192.168.1.1', array('192.168.123.*', '192.168.123.124'));

/// IPv6

$status = Ip::match('2001:cdba:0000:0000:0000:0000:3257:9652', '2001:cdba:0000:0000:0000:0000:3257:*'); // true

$status = Ip::match('2001:cdba:0000:0000:0000:0000:3257:9652', '2001:cdba:0000:0000:0000:0000:*:*'); // true

$status = Ip::match('2001:cdba:0000:0000:0000:0000:3257:9652',
					'2001:cdba:0000:0000:0000:0000:3257:1234-2001:cdba:0000:0000:0000:0000:3257:9999'); // true


$status = Ip::match('2001:cdba:0000:0000:0000:0000:3258:9652', '2001:cdba:0000:0000:0000:0000:3257:*'); // false

$status = Ip::match('2001:cdba:0000:0000:0000:1234:3258:9652', '2001:cdba:0000:0000:0000:0000:*:*'); // false

$status = Ip::match('2001:cdba:0000:0000:0000:0000:3257:7778',
					'2001:cdba:0000:0000:0000:0000:3257:1234-2001:cdba:0000:0000:0000:0000:3257:7777'); // false

```




-----
This code is available on
[Github](https://github.com/detain/php-ip-tools). Pull requests are welcome.

## Troubleshooting

If you like living on the edge, please report any bugs you find on the
[PHP IP Tools issues](https://github.com/detain/php-ip-tools/issues) page.

## Contributing

See [CONTRIBUTING.md](CONTRIBUTING.md) for information.

## License

Please see the [LICENSE](LICENSE.md) included in this repository for a full copy of the MIT license,
which this project is licensed under.

## Credits

- [Joe Huss aka Lord Detain](https://github.com/detain)
- [Avtandil Kikabidze aka LONGMAN](https://github.com/akalongman)

Full credit list in [CREDITS](CREDITS)
