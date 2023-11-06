
# Behavioral Design Patterns Examples in PHP

## Chain of Responsibility Pattern
```php
<?php
abstract class Handler {
    protected $nextHandler;

    public function setNext(Handler $handler): Handler {
        $this->nextHandler = $handler;
        return $handler;
    }

    public function handle($request): ?string {
        if ($this->nextHandler) {
            return $this->nextHandler->handle($request);
        }

        return null;
    }
}

class ConcreteHandler1 extends Handler {
    public function handle($request): ?string {
        if ($request == "1") {
            return 'Handler 1 at your service!';
        } else {
            return parent::handle($request);
        }
    }
}

class ConcreteHandler2 extends Handler {
    public function handle($request): ?string {
        if ($request == "2") {
            return 'Handler 2 at your service!';
        } else {
            return parent::handle($request);
        }
    }
}

// Client code
$handler1 = new ConcreteHandler1();
$handler2 = new ConcreteHandler2();

$handler1->setNext($handler2);

echo $handler1->handle('2');
?>
```

## Command Pattern
```php
<?php
interface Command {
    public function execute();
}

class SimpleCommand implements Command {
    private $payload;

    public function __construct($payload) {
        $this->payload = $payload;
    }

    public function execute() {
        echo 'SimpleCommand: See, I can do simple things like printing (' . $this->payload . ').';
    }
}

class ComplexCommand implements Command {
    private $receiver;
    private $a;
    private $b;

    public function __construct(Receiver $receiver, $a, $b) {
        $this->receiver = $receiver;
        $this->a = $a;
        $this->b = $b;
    }

    public function execute() {
        echo 'ComplexCommand: Complex stuff should be done by a receiver object.';
        $this->receiver->doSomething($this->a);
        $this->receiver->doSomethingElse($this->b);
    }
}

class Receiver {
    public function doSomething($a) {
        echo '\nReceiver: Working on (' . $a . ').';
    }

    public function doSomethingElse($b) {
        echo '\nReceiver: Also working on (' . $b . ').';
    }
}

class Invoker {
    private $onStart;
    private $onFinish;

    public function setOnStart(Command $command) {
        $this->onStart = $command;
    }

    public function setOnFinish(Command $command) {
        $this->onFinish = $command;
    }

    public function doSomethingImportant() {
        echo 'Invoker: Does anybody want something done before I begin?';
        if ($this->onStart) {
            $this->onStart->execute();
        }

        echo '\nInvoker: ...doing something really important...';

        echo '\nInvoker: Does anybody want something done after I finish?';
        if ($this->onFinish) {
            $this->onFinish->execute();
        }
    }
}
?>
```
