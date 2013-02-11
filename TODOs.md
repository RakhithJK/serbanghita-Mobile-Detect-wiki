### v.3.0.0 branch

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

### Validate the class against:

1. <s>http://www.zytrax.com/tech/web/mobile_ids.html</s>
1. http://techpatterns.com/downloads/firefox/useragentswitcher.xml
1. http://brettjankord.com/categorizr/categorizr-results.php
1. http://brettjankord.com/categorizr/desktop.php
1. <s>http://www.russellbeattie.com/blog/media/headers.txt [HTTP headers]</s>
1. <s>https://deviceatlas.com/resources/side-loaded-browser-handling [Alternative HTTP headers for HTTP_USER_AGENT]</s>
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

### Code updates:

1. <s>On demo.php send the results via jsonp to mobiledetect.net db</s>
2. <s>Study `Windows; U;`, `Linux; U;` and `Macintosh; U;` user-agents</s>
3. Study `CodeIgniter`'s user-agent class.