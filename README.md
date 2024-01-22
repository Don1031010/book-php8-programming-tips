# book-php8-programming-tips


## Using constructor property promotion

This new feature combines property declarations and argument lists in the `__construct()` method signature, as well as assigning defaults.

* You need to define a visibility level
* You do not have to explicitly declare the properties in advance
* You do not need to make assignments in the body of the `__construct()` method

```php
declare(strict_types=1);

class Test {
    public function __construct(
        public int $id,
        public int $token = 0,
        public string $name = '')
    { }
}

$test = new Test(999);

var_dump($test);
```
