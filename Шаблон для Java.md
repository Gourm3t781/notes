
1. Шаблон базового класса (Инкапсуляция, конструкторы, методы)

Используется, когда просят описать сущность (Студент, Книга, Товар) с приватными полями.

```java
public class MyClass {
    // 1. Приватные поля
    private int id;
    private String name;
    private double price;

    // 2. Конструктор по умолчанию
    public MyClass() {
        this.id = 0;
        this.name = "Unknown";
        this.price = 0.0;
    }

    // 3. Конструктор с параметрами (используем this)
    public MyClass(int id, String name, double price) {
        this.id = id;
        this.name = name;
        this.price = price;
    }

    // 4. Геттеры и сеттеры
    public int getId() { return id; }
    public void setId(int id) { this.id = id; }
    
    public String getName() { return name; }
    public void setName(String name) { this.name = name; }

    // 5. Переопределение toString для вывода
    @Override
    public String toString() {
        return "ID: " + id + ", Name: " + name + ", Price: " + price;
    }
}
```

2. Шаблон наследования и полиморфизма

Используется, когда просят создать производный класс (Студент-заочник, Электронная книга) и переопределить поведение.

```java
// Родительский класс
class Parent {
    protected String name;

    public Parent(String name) {
        this.name = name;
    }

    public void showInfo() {
        System.out.println("Родитель: " + name);
    }
}

// Дочерний класс
class Child extends Parent {
    private int extraParam;

    // Вызов конструктора родителя через super()
    public Child(String name, int extraParam) {
        super(name); 
        this.extraParam = extraParam;
    }

    // Переопределение метода (Полиморфизм)
    @Override
    public void showInfo() {
        super.showInfo(); // Вызов версии родителя
        System.out.println("Доп. параметр: " + extraParam);
    }
}

// Использование в main (Полиморфизм):
// Parent p = new Child("Тест", 5);
// p.showInfo(); 
```

3. Шаблон абстрактного класса

Используется, когда дают общее понятие (Фигура, Транспорт), которое нельзя создать напрямую.

```java
abstract class AbstractEntity {
    protected String title;

    public AbstractEntity(String title) {
        this.title = title;
    }

    // Абстрактный метод (без тела)
    public abstract double calculateSomething();

    // Обычный метод
    public void printTitle() {
        System.out.println("Название: " + title);
    }
}

class ConcreteEntity extends AbstractEntity {
    private double value;

    public ConcreteEntity(String title, double value) {
        super(title);
        this.value = value;
    }

    // Обязаны реализовать абстрактный метод
    @Override
    public double calculateSomething() {
        return value * 2; 
    }
}
```

4. Шаблон интерфейса

Используется, когда задают контракт (Printable, Movable) или несколько ролей.

```java
interface MyInterface {
    // Неявно public abstract
    void doSomething(); 
    
    // Метод с реализацией по умолчанию (Java 8+)
    default void defaultMethod() {
        System.out.println("Действие по умолчанию");
    }
}

class Implementor implements MyInterface {
    @Override
    public void doSomething() {
        System.out.println("Реализация метода");
    }
}
```

5. Шаблон вложенных классов

Используется, когда спрашивают разницу между inner и nested классами.

```java
public class Outer {
    private int outerField = 10;
    private static int staticField = 20;

    // 1. Нестатический внутренний класс (нужен объект Outer)
    class Inner {
        void print() {
            System.out.println(outerField); // Имеет доступ к полям Outer
        }
    }

    // 2. Статический вложенный класс (не нужен объект Outer)
    static class Nested {
        void print() {
            // System.out.println(outerField); // ОШИБКА! Нет доступа к нестатическому
            System.out.println(staticField);
        }
    }
}

// Создание в main:
// Outer outer = new Outer();
// Outer.Inner inner = outer.new Inner();
// Outer.Nested nested = new Outer.Nested();
```

6. Шаблон работы с исключениями (try-catch-finally)

Используется в задачах на обработку ошибок или проверку ввода.

```java
public void doSomething() {
    try {
        // Код, который может вызвать ошибку
        int[] arr = new int[5];
        arr[10] = 1; // Вызовет исключение
    } 
    catch (ArrayIndexOutOfBoundsException e) {
        System.out.println("Ошибка: выход за границы массива!");
    } 
    catch (Exception e) { // Общий перехват (должен быть последним)
        System.out.println("Другая ошибка: " + e.getMessage());
    } 
    finally {
        // Выполнится в любом случае
        System.out.println("Блок finally завершен.");
    }
}
```

7. Шаблон создания своего исключения (Custom Exception)

Используется, когда просят создать класс-исключение для специфической ошибки (например, отрицательный возраст).

```java
// 1. Создание класса исключения
class MyCustomException extends Exception {
    private int errorCode;

    public MyCustomException(String message, int errorCode) {
        super(message);
        this.errorCode = errorCode;
    }
    
    public int getErrorCode() { return errorCode; }
}

// 2. Использование в методе
class Validator {
    // Объявляем, что метод может выбросить ошибку (throws)
    public void checkAge(int age) throws MyCustomException {
        if (age < 0) {
            // Выбрасываем ошибку (throw)
            throw new MyCustomException("Возраст не может быть отрицательным", 404);
        }
    }
}

// 3. Обработка в main
// try {
//     validator.checkAge(-5);
// } catch (MyCustomException e) {
//     System.out.println(e.getMessage() + " Код: " + e.getErrorCode());
// }
```

8. Шаблон работы с массивами и String

Часто нужно в задачах на обработку данных.

```java
public class ArrayStringTemplate {
    public static void main(String[] args) {
        // --- МАССИВЫ ---
        int[] nums = new int[5];
        int[] initNums = {10, 20, 30};
        
        // Проход по массиву
        for (int i = 0; i < initNums.length; i++) {
            // initNums[i]
        }
        // foreach
        for (int num : initNums) {
            // num
        }

        // --- СТРОКИ ---
        String str = "Java";
        int len = str.length();               // длина
        char c = str.charAt(0);               // символ 'J'
        boolean eq = str.equals("Java");      // СРАВНЕНИЕ (не ==)
        boolean eqIgnore = str.equalsIgnoreCase("JAVA");
        String sub = str.substring(1, 3);     // "av"
        String[] words = str.split(" ");      // разбить на массив
    }
}
```
