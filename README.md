# orderbox.php

PHP library for the ResellerClub and LogicBoxes HTTP API.


## Getting Started

### Download
Download the [latest version of orderbox.php](https://github.com/sandeepshetty/orderbox.php/archives/master):

```shell
$ curl -L http://github.com/sandeepshetty/orderbox.php/tarball/master | tar xvz
$ mv sandeepshetty-orderbox.php-* orderbox.php
```

### Register your IP addresses

Register the IP addresses from where you will be making the HTTP API requests from the `Setting > API page` within your Control Panel.


### Require

```php
<?php

	require 'path/to/orderbox.php/orderbox.php';

?>
```

### Usage

Making API calls:

```php
<?php

	// For regular apps:
	$orderbox = orderbox_api_client($userid, $password);

	try
	{
		// Get account balance
		$response = $orderbox('GET', '/billing/reseller-balance.json', array('reseller-id'=>'22222'));
		echo "Your Account Balance is {$response['sellingcurrencysymbol']} {$response['sellingcurrencybalance']}";


		try
		{
			// Signing up a new customer
			$customer_id = $orderbox(
				'POST',
				'/customers/signup.json',
				array(
					'username'=>'email1@email.com',
					'passwd'=>'password9',
					'name'=>'name',
					'company'=>'company',
					'address-line-1'=>'address-line-1',
					'city'=>'city',
					'state'=>'state',
					'country'=>'US',
					'zipcode'=>'0000',
					'phone-cc'=>'0',
					'phone'=>'000000',
					'lang-pref'=>'en'),
				$response_headers); // All requests accept an optional fourth parameter, that is populated with the response headers.

		}
		catch (OrderboxApiException $e)
		{
			// If you're here, either HTTP status code was >= 400 or response contained the key 'errors'
		}

	}
	catch (OrderboxApiException $e)
	{
		/* $e->getInfo() will return an array with keys:
			* method
			* path
			* params (third parameter passed to $orderbox)
			* response_headers
			* response
			* shops_myorderbox_domain
			* shops_token
		*/
	}
	catch (OrderboxCurlException $e)
	{
		// $e->getMessage() returns value of curl_errno() and $e->getCode() returns value of curl_ error()
	}
?>
```
