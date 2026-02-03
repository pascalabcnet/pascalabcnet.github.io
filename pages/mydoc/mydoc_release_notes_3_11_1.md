---
title: Версия 3.11.1
keywords: release notes, what's new, announcements, new features
last_updated: 03.02.26
sidebar: mydoc_sidebar
permalink: mydoc_release_notes_3_11_1.html
toс: false
folder: mydoc
---

## Изменения в версии 3.11.1 (03.02.26)

### Основное - реализован новый модуль DataFrameABC для промышленной работы с датасетами
Возможности DataFrameABC:
* Постолбцовое хранение данных (column-store) для высокой производительности
* Типизированные столбцы: int, float, string, bool с поддержкой NA
* Cursor API для эффективного построчного обхода без лишних аллокаций
* Фильтрация строк с пользовательским предикатом (`Filter`)
* Изменение схемы таблицы: `Select`, `Drop`, `Rename`
* Добавление вычисляемых столбцов: `WithColumn*`
* Группировка данных и агрегаты (`GroupBy`)
* Соединение таблиц: Inner / Left / Full Join (один и несколько ключей)
* Описательная статистика: `Count`, `Mean`, `Std`, `Min`, `Max`, `Describe`
* Загрузка данных из CSV и многострочного CSV-текста
* Удобный вывод больших таблиц с предпросмотром (`Print` / `PrintPreview`)

Семантика операций:
* `Select` / `Drop` / `Rename` → view (без копирования данных)
* `Filter` / `Join` / `Sort` / `WithColumn` → materialize (создаются новые массивы)
* `Count` считает только валидные (non-NA) значения

Пример:
```pascal
uses DataFrameABC;

begin
  var df := DataFrame.FromCsvText('''
  name,age,score
  Alice,20,85
  Bob,22,90
  Charlie,21,78
  Bob,22,90
  Clara,,78
  Kat,21,NA
  ''');
  
  df.Filter(row -> row.Int('age') > 20).Println;
  
  df.GroupBy('age').Mean('score').Println;

  var stat := df.Describe('score');
  Println($'score: count={stat.Count}, min={stat.Min}, max={stat.Max}, mean={stat.Mean}, std={stat.Std.Round(3)}');
end.
```

## Изменения в языке PascalABC.NET 3.11.1

### Контейнеры в интерполированных строках

Теперь в интерполированных строках в качестве значений можно использовать любые контейнеры - они будут интерполироваться до соответствующего значения
```pascal
begin
  Println($'{Arr(1,2,3)}');
  Println($'{Lst([1,2],[3,4])}');
  Println($'{SetOf(1,2,3)}');
end.
```
Будет выведено
```
[1,2,3]
[[1,2],[3,4]]
{1,2,3}
```

### Паскалевский спецификатор формата данных в Println
В Print - Println, так же как и в Write, можно теперь использовать паскалевские спецификаторы формата данных
```pascal
  Print(Pi:0:4)
```

## Изменения в стандартных функциях

### ArrRandomInteger и MatrRandomInteger с диапазоном
Функции ArrRandomInteger и MatrRandomInteger могут теперь принимать диапазон, что воспринимается естественнее чем 2 параметра:
```pascal
begin
  var a := ArrRandomInteger(10,5..10);
  a.Println;
  var m := MatrRandomInteger(3,4,2..9);
  m.Println(3);
end.
```

### Параметр Digits в методе ToString для массива вещественных
При преобразовании масива вещественных в строку можно указать, сколько цифр после десятичной точки учитывается. Это аналогично методу ToString для индивидуального значения типа real:
```pascal
begin
  var a := ArrRandomReal(5);
  var s := a.ToString(1);
  Println(s);
  var r: real := 3.14;
  s := r.ToString(1);
  Println(s);
end.
```

Вывод:
```
1.3 1.5 0.4 9.8 1.3
3.1
```

### Параметр allowOverlap в методе CountOf
В методе CountOf для последовательностей появился необязательный параметр allowOverlap, позволяющий учесть наложения 
```pascal
begin
  var s := 'arara';
  s.CountOf('ara').Println;
  s.CountOf('ara',allowOverlap := True).Println;
end.
```

Вывод:
```
1
2
```

## Новые стандартные функции и методы

### Кубический корень - работает и с отрицательными числами
```pascal
Print(CbRt(-27));
```

### Метод Copy для массивов и матриц
Для клонирования массивов и матриц можно теперь использовать стандартный метод `Copy`
```pascal
begin
  var a := [1,3,5];
  var a1 := a.Copy;
  Println(a1);
  var m := Matr([2,4,6],[3,5,8]);
  var m1 := m.Copy;
  Println(m1);
end.
```

### Стандартные функции GCD, GCM
Функции нахождения наибольшего общего делителя и наименьшего общего кратного теперь - в стандартном модуле
```pascal
##
var (a,b) := (56,98);
 
var GCD := GCD(a, b);
var LCM := LCM(a, b);
```

### Стандартная функция Hypot
Стандартная функция Hypot позволяет находить гипотенузу по двум катетам. ее реализация - это не наивное Sqrt(a*a + b*b) и позволяет избежать переполнения при больших a или b
```pascal
##
Hypot(3,4).Println;
Hypot(6,5).Println;
```

