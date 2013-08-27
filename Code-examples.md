```
<?php
// These lines are mandatory.
require_once 'Mobile_Detect.php';
$detect = new Mobile_Detect;
```

```
<?php
// Basic detection.
$detect->isMobile();
$detect->isTablet();

// Magic methods.
$detect->isIphone();
$detect->isSamsung();
// [...]

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

```
<?php
// Check for mobile environment.
if ($detect->isMobile()) {
    // Your code here.
}
```

```
<?php
// Check for tablet device.
if($detect->isTablet()){
    // Your code here.
}
```

```
<?php
// Check for any mobile device, excluding tablets.
if ($detect->isMobile() && !$detect->isTablet()) {
    // Your code here.
}
```

```
<?php
//  Keep the value in $_SESSION for later use
//    and for optimizing the speed of the code.
if(!$_SESSION['isMobile']){
    $_SESSION['isMobile'] = $detect->isMobile();
}
```

```
<?php
// Redirect the user to your mobile version of the site.
if($detect->isMobile()){
    header('http://m.yoursite.com', true, 301);
}
```