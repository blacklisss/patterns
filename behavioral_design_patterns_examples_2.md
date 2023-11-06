
# Additional Behavioral Design Patterns Examples in PHP

## Interpreter Pattern
```php
<?php
interface Expression {
    public function interpret(array $context);
}

class TerminalExpression implements Expression {
    private $name;

    public function __construct($name) {
        $this->name = $name;
    }

    public function interpret(array $context) {
        return in_array($this->name, $context);
    }
}

class OrExpression implements Expression {
    private $expression1;
    private $expression2;

    public function __construct(Expression $expression1, Expression $expression2) {
        $this->expression1 = $expression1;
        $this->expression2 = $expression2;
    }

    public function interpret(array $context) {
        return $this->expression1->interpret($context) || $this->expression2->interpret($context);
    }
}

class AndExpression implements Expression {
    private $expression1;
    private $expression2;

    public function __construct(Expression $expression1, Expression $expression2) {
        $this->expression1 = $expression1;
        $this->expression2 = $expression2;
    }

    public function interpret(array $context) {
        return $this->expression1->interpret($context) && $this->expression2->interpret($context);
    }
}
?>
```

## Iterator Pattern
```php
<?php
interface IteratorAggregate extends Traversable {
    public function getIterator();
}

class ConcreteCollection implements IteratorAggregate {
    private $items = [];

    public function addItem($item) {
        $this->items[] = $item;
    }

    public function getIterator(): Iterator {
        return new AlphabeticalOrderIterator($this);
    }
}

class AlphabeticalOrderIterator implements Iterator {
    private $collection;
    private $position = 0;
    private $reverse = false;

    public function __construct($collection, $reverse = false) {
        $this->collection = $collection;
        $this->reverse = $reverse;
    }

    public function rewind() {
        $this->position = $this->reverse ? count($this->collection->items) - 1 : 0;
    }

    public function current() {
        return $this->collection->items[$this->position];
    }

    public function key() {
        return $this->position;
    }

    public function next() {
        $this->position += $this->reverse ? -1 : 1;
    }

    public function valid() {
        return isset($this->collection->items[$this->position]);
    }
}
?>
```
