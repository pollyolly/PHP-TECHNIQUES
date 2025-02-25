### PHP-TECHNIQUES
[NAMESPACES](https://www.php.net/manual/en/language.namespaces.php)
```php
file1:
<?php namespace foo;
  class Cat {
    static function says() {echo 'meoow';}  } ?>

file2:
<?php namespace bar;
  class Dog {
    static function says() {echo 'ruff';}  } ?>

file3:
<?php namespace other\animate;
  class Animal {
    static function breathes() {echo 'air';}  } ?>

file4:
<?php namespace fub;
  include 'file1.php';
  include 'file2.php';
  include 'file3.php';
  use foo as feline;
  use bar as canine;
  use other\animate;       //use other\animate as animate;
  echo feline\Cat::says(), "<br />\n";
  echo canine\Dog::says(), "<br />\n";
  echo \animate\Animal::breathes(), "<br />\n";  ?>
```
[AUTOLOADING](https://www.php.net/manual/en/language.oop5.autoload.php)
```php
class Autoloader
{
    public static function register()
    {
        spl_autoload_register(function ($class) {
            $file = str_replace('\\', DIRECTORY_SEPARATOR, $class).'.php';
            if (file_exists($file)) {
                require $file;
                return true;
            }
            return false;
        });
    }
}
Autoloader::register();
```
```php
require __DIR__ . '/vendor/autoload.php';
```
[TRAITS](https://www.php.net/manual/en/language.oop5.traits.php)
```php
<?php
class Base {
    public function sayHello() {
        echo 'Hello ';
    }
}

trait SayWorld {
    public function sayHello() {
        parent::sayHello();
        echo 'World!';
    }
}

class MyHelloWorld extends Base {
    use SayWorld;
}

$o = new MyHelloWorld();
$o->sayHello();
?>
```
#### PHP CHAINING
```php
class Demo {
  public function hello(){
    echo "Hello";
    return $this;
  }
  public function world(){
    echo "World!";
    return $this;
  }
}
$demo = new Demo();
$demo->hello()->world();
```
#### STATIC CHAINING
```php
class Demo {
    public static function hello(){
        echo "Hello";
        return __CLASS__; //$this will not work in static
    }
    public static function world(){
        echo "World!";
        return __CLASS__;
    }
}
Demo::hellow()::world();
```
#### SELF, PARENT, STATIC, REGULAR OBJECT
```php
class Author {
	public static function person(){
		return "Rodriguez";
	}
}
class Book extends Author {
    public static $id;
    public static $bookname;
    public static $stock_qty;

    public function __construct($id,$bookname,$stock_qty)
    {
        self::$id = $id;
        self::$bookname = $bookname;
        self::$stock_qty = $stock_qty;
    }
    public function addBook($qty)
    {
		self::$stock_qty += $qty;
        self::display();
    }
    public static function display()
    {
        echo "Book id: ".self::$id."<br />";
        echo "Book name: ".self::$bookname."<br />";
		echo "Author: ".parent::person()."<br />";
        echo "Current stock: ".self::$stock_qty."<br />";
    }
	
}
$bookObject = new Book("123","Learning PHP",120);
$bookObject->addBook(30);
Book::display();
```
