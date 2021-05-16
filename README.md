# Instagram PHP Scraper (Using any Proxy API)
This library is based on the [postaddictme](https://github.com/postaddictme/instagram-php-scrape/) and [restyler](https://github.com/restyler/instagram-php-scraper/) libary versions. The difference is that my version can be associated with any proxy API. I've decided to build this version, because the restyler version was giving this message: "You have exceeded the MONTHLY quota for Requests on your current plan, BASIC." and it can only work with one proxy API and the postaddictme version do not allow us to use a proxy API directly.

## Dependencies
- PHP >= 7.2
- [PSR-16](http://www.php-fig.org/psr/psr-16/)
- [PSR-18](http://www.php-fig.org/psr/psr-18/)

Only these methods can use proxy API because they do not require authentication:
- getAccount()
- getAccountById()
- getMedias()
- getMediasByTag()
- getMediaByUrl()
- getMediaByCode()

## Exemple using restyler's Rapid Api ([Click here to subscribe on his Proxy API](https://rapidapi.com/restyler/api/instagram40/))
```php
$instagram = new \InstagramScraper\Instagram();
$instagram->getProxyApiUrl('https://instagram40.p.rapidapi.com/proxy?url={instagram_url}'); // do not remove {instagram_url}, it is later replaced by the instagram endpoints
$instagram->setHeaders(['x-rapidapi-key'=>'YOUR-RAPID-API-KEY']); // this is where you add your api key
$nonPrivateAccountMedias = $instagram->getMedias('kevin');
echo $nonPrivateAccountMedias[0]->getLink();
```

## Exemple using scraperapi ([Click here to subscribe on his Proxy API](https://www.scraperapi.com/))
```php
$instagram = new \InstagramScraper\Instagram();
$instagram->getProxyApiUrl('http://api.scraperapi.com?api_key=YOUR-SCRAPPER-API-KEY&url={instagram_url}'); // do not remove {instagram_url}, it is later replaced by the instagram endpoints
$nonPrivateAccountMedias = $instagram->getMedias('kevin');
echo $nonPrivateAccountMedias[0]->getLink();
```

Other methods that can be used in this libary are:
- getProxyApiUrl('string') : used to check and check if your Proxy API url is set
- clearProxyApiUrl() : to clear the previous url set if you don't want to make request with proxy API anymore
- setHeaders($array): is used to set headers, in case you want add them in your proxy API Request

## From here, all the rest is just the same as it is on postaddictme library 

## Code Example
```php
$instagram = \InstagramScraper\Instagram::withCredentials(new \GuzzleHttp\Client(), 'username', 'password');
$instagram->login();
$account = $instagram->getAccountById(3);
echo $account->getUsername();
```

Some methods do not require authentication: 
```php
$instagram = new \InstagramScraper\Instagram(new \GuzzleHttp\Client());
$nonPrivateAccountMedias = $instagram->getMedias('kevin');
echo $nonPrivateAccountMedias[0]->getLink();
```

If you use authentication it is recommended to cache the user session. In this case you don't need to run the `$instagram->login()` method every time your program runs:

```php
use Phpfastcache\Helper\Psr16Adapter;

$instagram = \InstagramScraper\Instagram::withCredentials(new \GuzzleHttp\Client(), 'username', 'password', new Psr16Adapter('Files'));
$instagram->login(); // will use cached session if you want to force login $instagram->login(true)
$instagram->saveSession();  //DO NOT forget this in order to save the session, otherwise have no sense
$account = $instagram->getAccountById(3);
echo $account->getUsername();
```

Using proxy for requests:

```php
// https://docs.guzzlephp.org/en/stable/request-options.html#proxy
$instagram = new \InstagramScraper\Instagram(new \GuzzleHttp\Client(['proxy' => 'tcp://localhost:8125']));
// Request with proxy
$account = $instagram->getAccount('kevin');
\InstagramScraper\Instagram::setHttpClient(new \GuzzleHttp\Client());
// Request without proxy
$account = $instagram->getAccount('kevin');
```

## Installation

### Using composer

```sh
composer.phar require ravelinodecastro/instagram-php-scraper
```
or 
```sh
composer require ravelinodecastro/instagram-php-scraper
```

### If you don't have composer
You can download it [here](https://getcomposer.org/download/).

## Examples
See examples [here](https://github.com/postaddictme/instagram-php-scraper/tree/master/examples).

## Other
Java library: https://github.com/postaddictme/instagram-java-scraper
