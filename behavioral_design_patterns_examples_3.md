
# Additional Behavioral Design Patterns Examples in PHP

## Mediator Pattern
```php
<?php
interface Mediator {
    public function notify($sender, $event);
}

class ConcreteMediator implements Mediator {
    private $component1;
    private $component2;

    public function __construct(Component1 $c1, Component2 $c2) {
        $this->component1 = $c1;
        $this->component1->setMediator($this);
        $this->component2 = $c2;
        $this->component2->setMediator($this);
    }

    public function notify($sender, $event) {
        if ($event == "A") {
            echo "Mediator reacts on A and triggers following operations:\n";
            $this->component2->doC();
        }
        if ($event == "D") {
            echo "Mediator reacts on D and triggers following operations:\n";
            $this->component1->doB();
            $this->component2->doC();
        }
    }
}

class BaseComponent {
    protected $mediator;

    public function __construct($mediator = null) {
        $this->mediator = $mediator;
    }

    public function setMediator(Mediator $mediator) {
        $this->mediator = $mediator;
    }
}

class Component1 extends BaseComponent {
    public function doA() {
        echo "Component 1 does A.\n";
        $this->mediator->notify($this, "A");
    }

    public function doB() {
        echo "Component 1 does B.\n";
        $this->mediator->notify($this, "B");
    }
}

class Component2 extends BaseComponent {
    public function doC() {
        echo "Component 2 does C.\n";
        $this->mediator->notify($this, "C");
    }

    public function doD() {
        echo "Component 2 does D.\n";
        $this->mediator->notify($this, "D");
    }
}
?>
```

## Memento Pattern
```php
<?php
class Originator {
    private $state;

    public function __construct($state) {
        $this->state = $state;
        echo 'Originator: My initial state is: ' . $this->state . '\n';
    }

    public function doSomething() {
        echo 'Originator: I'm doing something important.\n';
        $this->state = $this->generateRandomString(30);
        echo 'Originator: and my state has changed to: ' . $this->state . '\n';
    }

    private function generateRandomString($length = 10) {
        return substr(str_shuffle('ABCDEFGHIJKLMNOPQRSTUVWXYZabcdefghijklmnopqrstuvwxyz0123456789'), 0, $length);
    }

    public function save(): Memento {
        return new ConcreteMemento($this->state);
    }

    public function restore(Memento $memento) {
        $this->state = $memento->getState();
        echo 'Originator: My state has changed to: ' . $this->state . '\n';
    }
}

interface Memento {
    public function getName(): string;
    public function getDate(): string;
    public function getState(): string;
}

class ConcreteMemento implements Memento {
    private $state;
    private $date;

    public function __construct($state) {
        $this->state = $state;
        $this->date = date('Y-m-d H:i:s');
    }

    public function getState(): string {
        return $this->state;
    }

    public function getName(): string {
        return $this->date . ' / (' . substr($this->state, 0, 9) . '...)';
    }

    public function getDate(): string {
        return $this->date;
    }
}

class Caretaker {
    private $mementos = [];
    private $originator;

    public function __construct(Originator $originator) {
        $this->originator = $originator;
    }

    public function backup() {
        echo '\nCaretaker: Saving Originator's state...\n';
        $this->mementos[] = $this->originator->save();
    }

    public function undo() {
        if (!count($this->mementos)) {
            return;
        }
        $memento = array_pop($this->mementos);
        echo '\nCaretaker: Restoring state to: ' . $memento->getName() . '\n';
        $this->originator->restore($memento);
    }

    public function showHistory() {
        echo '\nCaretaker: Here's the list of mementos:\n';
        foreach ($this->mementos as $memento) {
            echo $memento->getName() . '\n';
        }
    }
}
?>
```
