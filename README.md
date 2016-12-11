# mailwizz-nginx-seo
[![](https://img.shields.io/badge/PayPal--ffffff.svg?style=social&logo=data%3Aimage%2Fpng%3Bbase64%2CiVBORw0KGgoAAAANSUhEUgAAABAAAAAQCAYAAAAf8%2F9hAAAABHNCSVQICAgIfAhkiAAAAZZJREFUOI3Fkb1PFFEUxX%2F3zcAMswFCw0KQr1BZSKUQYijMFibGkhj9D4zYYAuU0NtZSIiNzRZGamqD%2BhdoJR%2FGhBCTHZ11Pt%2B1GIiEnY0hFNzkFu%2FmnHPPPQ%2Buu%2BTiYGjy0ZPa5N1t0SI5m6mITeP4%2B%2FGP%2Fbccvto8j3cuCsQTSy%2FCzLkdxqkXpoUXJoUXJrkfFTLMwHiDYLrFz897Z3jT6ckdBwsiYDMo0tNOIGuBqS%2Beh7sdAkU2g%2BkBFGkd%2FrtSgD8Z%2BrBxj68MAGG1A9efRhVsXrKMU7Y4cNyGOwtDU28OtrqdUMetldvzFKxCYSHJ4NsJ%2BnRJGexHba7VJ%2FTff4BaQFBjVcbqIEZ1bESYn4PRUcHx2N952awUkOHZedUcWm14%2FtjqjREHawUEsgx6Ajg5%2Bsi7jWqBwA%2BmIrXlo9YHUVTmEP%2F6hOO1Ofiyy3pjo%2BsvBDX%2FZpSakhz4BqvQDvdYvrXQEXZViI5rPpBEOwR2l16vtN7bd9SN3L1WXj%2BjGSnN38rq%2B7VL8xXQOdDF%2F0KvXn8BlbuY%2FvUAHysAAAAASUVORK5CYII%3D)][paypal]
[paypal]: https://www.paypal.com/cgi-bin/webscr?cmd=_s-xclick&hosted_button_id=ZRP5WBD8CT8EW
___
:beer: **Please support me**: Although all my software is free, it is always appreciated if you can support my efforts on Github with a [contribution via Paypal][paypal] - this allows me to write cool projects like this in my personal time and hopefully help you or your business. 
___

MailWizz nginx example with search-engine friendly URLs - this has been tested with MailWizz 1.3.6.x running on CentOS 7 with nginx/1.6.3

The nginx configuration includes:
- SSL and non SSL config
- PHP FPM
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

## Sessions via PHP-FPM
PHP-FPM by default writes sessions into the /var/lib/php directory and I personally do not like assigning nginx permissions to it (since MailWizz stores session info). I have therefore adjusted PHP-FPM config to have it's own directory.

You will need to run this (or adjust /etc/php-fpm.d/www.conf):
```
mkdir -p /var/lib/nginx/session
mkdir -p /var/lib/nginx/wsdlcache
chown -R nginx:nginx /var/lib/nginx
```

## About SSL-stapling
I use Thawte SSL123 certs and you will notice in the SSL configuration a reference to `ssl_trusted_certificate /etc/pki/tls/certs/combined-certs.crt;`. I suggest you read up about SSL stapling first: https://raymii.org/s/tutorials/OCSP_Stapling_on_nginx.html.

The `ssl_trusted_certificate` needs to be in a specific order, and this worked for me (copy each cert in this sequence into the file):
- The Webserver certificate - this is the cert for the domain and will be included in the SSL cert order email
- The intermediate CA - this will also be included in the SSL cert order email
- The primary root CA - for SSL123 Thawte it can be obtained from here: https://www.thawte.com/roots/thawte_Primary_Root_CA.pem

You can then test the SSL stapling via:
```
openssl s_client -connect yourdomain.com:443 -tls1 -tlsextdebug -status
```
Which will output something like this:
```
----
...
OCSP response:
======================================
OCSP Response Data:
    OCSP Response Status: successful (0x0)
    Response Type: Basic OCSP Response
...
----
```
