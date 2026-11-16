# Абстрактный класс

Пример средней сложности: Student → ContractStudent. В абстрактном классе есть обычные методы (с кодом) и абстрактные (без кода). У наследника — своё дополнительное поле и свои расчёты.

## Файл 1: Student.java (родитель)

Родитель знает ФИО и курс, умеет печатать информацию. Как считать стипендию — не знает, поэтому такие методы абстрактные.

```java
package arraylist;

public abstract class Student {
    protected String fio;
    protected int kurs;

    public Student(String fio, int kurs) {
        this.fio = fio;
        this.kurs = kurs;
    }

    // --- ОБЫЧНЫЕ МЕТОДЫ (с телом) ---
    public String getFio() {
        return fio;
    }

    public void printInfo() {
        System.out.println("Студент: " + fio + " | Курс: " + kurs);
    }

    // --- АБСТРАКТНЫЕ МЕТОДЫ (без тела) ---
    // Родитель не знает, сколько стипендия, пусть решает наследник
    public abstract double calculateScholarship();

    // И не знает точный тип студента
    public abstract String getType();
}
```

## Файл 2: ContractStudent.java (наследник)

Платный студент: дополнительное поле price (стоимость обучения) и реализация обоих абстрактных методов.

```java
package arraylist;

public class ContractStudent extends Student {
    private double price; // Стоимость обучения

    // Конструктор
    public ContractStudent(String fio, int kurs, double price) {
        super(fio, kurs); // Передаем ФИО и курс родителю
        this.price = price;
    }

    // Реализуем абстрактный метод из родителя
    @Override
    public double calculateScholarship() {
        // Платники стипендию не получают
        return 0.0;
    }

    // Реализуем второй абстрактный метод
    @Override
    public String getType() {
        return "Платный студент (Долг за учебу: " + price + ")";
    }
}
```

## Файл 3: Main.java (запуск)

Ввод с клавиатуры и проверка всех методов. Строку с new Student не раскомментируй — абстрактный класс нельзя создать напрямую, компилятор выдаст ошибку.

```java
package arraylist;
import java.util.Scanner;

public class Main {
    public static void main(String[] args) {
        Scanner sc = new Scanner(System.in);
        Scanner scL = new Scanner(System.in);

        System.out.println("--- Ввод данных ---");
        System.out.print("Введите ФИО: ");
        String fio = scL.nextLine();

        System.out.print("Введите курс: ");
        int kurs = sc.nextInt();

        System.out.print("Введите стоимость обучения: ");
        double price = sc.nextDouble();

        // Создаем конкретного наследника
        ContractStudent student = new ContractStudent(fio, kurs, price);

        System.out.println("--- Результат ---");

        // 1. Обычный метод родителя
        student.printInfo();

        // 2. Реализованный абстрактный метод
        System.out.println("Тип: " + student.getType());

        // 3. Расчет стипендии
        System.out.println("Стипендия: " + student.calculateScholarship() + " руб.");

        // 4. Если препод спросит: "А можно создать обычного Student?"
        // new Student("Иванов", 1); // ОШИБКА! Абстрактный класс.

        sc.close();
        scL.close();
    }
}
```

## Как объяснить на защите

**Зачем printInfo() в абстрактном классе?**

Чтобы не дублировать код: ФИО и курс печатаются у всех студентов одинаково. Пишем один раз в родителе.

**Зачем abstract для calculateScholarship()?**

У платника стипендия 0, у бюджетника была бы, например, 2000. Родитель не знает формулу, поэтому требует от наследника написать её самому.

**Где здесь полиморфизм?**

В список со Student можно класть и платников, и бюджетников. В цикле вызываем у всех calculateScholarship() — каждый считает по-своему.
