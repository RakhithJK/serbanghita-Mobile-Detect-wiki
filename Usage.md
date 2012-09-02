### Basic examples
```php
// These lines are mandatory.
include 'Mobile_Detect.php';
$detect = new Mobile_Detect;
```

```php
// 1. Check for mobile environment.
if ($detect->isMobile()) {
    // Your code here.
}
```

```php
// 2. Check for tablet device.
if($detect->isTablet()){
    // Your code here.
}
```

```php
// 3. Check for any mobile device, excluding tablets.
if ($detect->isMobile() && !$detect->isTablet()) {
    // Your code here.
}
```

```php
// 4. Keep the value in $_SESSION for later use
//    and for optimizing the speed of the code.
if(!$_SESSION['isMobile']){
    $_SESSION['isMobile'] = $detect->isMobile();
}
```

```php
// 5. Redirect the user to your mobile version of the site.
if($detect->isMobile()){
    header('http://m.yoursite.com', true, 301);
}
```