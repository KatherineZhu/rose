# PHP and Yii2

## static vs self

```
<?php
class Human {
    private $name;
    private $gender;

    public function __construct($name = 'unknown', $gender = 'male')
    {
        $this->name = $name;
        $this->gender = $gender;
    }

    public function getName() {
        return $this->name;
    }

    public static function hello() {
        $instance = new static();
        // $instance = new self();
        return 'Hello from ' . $instance->getName();
    }
}

class Child extends Human {
    private $school;

    public function __construct($school = '', $name = 'child', $gender = 'boy')
    {
        $this->school = $school;
        parent::__construct($name, $gender);
    }
}


echo(Human::hello());
echo(Child::hello());
```

## DI

DI = Dependency Injection

```
Yii::$container->get('Foo', $params) = new Foo($params)
Yii::$container->invoke(['Foo', 'hello'], ['params1' => 'katherine']) = $foo->hello('katherine')
```
