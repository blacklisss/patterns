
# Structural Design Patterns Examples in PHP

## Adapter Pattern
```php
<?php
// Целевой интерфейс
interface Target {
    public function request(): string;
}

// Адаптируемый класс
class Adaptee {
    public function specificRequest(): string {
        return ".eetpadA eht fo roivaheb laicepS";
    }
}

// Адаптер
class Adapter implements Target {
    private $adaptee;

    public function __construct(Adaptee $adaptee) {
        $this->adaptee = $adaptee;
    }

    public function request(): string {
        return strrev($this->adaptee->specificRequest());
    }
}
?>
```

## Bridge Pattern
```php
<?php
interface Implementor {
    public function operationImpl();
}

class Abstraction {
    protected $implementor;

    public function __construct(Implementor $implementor) {
        $this->implementor = $implementor;
    }

    public function operation() {
        $this->implementor->operationImpl();
    }
}

class ConcreteImplementorA implements Implementor {
    public function operationImpl() {
        echo "ConcreteImplementorA Operation
";
    }
}

class ConcreteImplementorB implements Implementor {
    public function operationImpl() {
        echo "ConcreteImplementorB Operation
";
    }
}

class RefinedAbstraction extends Abstraction {
    public function refinedOperation() {
        echo "RefinedOperation
";
        $this->implementor->operationImpl();
    }
}
?>
```

## Composite Pattern
```php
<?php
interface Component {
    public function operation(): string;
}

class Leaf implements Component {
    public function operation(): string {
        return 'Leaf';
    }
}

class Composite implements Component {
    protected $children = [];

    public function add(Component $component) {
        $this->children[] = $component;
    }

    public function operation(): string {
        $results = [];
        foreach ($this->children as $child) {
            $results[] = $child->operation();
        }
        return "Branch(" . implode('+', $results) . ")";
    }
}
?>
```

## Decorator Pattern
```php
<?php
interface Component {
    public function operation(): string;
}

class ConcreteComponent implements Component {
    public function operation(): string {
        return "ConcreteComponent";
    }
}

class Decorator implements Component {
    protected $component;

    public function __construct(Component $component) {
        $this->component = $component;
    }

    public function operation(): string {
        return $this->component->operation();
    }
}

class ConcreteDecoratorA extends Decorator {
    public function operation(): string {
        return "ConcreteDecoratorA(" . parent::operation() . ")";
    }
}

class ConcreteDecoratorB extends Decorator {
    public function operation(): string {
        return "ConcreteDecoratorB(" . parent::operation() . ")";
    }
}
?>
```

## Facade Pattern
```php
<?php
class Subsystem1 {
    public function operation1(): string {
        return "Subsystem1: Ready!\n";
    }

    public function operationN(): string {
        return "Subsystem1: Go!\n";
    }
}

class Subsystem2 {
    public function operation1(): string {
        return "Subsystem2: Get ready!\n";
    }

    public function operationZ(): string {
        return "Subsystem2: Fire!\n";
    }
}

class Facade {
    protected $subsystem1;
    protected $subsystem2;

    public function __construct(Subsystem1 $subsystem1 = null, Subsystem2 $subsystem2 = null) {
        $this->subsystem1 = $subsystem1 ?: new Subsystem1();
        $this->subsystem2 = $subsystem2 ?: new Subsystem2();
    }

    public function operation(): string {
        $result = "Facade initializes subsystems:\n";
        $result .= $this->subsystem1->operation1();
        $result .= $this->subsystem2->operation1();
        $result .= "Facade orders subsystems to perform the action:\n";
        $result .= $this->subsystem1->operationN();
        $result .= $this->subsystem2->operationZ();
        return $result;
    }
}
?>
```

## Flyweight Pattern
```php
<?php
class FlyweightFactory {
    private $flyweights = [];

    public function getFlyweight($sharedState): Flyweight {
        $key = $this->getKey($sharedState);
        if (!isset($this->flyweights[$key])) {
            echo "FlyweightFactory: Can't find a flyweight, creating new one.\n";
            $this->flyweights[$key] = new ConcreteFlyweight($sharedState);
        } else {
            echo "FlyweightFactory: Reusing existing flyweight.\n";
        }
        return $this->flyweights[$key];
    }

    private function getKey($state): string {
        return implode("_", $state);
    }
}

class ConcreteFlyweight {
    private $sharedState;

    public function __construct($sharedState) {
        $this->sharedState = $sharedState;
    }

    public function operation($uniqueState): void {
        $s = json_encode($this->sharedState);
        $u = json_encode($uniqueState);
        echo "ConcreteFlyweight: Displaying shared ($s) and unique ($u) state.\n";
    }
}
?>
```

## Proxy Pattern
```php
<?php
interface Subject {
    public function request(): void;
}

class RealSubject implements Subject {
    public function request(): void {
        echo "RealSubject: Handling request.\n";
    }
}

class Proxy implements Subject {
    private $realSubject;

    public function __construct(RealSubject $realSubject) {
        $this->realSubject = $realSubject;
    }

    public function request(): void {
        if ($this->checkAccess()) {
            $this->realSubject->request();
            $this->logAccess();
        }
    }

    private function checkAccess(): bool {
        echo "Proxy: Checking access prior to firing a real request.\n";
        return true;
    }

    private function logAccess(): void {
        echo "Proxy: Logging the time of request.\n";
    }
}
?>
```
