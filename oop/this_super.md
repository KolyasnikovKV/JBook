# Ключевые слова this и super

Ключевое слово `this` - это ссылка на объект текущего класса, а `super` - ссылка на объект его родительского класса.
Само слово `super` пошло из теории множеств, где используется термин супермножество.

Раз у нас есть ссылка на объект текущего и родительского класса, то и доступ до полей класса может быть организован через эту ссылку.
Примеры этого вы уже неоднократно встречали:

```java
public Person(int age, String name) {
  this.age = age;
  this.name = name;
}
```

С помощью ссылки мы получили поле `age` у текущего объекта и присвоили туда значение, переданное в конструктор.
Аналогичная ситуация с полем `name`.

Ключевые слова `this` и `super` также могут быть использованы для вызова конструктора текущего или родительского класса соответственно:

```java
class Person {
  protected int age;
  protected String name;

  public Person(int age, String name) {
    this.age = age;
    this.name = name;
  }

  public Person(String name) {
    this(20, name);
  }

  // some remaining code
}
```

Здесь мы создали два конструктора класса, один из которых принимает два аргумента, а другой - только один.
В теле второго конструктора мы вызываем первый, наиболее расширенный, передавая ему значение по умолчанию `20`.

Второй конструктор по сути равносилен следующей записи:

```java
public Person(String name) {
  this.age = 20;
  this.name = name;
}
```

То же самое справедливо и для `super()`, что мы видели в примере выше:

```java
class Employee extends Person {
    private String address;

    public Employee(int age, String name, String address) {
        super(age, name);
        this.address = address
    }
}
```

Из этого кода можно сделать вывод, что если у родительского класса есть не пустой конструктор, 
то при создании дочернего класса мы **обязаны** вызвать родительский конструктор, что и происходит.

Если задуматься, то это довольно логично, ведь вы строите свой дочерний класс, который базируется на родителськом.
Нельзя построить дом, не заложив фундамент.

И `this`, и `super` — это нестатические переменные, соответственно их нельзя использовать в статическом контексте.
Переменной `this` нельзя присвоить новое значение, что тоже логично.

Теперь стоит сказать о том, что в `Java` все классы неявно являются наследниками `java.lang.Object`.
А значит при создании любого класса, происходит вызов родительского конструктора через `super()`.

При этом, даже если вы не создали ни одного конструктора класса, то `Java` все равно создаст пустой конструктор по умолчанию и вызовет там `super()`.
Ведь мы создаем объект класса, родительский класс которого `java.lang.Object`.

---

Если вам потребовалось внутри класса вызвать конструктор, например, более расширенный, то делать это надо именно через `this`:

```java
class Employee extends Person {
    protected String address;

    public Employee(int age, String name, String address) {
        super(age, name);
        this.address = address;
    }

    public Employee(int age, String name) {
        this(age, name, "EMPTY");
    }
}
```

В примере выше создаетсядва конструктора, внутри которого вызываем главный, с наибольшим количеством аргументов.

Этот прием используется, например тогда, когда вам необходимо для каких-то полей сделать значения по умолчанию.

---

Для закрепления:

| Ключевое слово | Описание                                               |
|----------------|--------------------------------------------------------|
| this           | Ссылка на текущий объект                               |
| super          | Ссылка на родительский объект                          |
| this()         | Вызов конструктора без аргументов                      |
| super()        | Вызов конструктора без аргументов родительского класса |

## Полезные ссылки

1. [Лекторий ФИВТ, Java (3 курс) - лектор Пономарёв. Java #3. Классы](https://www.youtube.com/watch?v=BC7OWimiVoo&list=PL4_hYwCyhAvblhTbPQmOF4b3kilWSpOjU&index=3)
2. [Ключевое слово this](https://www.examclouds.com/ru/java/java-core-russian/keyword-this)