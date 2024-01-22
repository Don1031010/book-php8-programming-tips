# book-php8-programming-tips

## Using constructor property promotion

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
