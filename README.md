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

```php
namespace Php7\Image;

class SingleChar {
    public $text     = '';
    public $fontFile = '';
    public $width    = 100;
    public $height   = 100;
    public $size     = 0;
    public $angle    = 0.00;
    public $textX    = 0;
    public $textY    = 0;

    const DEFAULT_TX_X = 25;
    const DEFAULT_TX_Y = 75;
    const DEFAULT_TX_SIZE  = 60;
    const DEFAULT_TX_ANGLE = 0;

    public function __construct(
        string $text,
        string $fontFile,
        int $width  = 100,
        int $height = 100,
        int $size   = self::DEFAULT_TX_SIZE,
        float $angle = self::DEFAULT_TX_ANGLE,
        int $textX  = self::DEFAULT_TX_X,
        int $textY  = self::DEFAULT_TX_Y)
    {
        $this->text     = $text;
        $this->fontFile = $fontFile;
        $this->width    = $width;  
        $this->height   = $height;
        $this->size     = $size;
        $this->angle    = $angle;
        $this->textX    = $textX;
        $this->textY    = $textY;
    
        // other code not shown
    }
}
```

