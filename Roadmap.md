### What we developed so far.

```
<?php
// Basic detection.
$detect->isMobile();
$detect->isTablet();

// Magic methods.
$detect->isIphone();

// Alternative to magic methods.
$detect->is('iphone');

// Find the version of component.
$detect->version('Android');

// Additional match method.
$detect->match('regex.*here');

// Browser grade method.
$detect->mobileGrade();

// Batch methods.
$detect->setUserAgent($userAgent);
$detect->setHttpHeaders($httpHeaders);
```

### What we need.
> Whether the user is coming from mobile, tablet or computer there is an acute need to categorize them. Every information about the device, operating system and browser can help the developer serving the right content.

```
<?php
// General informations about the device.
$detect->what();
/*
@return array(

 'browser' => 'Chrome',
 'browserVer' => 11.2,
 'deviceBrand' => 'HTC',
 'deviceVer' => 5.0,
 'os' => 'Windows Phone OS',
 'osVer' => 7.5,
 'deviceType' => 'mobile',
 
 // extended
 'WebKit' => 534.30,
 'Build' => 4.1.A.0.562,

)
*/

// Possibility to pick component.
$detect->what('browser');
```

### Internal updates and refactoring of code
> We are currently storing `$phoneDevices`, `$tabletDevices`, `$operatingSystems`, `$userAgents`, `$utilities` and `$properties` separately and unify the first 5 variables through `setMobileDetectionRules()` depending of the method the user is using: `isMobile()`, `isTablet()` respectively `is()`. This can be costly and in the same time it doesn't provide flexibility we need for the new `what()` method.

**The `what()` method**

```
<?php

private $what; // Array

private $search = array(

 'mobile' => array(
   'iPhone' => 'regex',
   'HTC' => 'regex',
   'Samsung' => 'regex',
   'Generic' => 'regex'
 ),

 'tablet' => array(
   'iPad' => 'regex',
   'HTC' => 'regex',
   'Nexus' => 'regex',
   'Generic' => 'regex'
 ),

 'os' => array(
   'Android'    => array('match' => 'regex', 'label' => 'Android', 'ver' => ''),
   'BlackBerry' => array('match' => 'regex', 'label' => 'BlackBerry', 'ver' => ''),
 ),

 'browser' => array(
   'Chrome' => array('match' => 'regex', 'label' => 'Chrome', 'ver' => ''),
   'Opera'  => array('match' => 'regex', 'label' => 'Opera', 'ver' => ''),
 ),

);

public function what($prop = null){
   
}
```