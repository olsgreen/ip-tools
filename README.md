# IPv4/IPv6 and Subnet Calculator (PHP)

Universal Network calculator and IP Tools for manipulation on IPv4 addresses, IPv6 addresses; and subnet mask manipulation and getting (CIDR) network information.

[![Build Status](https://travis-ci.org/detain/ip-tools.svg?branch=master)](https://travis-ci.org/detain/ip-tools)
[![Latest Stable Version](https://img.shields.io/packagist/v/Detain/ip-tools.svg)](https://packagist.org/packages/detain/ip-tools)
[![Total Downloads](https://img.shields.io/packagist/dt/Detain/ip-tools.svg)](https://packagist.org/packages/detain/ip-tools)
[![Downloads Month](https://img.shields.io/packagist/dm/Detain/ip-tools.svg)](https://packagist.org/packages/detain/ip-tools)
[![License](https://img.shields.io/packagist/l/Detain/ip-tools.svg)](https://github.com/detain/ip-tools/blob/master/LICENSE)
[![Coverage Status](https://coveralls.io/repos/github/detain/ip-tools/badge.svg?branch=master)](https://coveralls.io/github/detain/ip-tools?branch=master)

Features
--------
Given an IP address and CIDR network size, it calculates the network information and provides all-in-one aggregated reports.

### Calculations
 * IP address
 * Network size
 * Subnet mask
 * Network portion
 * Host portion
 * Number of IP addresses in the network
 * Number of addressable hosts in the network
 * IP address range
 * Broadcast address
 * Min and max host

Provides each data in dotted quads, hexadecimal, and binary formats, as well as array of quads.

### Aggregated Network Calculation Reports
 * Associative array
 * JSON
 * String
 * Printed to STDOUT

Setup
-----


### Minimum Requirements
 * PHP 5.3.0

 Add the library to your `composer.json` file in your project:

```javascript
{
  "require": {
	  "markrogoyski/ipv4-subnet-calculator": "2.*"
  }
}
```

Use [composer](http://getcomposer.org) to install the library:

```bash
$ php composer.phar install
```

Composer will install IPv4 Subnet Calculator inside your vendor folder. Then you can add the following to your
.php files to the use library with Autoloading.

```php
require_once(__DIR__ . '/vendor/autoload.php');
```


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




Usage
-----

### Create New Subnet
```php
// For network 192.168.112.203/23
$sub = new Net\Subnet('192.168.112.203', 23);
```

### Various Network Information
```php
$number_ip_addresses    = $sub->getNumberIPAddresses();      // 512
$number_hosts           = $sub->getNumberAddressableHosts(); // 510
$address_rage           = $sub->getIPAddressRange();         // [192.168.112.0, 192.168.113.255]
$addressable_host_range = $sub->getAddressableHostRange();   // [192.168.112.1, 192.168.113.254]
$network_size           = $sub->getNetworkSize();            // 23
$broadcast_address      = $sub->getBroadcastAddress();       // 192.168.113.255
```

### IP Address
```php
$ip_address        = $sub->getIPAddress();       // 192.168.112.203
$ip_address_quads  = $sub->getIPAddressQuads();  // [192, 168, 112, 203]
$ip_address_hex    = $sub->getIPAddressHex();    // C0A870CB
$ip_address_binary = $sub->getIPAddressBinary(); // 11000000101010000111000011001011
```

### Subnet Mask
```php
$subnet_mask        = $sub->getSubnetMask();       // 255.255.254.0
$subnet_mask_quads  = $sub->getSubnetMaskQuads();  // [255, 255, 254, 0]
$subnet_mask_hex    = $sub->getSubnetMaskHex();    // FFFFFE00
$subnet_mask_binary = $sub->getSubnetMaskBinary(); // 11111111111111111111111000000000
```

### Network Portion
```php
$network        = $sub->getNetworkPortion();       // 192.168.112.0
$network_quads  = $sub->getNetworkPortionQuads();  // [192, 168, 112, 0]
$network_hex    = $sub->getNetworkPortionHex();    // C0A87000
$network_binary = $sub->getNetworkPortionBinary(); // 11000000101010000111000000000000
```

### Host Portion
```php
$host        = $sub->getHostPortion();       // 0.0.0.203
$host_quads  = $sub->getHostPortionQuads();  // [0, 0, 0, 203]
$host_hex    = $sub->getHostPortionHex();    // 000000CB
$host_binary = $sub->getHostPortionBinary(); // 00000000000000000000000011001011
```

### Min and Max Host
```php
$min_host        = $sub->getMinHost();       // 192.168.112.1
$min_host_quads  = $sub->getMinHostQuads();  // [192, 168, 112, 1]
$min_host_hex    = $sub->getMinHostHex();    // C0A87001
$min_host_binary = $sub->getMinHostBinary(); // 11000000101010000111000000000001

$max_host        = $sub->getMaxHost();       // 192.168.113.254
$max_host_quads  = $sub->getMaxHostQuads();  // [192, 168, 113, 254]
$max_host_hex    = $sub->getMaxHostHex();    // C0A871FE
$max_host_binary = $sub->getMaxHostBinary(); // 11000000101010000111000111111110
```

### Reports

#### Printed Report
```php
$sub->printSubnetReport();
/*
192.168.112.203/23           Quads      Hex                           Binary
------------------ --------------- -------- --------------------------------
IP Address:        192.168.112.203 C0A870CB 11000000101010000111000011001011
Subnet Mask:         255.255.254.0 FFFFFE00 11111111111111111111111000000000
Network Portion:     192.168.112.0 C0A87000 11000000101010000111000000000000
Host Portion:            0.0.0.203 000000CB 00000000000000000000000011001011

Number of IP Addresses:      512
Number of Addressable Hosts: 510
IP Address Range:            192.168.112.0 - 192.168.113.255
Broadcast Address:           192.168.113.255
Min Host:                    192.168.112.1
Max Host:                    192.168.113.254
*/
```

#### Array Report
```php
$sub->getSubnetArrayReport();
/*
Array
(
	[ip_address_with_network_size] => 192.168.112.203/23
	[ip_address] => Array
		(
			[quads] => 192.168.112.203
			[hex] => C0A870CB
			[binary] => 11000000101010000111000011001011
		)

	[subnet_mask] => Array
		(
			[quads] => 255.255.254.0
			[hex] => FFFFFE00
			[binary] => 11111111111111111111111000000000
		)

	[network_portion] => Array
		(
			[quads] => 192.168.112.0
			[hex] => C0A87000
			[binary] => 11000000101010000111000000000000
		)

	[host_portion] => Array
		(
			[quads] => 0.0.0.203
			[hex] => 000000CB
			[binary] => 00000000000000000000000011001011
		)

	[network_size] => 23
	[number_of_ip_addresses] => 512
	[number_of_addressable_hosts] => 510
	[ip_address_range] => Array
		(
			[0] => 192.168.112.0
			[1] => 192.168.113.255
		)

	[broadcast_address] => 192.168.113.255
	[min_host] => 192.168.112.1
	[max_host] => 192.168.113.254
)
*/
```

#### JSON Report
```php
$sub->getJSONReport();
/*
{
	"ip_address_with_network_size": "192.168.112.203/23",
	"ip_address": {
		"quads": "192.168.112.203",
		"hex": "C0A870CB",
		"binary": "11000000101010000111000011001011"
	},
	"subnet_mask": {
		"quads": "255.255.254.0",
		"hex": "FFFFFE00",
		"binary": "11111111111111111111111000000000"
	},
	"network_portion": {
		"quads": "192.168.112.0",
		"hex": "C0A87000",
		"binary": "11000000101010000111000000000000"
	},
	"host_portion": {
		"quads": "0.0.0.203",
		"hex": "000000CB",
		"binary": "00000000000000000000000011001011"
	},
	"network_size": 23,
	"number_of_ip_addresses": 512,
	"number_of_addressable_hosts": 510,
	"ip_address_range": [
		"192.168.112.0",
		"192.168.113.255"
	],
	"broadcast_address": "192.168.113.255",
	"min_host": "192.168.112.1",
	"max_host": "192.168.113.254"
}
*/
```

#### String Report
```php
$string_report = $sub->getPrintableReport();
/*
192.168.112.203/23           Quads      Hex                           Binary
------------------ --------------- -------- --------------------------------
IP Address:        192.168.112.203 C0A870CB 11000000101010000111000011001011
Subnet Mask:         255.255.254.0 FFFFFE00 11111111111111111111111000000000
Network Portion:     192.168.112.0 C0A87000 11000000101010000111000000000000
Host Portion:            0.0.0.203 000000CB 00000000000000000000000011001011

Number of IP Addresses:      512
Number of Addressable Hosts: 510
IP Address Range:            192.168.112.0 - 192.168.113.255
Broadcast Address:           192.168.113.255
Min Host:                    192.168.112.1
Max Host:                    192.168.113.254
*/

// Printing the Subnet object will print the printable report.
print($sub);
/*
192.168.112.203/23           Quads      Hex                           Binary
------------------ --------------- -------- --------------------------------
IP Address:        192.168.112.203 C0A870CB 11000000101010000111000011001011
Subnet Mask:         255.255.254.0 FFFFFE00 11111111111111111111111000000000
Network Portion:     192.168.112.0 C0A87000 11000000101010000111000000000000
Host Portion:            0.0.0.203 000000CB 00000000000000000000000011001011

Number of IP Addresses:      512
Number of Addressable Hosts: 510
IP Address Range:            192.168.112.0 - 192.168.113.255
Broadcast Address:           192.168.113.255
Min Host:                    192.168.112.1
Max Host:                    192.168.113.254
*/
```

Unit Tests
----------

```bash
$ cd tests
$ phpunit
```

[![Coverage Status](https://coveralls.io/repos/github/markrogoyski/ipv4-subnet-calculator-php/badge.svg?branch=master)](https://coveralls.io/github/markrogoyski/ipv4-subnet-calculator-php?branch=master)

Standards
---------

IPv4 Subnet Calculator (PHP) conforms to the following standards:

 * PSR-1 - Basic coding standard (http://www.php-fig.org/psr/psr-1/)
 * PSR-2 - Coding style guide (http://www.php-fig.org/psr/psr-2/)
 * PSR-4 - Autoloader (http://www.php-fig.org/psr/psr-4/)


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
- [Mark Rogoyski](https://github.com/markrogoyski)

