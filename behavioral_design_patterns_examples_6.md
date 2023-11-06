
# Additional Behavioral Design Patterns Examples in PHP

## Visitor Pattern
```php
<?php
interface Component {
    public function accept(Visitor $visitor);
}

class ConcreteComponentA implements Component {
    public function accept(Visitor $visitor) {
        $visitor->visitConcreteComponentA($this);
    }

    public function exclusiveMethodOfConcreteComponentA(): string {
        return 'A';
    }
}

class ConcreteComponentB implements Component {
    public function accept(Visitor $visitor) {
        $visitor->visitConcreteComponentB($this);
    }

    public function specialMethodOfConcreteComponentB(): string {
        return 'B';
    }
}

interface Visitor {
    public function visitConcreteComponentA(ConcreteComponentA $element);
    public function visitConcreteComponentB(ConcreteComponentB $element);
}

class ConcreteVisitor1 implements Visitor {
    public function visitConcreteComponentA(ConcreteComponentA $element) {
        echo 'ConcreteVisitor1: ' . $element->exclusiveMethodOfConcreteComponentA() . '\n';
    }

    public function visitConcreteComponentB(ConcreteComponentB $element) {
        echo 'ConcreteVisitor1: ' . $element->specialMethodOfConcreteComponentB() . '\n';
    }
}

class ConcreteVisitor2 implements Visitor {
    public function visitConcreteComponentA(ConcreteComponentA $element) {
        echo 'ConcreteVisitor2: ' . $element->exclusiveMethodOfConcreteComponentA() . '\n';
    }

    public function visitConcreteComponentB(ConcreteComponentB $element) {
        echo 'ConcreteVisitor2: ' . $element->specialMethodOfConcreteComponentB() . '\n';
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
    protected $children;

    public function __construct() {
        $this->children = [];
    }

    public function add(Component $component): void {
        $this->children[spl_object_hash($component)] = $component;
    }

    public function remove(Component $component): void {
        unset($this->children[spl_object_hash($component)]);
    }

    public function operation(): string {
        $results = [];
        foreach ($this->children as $child) {
            $results[] = $child->operation();
        }
        return 'Branch(' . implode('+', $results) . ')';
    }
}
?>
```
