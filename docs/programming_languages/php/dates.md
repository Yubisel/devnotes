# Dates

## Compare two 24 hour time

Use the built-in function [strtotime()](https://secure.php.net/manual/en/function.strtotime.php):

```php
$time="00:05:00"; //5 minutes
if(strtotime($time) <= strtotime('00:03:00')) {
    //do some work
} else {
    //do something
}
```
