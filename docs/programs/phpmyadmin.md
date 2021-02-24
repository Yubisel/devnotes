# phpmyadmin

## Config

### Enable host selection

In the root directory of phpmyadmin open ```config.inc.php``` file, if this file don't appear create it and inside define the next variable

```php
$cfg['AllowArbitraryServer'] = true;
```
