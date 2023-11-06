
# Additional Behavioral Design Patterns Examples in PHP

## Observer Pattern
```php
<?php
interface Observer {
    public function update($subject);
}

interface Subject {
    public function attach(Observer $observer);
    public function detach(Observer $observer);
    public function notify();
}

class ConcreteSubject implements Subject {
    public $state;
    private $observers;

    public function __construct() {
        $this->observers = [];
    }

    public function attach(Observer $observer) {
        $this->observers[] = $observer;
    }

    public function detach(Observer $observer) {
        foreach ($this->observers as $key => $s) {
            if ($s === $observer) {
                unset($this->observers[$key]);
            }
        }
    }

    public function notify() {
        echo 'Subject: Notifying observers...\n';
        foreach ($this->observers as $observer) {
            $observer->update($this);
        }
    }

    public function someBusinessLogic() {
        echo 'Subject: I'm doing something important.\n';
        $this->state = rand(0, 10);
        echo "Subject: My state has just changed to: {$this->state}\n";
        $this->notify();
    }
}

class ConcreteObserverA implements Observer {
    public function update($subject) {
        if ($subject->state < 3) {
            echo 'ConcreteObserverA: Reacted to the event.\n';
        }
    }
}

class ConcreteObserverB implements Observer {
    public function update($subject) {
        if ($subject->state === 0 || $subject->state >= 2) {
            echo 'ConcreteObserverB: Reacted to the event.\n';
        }
    }
}
?>
```

## State Pattern
```php
<?php
interface State {
    public function doAction(Context $context);
}

class Context {
    private $state;

    public function __construct(State $state) {
        $this->transitionTo($state);
    }

    public function transitionTo(State $state) {
        echo "Context: Transition to " . get_class($state) . ".\n";
        $this->state = $state;
        $this->state->setContext($this);
    }

    public function request1() {
        $this->state->handle1();
    }

    public function request2() {
        $this->state->handle2();
    }
}

class ConcreteStateA implements State {
    private $context;

    public function setContext(Context $context) {
        $this->context = $context;
    }

    public function handle1() {
        echo "ConcreteStateA handles request1.\n";
        echo "ConcreteStateA wants to change the state of the context.\n";
        $this->context->transitionTo(new ConcreteStateB());
    }

    public function handle2() {
        echo "ConcreteStateA handles request2.\n";
    }
}

class ConcreteStateB implements State {
    private $context;

    public function setContext(Context $context) {
        $this->context = $context;
    }

    public function handle1() {
        echo "ConcreteStateB handles request1.\n";
    }

    public function handle2() {
        echo "ConcreteStateB handles request2.\n";
        echo "ConcreteStateB wants to change the state of the context.\n";
        $this->context->transitionTo(new ConcreteStateA());
    }
}
?>
```
