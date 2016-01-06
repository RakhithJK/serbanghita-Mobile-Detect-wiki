## Mobile Detect business user stories

## Mobile Detect + GeoIP

The visitor is comming to http://test.com/script/ 
Detect the visitor device:

   * If the visitor is comming from mobile/ipad device and from the country listed bellow: "US" "CA" "GB" "NZ" "AU" "DE" then redirect the visitor to link:http://aaa.com
   * If the visitor is comming from mobile/ipad device and from a country which one is not listed on point (1) then redirect the visitor to link: http://bbb.com
   * If the visitor is comming from a Desktop Device and from the country listed bellow: "US" "CA" "GB" "NZ" "AU" "DE" then redirect the visitor to link:http://ccc.com
   * If the visitor is comming from Desktop device and from a country which one is not listed on point (3) then redirect the visitor to link: http://ddd.com

## Mobile Detect as an official Wordpress module

1. Take over the Browser Detection Booleans https://codex.wordpress.org/Global_Variables and maybe provide new ones.
2. Make module vendors be aware of our official module, use it as a dependency.
3. Support cache plugins.
