# Constants

## Question

Const access from outside a class contained in string

With this code:

```php
class Constants{
   const ONE = 1;
   const TWO = 2;
   const THREE = 3;
}

$input = "ONE";

echo Constants::$input;
```

I want to access to the constants inside the class having the name in a variable.

I'ts that posible.

## Answer

Constant function will return value of a constant by its name:

```php
class Contants{
   const ONE = 1;
   const TWO = 2;
   const THREE = 3;
}

$input = "ONE";

echo constant(Contants::class . '::' . $input);
```
