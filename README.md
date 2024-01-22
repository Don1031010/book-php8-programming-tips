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

```php
public function __construct(
    public string $text,
    public string $fontFile,
    public int    $width    = 100,
    public int    $height   = 100,
    public int    $size     = self::DEFAULT_TX_SIZE,
    public float   $angle    = self::DEFAULT_TX_ANGLE,
    public int    $textX    = self::DEFAULT_TX_X,
    public int    $textY    = self::DEFAULT_TX_Y)
    { // other code not shown }
```

## Working with attributes

Another significant addition to PHP 8 is the addition of a brand-new class and language construct known as attributes. Simply put, attributes are replacements for traditional PHP comment blocks that follow a prescribed syntax. When the PHP code is compiled, these attributes are converted internally into `Attribute` class instances. The Attribute class addresses a potentially significant performance issue pertaining to an abuse of the traditional PHP comment block (DocBlock) to provide meta-instructions.

Instead of using DocBlocks with annotations, developers can define the equivalent in the form of attributes. An advantage of using attributes rather than DocBlocks is that they are a formal part of the language and are thus tokenized and compiled along with the rest of your code.

Here is the Attribute class definition:

```php
class Attribute {
    public const int TARGET_CLASS = 1;
    public const int TARGET_FUNCTION = (1 << 1);
    public const int TARGET_METHOD = (1 << 2);
    public const int TARGET_PROPERTY = (1 << 3);
    public const int TARGET_CLASS_CONSTANT = (1 << 4);
    public const int TARGET_PARAMETER = (1 << 5);
    public const int TARGET_ALL = ((1 << 6) - 1);
    public function __construct(
        int $flags = self::TARGET_ALL) {}
}
```

`SingleCar` class comparison:

```php
namespace Php7\Image;

/**
* Creates a single image, by default black on white
*/

class SingleChar {
    /**
     * Allocates a color resource
     *
     * @param array|int $r,
     * @param int $g
     * @param int $b]
     * @return int $color
     */
    public function colorAlloc()
    { /* code not shown */ }
```

```php
namespace Php8\Image;

#[description('Creates a single image')]
class SingleChar {
    #[SingleChar\colorAlloc\description('Allocates color')]
    #[SingleChar\colorAlloc\param('r','int|array')]
    #[SingleChar\colorAlloc\param('g','int')]
    #[SingleChar\colorAlloc\param('b','int')]
    #[SingleChar\colorAlloc\returns('int')]
    public function colorAlloc() { /* code not shown */ }
```


    /**

     * Allocates a color resource

     *

     * @param array|int $r,

     * @param int $g

     * @param int $b]

     * @return int $color

     */

    public function colorAlloc()

    { /* code not shown */ }

Now, have a look at the same thing using attributes:

// /repo/src/Php8/Image/SingleChar.php

namespace Php8\Image;

#[description('Creates a single image')]

class SingleChar {

    #[SingleChar\colorAlloc\description('Allocates color')]

    #[SingleChar\colorAlloc\param('r','int|array')]

    #[SingleChar\colorAlloc\param('g','int')]

    #[SingleChar\colorAlloc\param('b','int')]

    #[SingleChar\colorAlloc\returns('int')]

    public function colorAlloc() { /* code not shown */ }

As you can see, in addition to providing a more robust compilation and avoiding the hidden dangers mentioned, it's also more efficient in terms of space usage.

**TIP**

What goes inside the square brackets does have some restrictions; for example, although `#[returns('int')]` is allowed, this is not: `#[return('int')`. The reason for this is because return is a keyword.

Another example has to do with union types (explained in the Exploring new data types section). You can use `#[param('int|array test')]` in an attribute, but this is not allowed: `#[int|array('test')]`. Another peculiarity is that class-level attributes must be placed immediately before the class keyword and after any use statements.



