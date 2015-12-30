The order inside a browser family is **important**.

Eg. When checking for `isChrome()` we will first check for `mobile` and at last the `desktop` version.
The last `desktop` regex is a fallback.

# IE family

* http://msdn.microsoft.com/en-us/library/ms537503(v=vs.85).aspx

# Chrome family

* https://developers.google.com/chrome/mobile/docs/user-agent

# NetFront family

* @todo (Serban) Should we split this into NetFront and NetFrontLife? Mobile/Desktop
* @docs http://en.wikipedia.org/wiki/NetFront

# Generic

* Dolfin family has only mobile browsers.
* @docs https://en.wikipedia.org/wiki/Dolphin_Browser
* @docs http://www.ucweb.com/
* @todo If this grows, then split into multiple generic browsers as they become important.

# Apple family

* @docs http://developer.apple.com/library/safari/#documentation/AppleApplications/Reference/SafariWebContent/OptimizingforSafarioniPhone/OptimizingforSafarioniPhone.html#//apple_ref/doc/uid/TP40006517-SW3
* @note Safari 7534.48.3 is actually Version 5.1.
* @note On BlackBerry the Version is overwritten by the OS.