### Стандартная функция ISqrt
Вычисляет целочисленный квадратный корень из n. Возвращает наибольшее целое число r, такое что r² ≤ n < (r+1)².
Если n является точным квадратом, возвращает точный корень.

```pascal
begin
  Println(ISqrt(15),ISqrt(16));
  var bi: BigInteger := 6125476524;
  bi := bi*bi;
  Print(ISqrt(bi),ISqrt(bi-1));
end.
```

Вывод:
```
3 4
6125476524 6125476523 
```

### Метод массива и списка Swap
Метод массива и списка Swap меняет местами элементы по заданным индексам
```pascal
begin
  var L := Lst(1..10);
  // Меняет местами элементы списка
  L.Swap(0,9).Swap(1,8).Println;

  var a := Arr(1..10);
  // Меняет местами элементы массива
  a.Swap(0,9).Swap(1,8).Print;
end.
```

Вывод:
```
10 9 3 4 5 6 7 8 2 1
10 9 3 4 5 6 7 8 2 1
```

### Стандартная функция Lerp
Функция `Lerp` возвращает значение на отрезке [a, b] в позиции t ∈ [0, 1]. 
При t=0 — a, при t=1 — b, при t=0.5 — середина между ними.
```pascal
var x := Lerp(10, 20, 0.5);     // x = 15 (середина между 10 и 20)
var y := Lerp(0, 100, 0.25);    // y = 25 (четверть пути от 0 до 100)
var z := Lerp(5, 15, 0.0);      // z = 5 (начало отрезка)
```

### Стандартная функция Remap
Функция `Remap` преобразует x из диапазона [a1,b1] в диапазон [a2,b2] с сохранением относительной позиции.
```pascal
var a := Remap(0.5, 0, 1, 0, 100);  // a = 50 (0.5 → 50%)
var b := Remap(75, 0, 100, 0, 1);   // b = 0.75 (75% → 0.75)
var c := Remap(15, 10, 20, 0, 200); // c = 100 (15 посередине между 10 и 20 → 100 посередине между 0 и 200)
```

### Метод Elements для матриц
Метод Elements для матриц является сининимом метода ElementsByRow и превращает матрицу в одномерный массив по строкам:
```pascal
begin
  var m := MatrRandomInteger(3,4,1..10);
  m.Println;
  m.Elements.Println;
  Println(m.Elements.Min,m.Elements.Max,m.Elements.Sum);
end.
```

Вывод:
```
  10   2   4   5
   5  10   5   3
   5   4   5   3
10 2 4 5 5 10 5 3 5 4 5 3
2 10 61
```

### Метод None для последовательностей
Метод None для последовательностей возвращает Terue если ни один из элементов последовательности не удовлетворяет условию
```pascal
begin
  var a := Arr(1,2,3,4);
  a.None(x -> x > 5).Print
end.
```


### Стандартная функция SetRandomSeed
Стандартная функция SetRandomSeed является синонимом Randomize с более понятным названием и инициализирует датчик псевдослучайных чисел, используя значение seed. 
При одном и том же seed генерируются одинаковые псевдослучайные последовательности.
```pascal
begin
  loop 10 do
    Print(Random(100));
  Println;
  Println;

  SetRandomSeed(122);
  loop 10 do
    Print(Random(100));
  Println;
  // Вторая последовательность с тем же самым seed совпадает с первой. Нужно для тестирования
  SetRandomSeed(122);
  loop 10 do
    Print(Random(100));
end.
```

Вывод:
```
44 7 94 96 68 97 43 25 7 5 

46 61 4 59 28 17 56 64 7 26 
46 61 4 59 28 17 56 64 7 26 
```




## Новые модули

### Новый модуль TurtleABC
Модуль `TurtleABC` является слабой но всё таки заменой модуля Turtle, но зато может использоваться под Linux

```pascal
uses TurtleABC;

begin
  Mark;
  SetScale(4);
  SetOrigin(5,-35);
  Mark; 
  SetColor(Color.Blue);
  Down;
  
  loop 4 do
  begin
    Forw(20);
    Turn(90);
  end;
  
  Up;
  MoveTo(-30, 30);
  Down;
  SetColor(Color.Red);
  
  Mark;
  loop 3 do
  begin
    Forw(15);
    Turn(120);
  end;
  
  Up;
  MoveTo(30, 40);
  Down;
  SetColor(Color.Green);
  
  Mark;
  loop 20 do
  begin
    Forw(30);
    Turn(100);
  end;
  
  Up;
  MoveTo(-30, 90);
  Mark;
  Down;
  SetColor(Color.Orange);
  
  loop 5 do
  begin
    Forw(30);
    Turn(144);
  end;
  
  DrawCoordPoints;
end.
```

<img width="1444" height="1152" alt="изображение" src="https://github.com/user-attachments/assets/6d2ecca8-7603-41ae-9211-2395e7fe9469" />


## Оптимизация производительности

### Ускорен CountOf
Оптимизирован и ускорен метод CountOf для строк


{% include links.html %}

