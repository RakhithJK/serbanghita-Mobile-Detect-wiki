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
$detect->device();
```