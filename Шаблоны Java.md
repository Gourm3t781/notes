
## Файл 1: Сущность.java

Сущность меняешь на нужное слово (Student, Tovar, Car), а pole1 и pole2 — на характеристики (например, name и price). Обычно одно поле строковое, одно числовое (если дробное — ставь double).

```java
package arraylist;
import java.util.Comparator;

public class Сущность {
    // 1. Поля (обычно одно строковое, одно числовое)
    private String pole1; // Например, name, fio, model
    private int pole2;    // Например, price, kurs, speed

    // 2. Конструктор
    public Сущность(String pole1, int pole2) {
        this.pole1 = pole1;
        this.pole2 = pole2;
    }

    // 3. Геттеры (нужны для поиска и сортировки)
    public String getPole1() { return pole1; }
    public int getPole2() { return pole2; }

    // 4. Красивый вывод
    public String toString() {
        return "Pole1: " + pole1 + " | Pole2: " + pole2;
    }

    // 5. Компараторы (правила сортировки)

    // Для строк всегда compareToIgnoreCase
    public static class NameComparator implements Comparator<Сущность> {
        public int compare(Сущность o1, Сущность o2) {
            return o1.getPole1().compareToIgnoreCase(o2.getPole1());
        }
    }

    // Для чисел всегда Integer.compare (или Double.compare)
    public static class NumberComparator implements Comparator<Сущность> {
        public int compare(Сущность o1, Сущность o2) {
            return Integer.compare(o1.getPole2(), o2.getPole2());
        }
    }
}
```

## Файл 2: Менеджер.java

Менеджер меняешь на название базы (например, StudentGroup или Library). Имя своей сущности подставляешь в ArrayList.

```java
package arraylist;
import java.util.ArrayList;

public class Менеджер {
    private ArrayList<Сущность> a;

    public Менеджер() {
        a = new ArrayList<Сущность>();
    }

    // Добавление
    public void add(Сущность obj) {
        a.add(obj);
    }

    // Вывод на экран
    public void print() {
        System.out.println("--- Spisok ---");
        for (int i = 0; i < a.size(); i++) {
            System.out.println(a.get(i).toString());
        }
    }

    // !! ВАЖНО: УДАЛЕНИЕ (ВЫУЧИТЬ НАИЗУСТЬ)
    public void del(String searchPole1) {
        int p = -1; // Флаг, что не нашли
        for (int i = 0; i < a.size(); i++) {
            // Строки сравниваем через equalsIgnoreCase
            if (a.get(i).getPole1().equalsIgnoreCase(searchPole1)) {
                p = i; // Запоминаем индекс
                break; // Выходим из цикла
            }
        }

        if (p != -1) {
            a.remove(p);
            System.out.println("Udalen.");
        } else {
            System.out.println("Ne naiden.");
        }
    }

    // Сортировки
    public void sortByPole1() {
        a.sort(new Сущность.NameComparator());
    }

    public void sortByPole2() {
        a.sort(new Сущность.NumberComparator());
    }
}
```

## Файл 3: Main.java

Самый важный файл для ввода. Сканеры sc и scL не меняй — это спасение от багов: один для чисел (nextInt), другой для строк (nextLine).

```java
package arraylist;
import java.util.Scanner;

public class Main {
    public static void main(String[] args) {
        Scanner sc = new Scanner(System.in);  // Для чисел (nextInt)
        Scanner scL = new Scanner(System.in); // Для строк (nextLine)
        Менеджер mgr = new Менеджер();

        System.out.print("Vvedite kol-vo: ");
        int n = sc.nextInt();

        // Ввод данных
        for (int i = 0; i < n; i++) {
            System.out.print("Vvedite Pole1 (tekst): ");
            String p1 = scL.nextLine(); // Читаем строку

            System.out.print("Vvedite Pole2 (chislo): ");
            int p2 = sc.nextInt();      // Читаем число

            // Создаем и добавляем
            mgr.add(new Сущность(p1, p2));
        }

        // 1. Сортировка по тексту
        System.out.println("\n--- Sort po Pole1 ---");
        mgr.sortByPole1();
        mgr.print();

        // 2. Сортировка по числу
        System.out.println("\n--- Sort po Pole2 ---");
        mgr.sortByPole2();
        mgr.print();

        // 3. Удаление
        System.out.print("\nVvedite Pole1 dlya udaleniya: ");
        String delP1 = scL.nextLine();
        mgr.del(delP1);

        System.out.println("\n--- Posle udaleniya ---");
        mgr.print();

        sc.close();
        scL.close();
    }
}
```

## Как переделать под задание

Пример задания на контрольной:

> Описать класс Студент (ФИО, Курс). Удалить по ФИО, отсортировать по ФИО и Курсу.

1. Файл 1: Сущность → Student, pole1 → fio, pole2 → kurs.
2. Геттеры по всему коду: getFio() и getKurs().
3. Файл 2: Менеджер → StudentGroup, тип в ArrayList → Student.
4. В del() сравнение меняешь на getFio().
5. Файл 3: в Main правишь только тексты «Vvedite FIO» и «Vvedite Kurs».

## Что перепроверить на контрольной

**Ситуация · Как правильно**

• Дробное поле — тип double, ввод через sc.nextDouble()
• Компаратор для дробного — Double.compare(...)
• Числа сравниваем через ==, строки — только .equalsIgnoreCase()
• Конец Main — закрывай сканеры: sc.close(); scL.close();

На контрольной держи эти три файла перед глазами — подстановка под задание занимает пару минут.
