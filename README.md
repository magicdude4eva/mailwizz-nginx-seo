# mailwizz-nginx-seo
___
:beer: **Please support me**: Although all my software is free, it is always appreciated if you can support my efforts on Github with a [contribution via Paypal](https://www.paypal.com/cgi-bin/webscr?cmd=_s-xclick&hosted_button_id=ZRP5WBD8CT8EW) - this allows me to write cool projects like this in my personal time and hopefully help you or your business. 
___

MailWizz nginx example with search-engine friendly URLs - this has been tested with MailWizz 1.3.6.x running on CentOS 7 with nginx/1.6.3

The nginx configuration includes:
- SSL and non SSL config
- Support for tracking domains
- SSL configuration with SSL stapling and tuning
- Gzip compression
- Security configuration
 - Turn off access to hidden files and sensitive context
 - XSS configuration to enforce SAMEORIGIN
 - Limiting buffer overflow issues
 - Restricting request methods
 - CSP in reporting mode (ensure that you register with report-uri.io / or remove)
- Remove logging of favicon.ico and robots.txt
- Custom error pages for nginx (those could be much better)
- Cache control for media images, dynamic data and CSS/JavaScript

Included MySQL configuration and php.ini settings for a MailWizz system running on CentOS7, MySQL 5.7.12, nginx on a 4-core, 10GB single-server.
