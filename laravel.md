# Laravel

```
# First, check whether the extension already exists
› php --ri mongodb
# Extension 'mongodb' not present.

# Let's install the extension
sudo pecl install mongodb

# Wait a few minutes...
# Failed!

# /opt/homebrew/Cellar/php/8.1.8/include/php/ext/pcre/php_pcre.h:23:10:
# fatal error: 'pcre2.h' file not found
#include "pcre2.h"
#         ^~~~~~~~~
# 1 error generated.
# make: *** [src/MongoDB/Cursor.lo] Error 1
# ERROR: `make' failed

# Let's copy the missing file
cp /opt/homebrew/Cellar/pcre2/10.40/include/pcre2.h /opt/homebrew/Cellar/php/8.1.8/include/php/ext/pcre/pcre2.h

# Let's try installing again
sudo pecl install mongodb
# Build process completed successfully
# Installing '/opt/homebrew/Cellar/php/8.1.8/pecl/20210902/mongodb.so'
# install ok: channel://pecl.php.net/mongodb-1.13.0
# Extension mongodb enabled in php.ini

# Success!
# Let's look for MongoDB now

› pecl list | grep mongo
# mongodb 1.13.0  stable

# Let's check whether the extension exists again
› php --ri mongodb
# mongodb
#
# MongoDB support => enabled
# MongoDB extension version => 1.13.0
# MongoDB extension stability => stable
# libbson bundled version => 1.21.1
# libmongoc bundled version => 1.21.1
# libmongoc SSL => enabled
# libmongoc SSL library => Secure Transport
# libmongoc crypto => enabled
# libmongoc crypto library => Common Crypto
# libmongoc crypto system profile => disabled
# libmongoc SASL => enabled
# libmongoc ICU => disabled
# libmongoc compression => enabled
# libmongoc compression snappy => disabled
# libmongoc compression zlib => enabled
# libmongoc compression zstd => enabled
# libmongocrypt bundled version => 1.3.2
# libmongocrypt crypto => enabled
# libmongocrypt crypto library => Common Crypto
# 
# Directive => Local Value => Master Value
# mongodb.debug => no value => no value
# mongodb.mock_service_id => Off => Off

# Get your installed mongodb.so path
› pecl list-files mongodb | grep mongodb.so
# src  /opt/homebrew/Cellar/php/8.1.8/pecl/20210902/mongodb.so

# Find your php.ini path
› php --ini
# Configuration File (php.ini) Path: /opt/homebrew/etc/php/8.1
# Loaded Configuration File:         /opt/homebrew/etc/php/8.1/php.ini
# Scan for additional .ini files in: /opt/homebrew/etc/php/8.1/conf.d
# Additional .ini files parsed:      /opt/homebrew/etc/php/8.1/conf.d/error_log.ini,
# /opt/homebrew/etc/php/8.1/conf.d/ext-mongodb.ini,
# /opt/homebrew/etc/php/8.1/conf.d/ext-opcache.ini,
# /opt/homebrew/etc/php/8.1/conf.d/php-memory-limits.ini

# Open with your favorite editor
› code /opt/homebrew/etc/php/8.1/php.ini

# Comment out the line that says
# extension="mongodb.so"

# Add the path to mongodb.so
extension="/opt/homebrew/Cellar/php/8.1.8/pecl/20210902/mongodb.so"

# Optionally, you could create a separate extension configuration file
› touch /opt/homebrew/etc/php/8.1/conf.d/ext-mongodb.ini
› code touch /opt/homebrew/etc/php/8.1/conf.d/ext-mongodb.ini

# Then add the path to mongodb.so in that file
extension="/opt/homebrew/Cellar/php/8.1.8/pecl/20210902/mongodb.so"

# Verify your configuration
› php -i | grep mongodb
# Additional .ini files parsed => /opt/homebrew/etc/php/8.1/conf.d/ext-mongodb.ini,
# mongodb
# mongodb.debug => no value => no value
# mongodb.mock_service_id => Off => Off
```

{% embed url="https://nono.ma/install-mongodb-php-extension-macos" %}
