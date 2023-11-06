
# Additional Behavioral Design Patterns Examples in PHP

## Strategy Pattern
```php
<?php
interface Strategy {
    public function doAlgorithm(array $data): array;
}

class Context {
    private $strategy;

    public function __construct(Strategy $strategy) {
        $this->strategy = $strategy;
    }

    public function setStrategy(Strategy $strategy) {
        $this->strategy = $strategy;
    }

    public function doSomeBusinessLogic(): void {
        echo 'Context: Sorting data using the strategy (not sure how it'll do it)\n';
        $result = $this->strategy->doAlgorithm(["a", "b", "c", "d", "e"]);
        echo implode(",", $result) . '\n';
    }
}

class ConcreteStrategyA implements Strategy {
    public function doAlgorithm(array $data): array {
        sort($data);
        return $data;
    }
}

class ConcreteStrategyB implements Strategy {
    public function doAlgorithm(array $data): array {
        rsort($data);
        return $data;
    }
}
?>
```

## Template Method Pattern
```php
<?php
abstract class AbstractClass {
    public function templateMethod(): void {
        $this->baseOperation1();
        $this->requiredOperations1();
        $this->baseOperation2();
        $this->hook1();
        $this->requiredOperation2();
        $this->baseOperation3();
        $this->hook2();
    }

    protected function baseOperation1(): void {
        echo 'AbstractClass says: I am doing the bulk of the work\n';
    }

    protected function baseOperation2(): void {
        echo 'AbstractClass says: But I let subclasses override some operations\n';
    }

    protected function baseOperation3(): void {
        echo 'AbstractClass says: But I am doing the bulk of the work anyway\n';
    }

    protected abstract function requiredOperations1(): void;
    protected abstract function requiredOperation2(): void;

    protected function hook1(): void { }
    protected function hook2(): void { }
}

class ConcreteClass1 extends AbstractClass {
    protected function requiredOperations1(): void {
        echo 'ConcreteClass1 says: Implemented Operation1\n';
    }

    protected function requiredOperation2(): void {
        echo 'ConcreteClass1 says: Implemented Operation2\n';
    }
}

class ConcreteClass2 extends AbstractClass {
    protected function requiredOperations1(): void {
        echo 'ConcreteClass2 says: Implemented Operation1\n';
    }

    protected function requiredOperation2(): void {
        echo 'ConcreteClass2 says: Implemented Operation2\n';
    }

    protected function hook1(): void {
        echo 'ConcreteClass2 says: Overridden Hook1\n';
    }
}
?>
```
