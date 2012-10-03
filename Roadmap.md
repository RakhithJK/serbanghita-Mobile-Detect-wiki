### What we have now
***

```
<?php
// Basic detection.
$detect->isMobile();
$detect->isTablet();

// Magic methods.
$detect->isIphone();

// Alternative to magic methods.
$detect->is('iphone');

// Additional match method.
$detect->match('regex.*here');

// Browser grade method.
$detect->mobileGrade();

// Batch methods.
$detect->setUserAgent($userAgent);
$detect->setHttpHeaders($httpHeaders);
```

### What we need
***

```
<?php
// General informations about the device.
$detect->device();