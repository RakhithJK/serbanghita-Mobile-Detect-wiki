For basic usage of the `Mobile_Detect` class, run the example files https://github.com/serbanghita/Mobile-Detect/tree/master/examples

<table>
<tr><td><code><a href="https://github.com/serbanghita/Mobile-Detect/blob/master/examples/demo.php">demo.php</a></code></td><td>Point your browser or mobile device and check the results.</td></tr>
<tr><td><code><a href="https://github.com/serbanghita/Mobile-Detect/blob/master/examples/session_example.php">session_example.php</a></code></td><td>Simple example of switching between <code>mobile</code> and <code>classic</code> views between pages.</td></tr>
<tr><td><a href="http://www.youtube.com/watch?v=Tx9ozUVGn5U">Video [ES]</a></td><td>Mini tutorial in Spanish by Alejandro Palop.</td></tr>
</table>

```php 
<?php
// These lines are mandatory.
require_once 'Mobile_Detect.php';
$detect = new Mobile_Detect;
```

```php
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

```php
<?php
// Check for mobile environment.
if ($detect->isMobile()) {
    // Your code here.
}
```

```php
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

```php
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

```php
<?php
// Include and instantiate the class.
require_once 'Mobile_Detect.php';
$detect = new Mobile_Detect;

// Any mobile device (phones or tablets).
if ( $detect->isMobile() ) {

}

// Any tablet device.
if( $detect->isTablet() ){

}

// Exclude tablets.
if( $detect->isMobile() && !$detect->isTablet() ){

}

// Check for a specific platform with the help of the magic methods:
if( $detect->isiOS() ){

}

if( $detect->isAndroidOS() ){

}

// Alternative method is() for checking specific properties.
// WARNING: this method is in BETA, some keyword properties will change in the future.
$detect->is('Chrome')
$detect->is('iOS')
$detect->is('UCBrowser')
$detect->is('Opera')
// [...]

// Batch mode using setUserAgent():
$userAgents = array(
'Mozilla/5.0 (Linux; Android 4.0.4; Desire HD Build/IMM76D) AppleWebKit/535.19 (KHTML, like Gecko) Chrome/18.0.1025.166 Mobile Safari/535.19',
'BlackBerry7100i/4.1.0 Profile/MIDP-2.0 Configuration/CLDC-1.1 VendorID/103',
// [...]
);
foreach($userAgents as $userAgent){

  $detect->setUserAgent($userAgent);
  $isMobile = $detect->isMobile();
  $isTablet = $detect->isTablet();
  // Use the force however you want.

}

// Get the version() of components.
// WARNING: this method is in BETA, some keyword properties will change in the future.
$detect->version('iPad'); // 4.3 (float)
$detect->version('iPhone') // 3.1 (float)
$detect->version('Android'); // 2.1 (float)
$detect->version('Opera Mini'); // 5.0 (float)
// [...]
```