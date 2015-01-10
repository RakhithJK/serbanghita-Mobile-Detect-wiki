> "Work like a slave, command like a king, create like a God."

Whether the user is coming from mobile, tablet or computer there is an acute need to categorize them. Every information about the _device_, _operating system_ and _browser_ can help the developer serving the right content.

We are currently storing `$phoneDevices`, `$tabletDevices`, `$operatingSystems`, `$browsers`, `$utilities` and `$properties` separately and unify the first 5 variables through `setMobileDetectionRules()` depending of the method the user is using: `isMobile()`, `isTablet()` respectively `is()`. This can be costly and in the same time it doesn't provide flexibility we need for the new `what()` method.

### API

The `Mobile_Detect::$items` array will contain the following string keys: `phoneDevices`, `tabletDevices`, `operatingSystems`, and `browsers`. Each key will be an array where the keys will be strings that pertain to the vendor/class and the value will be an associative array with the following possible keys:

* `vendor`: The vendor name.
* `matchType`: If not specified, it defaults to `"regex"` since most checks are regular expressions. The other possible value is `"strpos"` which will use the PHP `strpos` method and will return true as long as the result is not false. Also, `"stripos"` will be the case-insensitive version of `"strpos"`. Note that `strpos` (and `stripos`) can be falsy with `0` but this will not be acceptable as `strpos(...) === false` (see [[docs|http://php.net/manual/en/function.strpos.php]]).
* `match`: The match, either a regular expression OR a string which is passed to `strpos` depending on `matchType` key.
* `model`: An array with at least 1 item indicating some matches that can be done to identify how to extract the model information. The presence of `[MODEL]` in the string will be substituted by the model regular expression match. `[MODEL]` regex might vary from vendor to vendor, another key like `modelMatch` might be needed.
* `property`: An array of strings that matches to `$itemProperties` array (as keys) to show which properties are available for this specific device type.

The following key was up for discussion but will not be included for reasons of not wanting a homogenous array at the moment.

* ~~`deviceType`: the type of device, either `phone`, `os`, `tablet`, or `browser`. However this may not be feasible based on the following notes.~~


##### Variables

```php
<?php
const VERSION = '3.0.0';
const GENERIC_VERSION_REGEX = '([\w._\+]+)';
const GENERIC_MODEL_REGEX = '([\w_]+)';
protected $userAgent = null;
protected $httpHeaders = array();
protected $cacheAdapter;//an object
protected $cache = array();
protected static $items = array(
 'phoneDevices' => array(
    'HTC'        => array(
      'vendor'    => 'HTC',
      'matchType' => 'regex',
      'match'     => 'HTC|HTC.*(Sensation|Evo|Vision|Explorer|6800|8100|8900|A7272|S510e|C110e|Legend|Desire|T8282)|APX515CKT|Qtek9090|APA9292KT|HD_mini|Sensation.*Z710e|PG86100|Z715e|Desire.*(A8181|HD)|ADR6200|ADR6425|001HT|Inspire 4G',
      'model'     => array('HTC.[MODEL]', 'HTC; [MODEL]', 'HTC/[MODEL]', ' [MODEL] Build'),
      'property'  => array('Build', 'Android')
      ),
    'iPhone' => array(
        'vendor'    => 'Apple',
        'matchType' => 'stripos',
        'match'     => 'iphone',
        'model'     => array('Version/[MODEL]'),
        'property'  => array('AppleWebkit')
    )
      // [...]  
 ),
 'tabletDevices' => array(),
 'operatingSystems' => array(),
 'browsers' => array(),
 'utilities' => array() // Move this?
);

protected static $itemsProperties = array(
 // Build specific version.
 'Mobile' => 'Mobile/[REGEX]',
 'Build' => 'Build/[REGEX]',
 // Device specific version.
 'iPad' => 'iPad.*CPU[a-z ]+[REGEX]',
 // Browser version.
 'Chrome' => array('Chrome/[REGEX]', 'CriOS/[REGEX]', 'CrMo/[REGEX]'),
 // Engine version.
 // OS version.
 // [...]
);
```

`$items[ $itemKey ]` should point to what `$itemsProperties` are relevant.

##### Methods

```php
<?php
// Constructor.
public function __construct( $httpHeaders, $userAgent, $cacheAdapter ) {
 $this->setHttpHeaders($httpHeaders); 
 $this->setUserAgent($userAgent);
 //the call below makes sure stuff like ->set(...) and ->get(...) exist on the object (or whatever we determine is right based on the discussion in #140)
 $this->checkCacheAdapter($cacheAdapter);
}
// Public utility methods.
public function getVersion();
public function setUserAgent( $userAgent );
public function getUserAgent();
public function getItems( $itemKey = null );
public function getItemsProperties();
// Internal utility methods
private function _prepareVersion( $version );
private function _match( $regex );
private function _matchDetectionRulesAgainstUA();
private function _matchUAAgainstKey( $key );
// Checking methods.
public function checkHttpHeadersForMobile();
public function __call($name, $arguments); // private?
public function isMobile();
public function isTablet();
public function is( $itemKey );

public function device() {
  //see this class definition further below
  return new Mobile_Detect_Device_Info(...);
}

public function deviceAsArray() {
 return array(
    'deviceType' => 'mobile', // tablet, classic
    'deviceVendor' => 'Samsung',
    'deviceModel' => 'GT-B2700',
    'os' => 'Android',
    'osVersion' => '4.1.1',
    'browser' => 'Chrome',
    'browserVer' => '18.0.1025.308',
 )
}

//IDEA
//What if device() method returns as object like so:

class Mobile_Detect_Device_Info
{
  protected $deviceType;
  public function getDeviceType(){return $this->deviceType;}

  protected $deviceVendor;
  public function getDeviceVendor(){return $this->deviceVendor;}

  protected $deviceModel;
  public function getDeviceModel(){return $this->deviceModel;}

  protected $os;
  public function getOs(){return $this->os;}

  //and so on, for the rest...
}

```

##### Examples

```
<?php
// General informations about the device.
$deviceInfoArray = $detect->deviceAsArray();
$deviceInfoObj   = $detect->device();

// Possibility of picking a component.
$browserInfo  = $deviceInfoArray['browser'];
$browserInfo2 = $detect->device()->getBrowser();
```

***


### TODO

1. Add regexes from `2.x.x`, from each device brand, to the `$items` array.
1. Add `version` key, from each device brand, to the `$items` array.
1. Remove `manual_tests` folder.
1. Remove `tests` folder.
1. Create `tests` folder for automated tests. Create subfolder `inc`. Add subfolder `inc` to the ignored test list.
1. Create `WhatTests.php` based on information from `mobilePerVendor_useragents.inc.php`. At least one User-Agent per vendor. Create `what` key and array value with the correct data to validate against.
1. Make `$detect->what()` method work against the above automated tests.
1. Create `version()` method.
1. Create `VersionTests.php` based on information from `mobilePerVendor_useragents.inc.php`. At least one test per vendor. Create `version` key and array value with the correct data to validate against.
1. Recreate `isMobile()` method for backward compatibility.
1. Recreate the pseudo-methods from `$items['mobile']` and `$items['tablet']` arrays.
1. Write `MobileTests.php` based on information from `mobilePerVendor_useragents.inc.php`. All the items should be able to validate against `isMobile()` method.
1. The same with `isTablet()` (use the same `MobileTests.php` file).
1. Recreate `isTablet()` method for backward compatibility.
1. Recreate `is()` method for backward compatibility.
1. Create internal caching mechanism for the `what()` method. Should be able to create, read and destroy the cache.
1. Remove all `@deprecated` methods and/or parameters.
1. When a `$cacheAdapter` is passed during construction, all heavy calls should pipe data through there with the user agent as the key. At the least, we can implement a simple local in-memory cache and maybe some other super-simple adapters like APC or memcached.

### Competition

List of open-source and commercial projects involved in detection of mobile devices or mobile features.

<table>
<tr>
<th colspan="3">Open source / Free</th>
</tr>
<tr>
<th>Name</th>
<th>Description</th>
<th>Notes</th>
</tr>
<tr>
<td><a href="https://github.com/davidwood/connect-categorizr">connect-categorizr</a></td>
<td>Connect middleware that provides device detection, based on Brett Jankord's Categorizr.</td>
<td>Node.js dependency</td>
</tr>
<tr>
<td><a href="https://github.com/bjankord/Categorizr">Categorizr</a></td>
<td>A modern device detection script.</td>
<td>PHP based. `isMobile()`, `isTablet()`, `isDesktop()`, `isTV()`</td>
</tr>
<tr>
<td><a href="https://github.com/dmolsen/Detector">Detector</a></td>
<td>Detector is a simple, PHP- and JavaScript-based browser- and feature-detection library that can adapt to new devices & browsers on its own without the need to pull from a central database of browser information.</td>
<td><font color="#9999cc">PHP</font> based. Uses `ua-parser-php` and `Modernizr`. Stores features via `Modernizr` in cookie and session. `$ua->isMobile`, `checkSpider()`</td>
</tr>
<tr>
<td><a href="https://github.com/jamesgpearce/modernizr-server">Modernizr server</a></td>
<td></td>
<td></td>
</tr>
<tr>
<td><a href="https://github.com/tobie/ua-parser">ua-parser</a></td>
<td></td>
<td></td>
</tr>
<tr>
<td><a href="https://github.com/yiibu/profile">profile</a></td>
<td></td>
<td></td>
</tr>
<tr>
<td><a href="https://github.com/raducotescu/browsermap">browsermap</a></td>
<td></td>
<td></td>
</tr>
</table>

### Validate the class against:

1. <s>http://www.zytrax.com/tech/web/mobile_ids.html</s>
1. http://techpatterns.com/downloads/firefox/useragentswitcher.xml
1. http://brettjankord.com/categorizr/categorizr-results.php
1. http://brettjankord.com/categorizr/desktop.php
1. <s>http://www.russellbeattie.com/blog/media/headers.txt [HTTP headers]</s>
1. <s>https://deviceatlas.com/resources/side-loaded-browser-handling [Alternative HTTP headers for HTTP_USER_AGENT]</s>
1. http://learnthemobileweb.com/2009/07/mobile-mime-types/ [MIME types]
1. http://www.quirksmode.org/mobile/browsers.html (Mobile Browsers List and comparisons)
1. https://github.com/davidwood/connect-categorizr/blob/master/test/tablet.json (Tablets user-agents)

### Add to projects list:

1. https://github.com/axelwehner/c5-MobileDetectHelper
1. <s>http://www.zenphoto.org/documentation/core/_zp-extensions---mobileTheme---Mobile_Detect.php.html ; http://www.zenphoto.org/news/mobiletheme</s>
1. <s>http://www.reviewsforjoomla.com</s>
1. https://code.todoyu.com/ticket/236
1. <s>http://www.concrete5.org/ (https://github.com/concrete5/concrete5/blob/master/web/concrete/libraries/3rdparty/mobile_detect.php)</s>
1. http://themeforest.net/item/my-mobile-page-v3-wordpress-theme/1328741
1. http://wordpress.org/extend/themes/swift-basic
1. https://github.com/otto-torino/gino
1. http://shoppica2.com/
1. https://github.com/spotweb/spotweb
1. https://github.com/IFBook/CommentPressPlugin

### Media sources

http://www.phonearena.com/news
http://www.newcellphonesblog.com/
http://www.phonedog.com/posts/
http://www.zdnet.com/blog/cell-phones/
http://www.mobileburn.com/

### Code updates:

1. <s>On demo.php send the results via jsonp to mobiledetect.net db</s>
2. <s>Study `Windows; U;`, `Linux; U;` and `Macintosh; U;` user-agents</s>
3. Study `CodeIgniter`'s user-agent class.

### 3.0 release todos

This is a list of items remaining prior to our first alpha release for 3.0.

 - [ ] Implement version getters for the Device class
 - [ ] Document how to implement caching using the callback implementation.
 - [ ] Complete remaining static "easy" methods.
 - [ ] Document an easy start guide for 3.0.
 - [ ] Add general documentation.