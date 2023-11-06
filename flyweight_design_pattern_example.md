
# Flyweight Design Pattern Example in PHP

```php
<?php
// Интринсическое состояние, которое хранится в объекте Flyweight
class CharacterFlyweight {
    private $character;

    public function __construct($character) {
        $this->character = $character;
    }

    public function render($font) {
        // В данном случае, внешнее состояние - это шрифт
        return "Character $this->character with font $font";
    }
}

// Фабрика, которая создает приспособленцев и управляет ими
class FlyweightFactory {
    private $pool = [];

    public function getFlyweight($character) {
        if (!isset($this->pool[$character])) {
            $this->pool[$character] = new CharacterFlyweight($character);
        }
        return $this->pool[$character];
    }
}

// Клиентский код
$factory = new FlyweightFactory();

// Поделимся одним и тем же приспособленцем для двух символов 'A', но с разными внешними состояниями
$flyweightA1 = $factory->getFlyweight('A');
$flyweightA2 = $factory->getFlyweight('A');

// Вывод будет одинаков для обоих объектов, несмотря на то, что внешнее состояние разное
echo $flyweightA1->render('Arial');
echo "\n";
echo $flyweightA2->render('Times New Roman');
?>
```
