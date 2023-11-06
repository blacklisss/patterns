
# Creational Design Patterns Examples in PHP

## Abstract Factory Pattern
```php
<?php
interface AbstractFactory {
    public function createProductA();
    public function createProductB();
}

class ConcreteFactory1 implements AbstractFactory {
    public function createProductA() {
        return new ProductA1();
    }

    public function createProductB() {
        return new ProductB1();
    }
}

class ConcreteFactory2 implements AbstractFactory {
    public function createProductA() {
        return new ProductA2();
    }

    public function createProductB() {
        return new ProductB2();
    }
}

interface AbstractProductA {
    public function usefulFunctionA();
}

class ProductA1 implements AbstractProductA {
    public function usefulFunctionA() {
        return "The result of the product A1.";
    }
}

class ProductA2 implements AbstractProductA {
    public function usefulFunctionA() {
        return "The result of the product A2.";
    }
}

interface AbstractProductB {
    public function usefulFunctionB();
}

class ProductB1 implements AbstractProductB {
    public function usefulFunctionB() {
        return "The result of the product B1.";
    }
}

class ProductB2 implements AbstractProductB {
    public function usefulFunctionB() {
        return "The result of the product B2.";
    }
}
?>
```

## Builder Pattern
```php
<?php
class Product {
    public $parts = [];

    public function addPart($part) {
        $this->parts[] = $part;
    }
}

interface Builder {
    public function producePartA();
    public function producePartB();
    public function producePartC();
    public function getProduct(): Product;
}

class ConcreteBuilder implements Builder {
    private $product;

    public function __construct() {
        $this->reset();
    }

    public function reset() {
        $this->product = new Product();
    }

    public function producePartA() {
        $this->product->addPart("PartA1");
    }

    public function producePartB() {
        $this->product->addPart("PartB1");
    }

    public function producePartC() {
        $this->product->addPart("PartC1");
    }

    public function getProduct(): Product {
        $result = $this->product;
        $this->reset();
        return $result;
    }
}

class Director {
    private $builder;

    public function setBuilder(Builder $builder) {
        $this->builder = $builder;
    }

    public function buildMinimalViableProduct() {
        $this->builder->producePartA();
    }

    public function buildFullFeaturedProduct() {
        $this->builder->producePartA();
        $this->builder->producePartB();
        $this->builder->producePartC();
    }
}
?>
```

## Factory Method Pattern
```php
<?php
abstract class Creator {
    abstract public function factoryMethod(): Product;

    public function someOperation(): string {
        $product = $this->factoryMethod();
        return "Creator: The same creator's code has just worked with " . $product->operation();
    }
}

class ConcreteCreator1 extends Creator {
    public function factoryMethod(): Product {
        return new ConcreteProduct1();
    }
}

class ConcreteCreator2 extends Creator {
    public function factoryMethod(): Product {
        return new ConcreteProduct2();
    }
}

interface Product {
    public function operation(): string;
}

class ConcreteProduct1 implements Product {
    public function operation(): string {
        return "{Result of the ConcreteProduct1}";
    }
}

class ConcreteProduct2 implements Product {
    public function operation(): string {
        return "{Result of the ConcreteProduct2}";
    }
}
?>
```

## Prototype Pattern
```php
<?php
abstract class Prototype {
    public $primitive;
    public $component;
    public $circularReference;

    abstract public function __clone();

    public function __construct() {
        $this->primitive = 245;
        $this->component = new \DateTime();
        $this->circularReference = new ComponentWithBackReference($this);
    }
}

class ConcretePrototype extends Prototype {
    public function __clone() {
        $this->component = clone $this->component;
        $this->circularReference = clone $this->circularReference;
        $this->circularReference->prototype = $this;
    }
}

class ComponentWithBackReference {
    public $prototype;

    public function __construct(Prototype $prototype) {
        $this->prototype = $prototype;
    }
}
?>
```

## Singleton Pattern (Already Provided)
```php
<?php
class Singleton {
    private static $instance;

    private function __construct() {
        // Конструктор приватный, чтобы предотвратить использование new
    }

    public static function getInstance() {
        if (self::$instance === null) {
            self::$instance = new self();
        }
        return self::$instance;
    }

    public function someBusinessLogic() {
        // ...
    }
}
?>
```
