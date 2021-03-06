---
title: Версия 3.7 
keywords: release notes, what's new, announcements, new features
last_updated: 24.08.20
sidebar: mydoc_sidebar
permalink: mydoc_release_notes_3_7.html
toс: false
folder: mydoc
---

## Новые конструкции в языке

### Unicode-символ →

Вместо лексемы `->` в лямбда-выражениях можно использовать Unicode-символ `→`

Чтобы его набрать в редакторе, следует нажать Alt и набрать 26 на дополнительной клавиатуре.

Программы с лямбда-выражениями благодаря этому становятся более читабельными:

```pascal
begin
  ArrGen(10,i → i*i).Print
end.  
```

### Массивы-значения

Для формирования массива из значений используется конструкция

```pascal
begin
  var a := |1,3,5,7,9|;
  a.Println;
  var aa := ||1,2,3|,|4,5,6|,|7,8,9||; // массив массивов
  var aa1 := ||1,2|,|4|*3,|0|*0|; // последний массив - пустой!
 
  var b := 10 * |0|; // массив из 10 нулей
  var m := Matr(|1,2,3|,|4,5,6|,|7,8,9,10|); // матрица. Недостающие элементы заполняются нулями
end.  
```

В массивах-значениях допускается использование различных типов: при использовании целых и вещественных типов результат приводится к вещественному, при использовании символов и строк результат приводится к строковому, при использовании объектов базового и производного классов результат приводится к базовому:

```pascal
##
var a := |1,2.5,3|;
var b := |'z','sss','q'|;
var c := |new Person, new Student|;
```

### Распаковка в переменные из последовательности 

Можно распаковвывать значения в переменные из последовательности так, как это сейчас осуществляется для кортежей. Несовпадение размеров при этом проверяется на этапе выполнения, а не на этапе компиляции.

```pascal
begin
  var a := Arr(1..3);
  var (x,y,z) := a;
  (x,y,z) := a;
  
  var aa := ArrRandomInteger(10);
  var (min,min2) := aa.Order.Distinct;
  Print(min,min2);
  
  (min,min2) := SSet(aa);  
  Print(min,min2);
end.
```

### Запись программ без внешнего begin-end

Для записи программ без внешнего begin-end используется префикс ##

При этом способе записи невозможно использовать описание констант, типов (в т.ч. классов), процедур и функций

Область использования данного способа записи - **короткие программы, состоящие из нескольких команд**:

```pascal
##
var a := ArrRandomInteger;
a.Println;
a := a[1:] + |a[0]|;
```

При использовании префикса ### дополнительно подключается модуль SF:

```pascal
###
Pr(RI+RI)
```

### Лямбда-выражение с явно указанными типами может присваиваться переменной без явного указания типа

Раньше лямбда-выражение можно было присвоить только переменной, у которой тип явно определён:

```pascal
var p: procedure := procedure -> begin
  Print(1);
end;

var f: (integer,integer)->integer := function(a,b: integer): integer -> a + b;
```

Теперь тип переменной в этих случаях можно не указывать:
```pascal
var p := procedure -> begin
  Print(1);
end;

var f := function(a,b: integer): integer -> a + b;
```

Заметим, что эта возможность работает только с лямбда-выражениями, начинающимися с `procedure` или `function`.


## Уточнения в языке

### Символы в case по строкам

В case по строкам теперь в качестве констант выбора допустимы символы:

```pascal
var s := ReadString;
case s of
'1': Print(1);
'2': Print(2);
end;
```

## Дополнения и изменения в стандартных библиотеках

### Модуль Controls

Модуль Controls полностью переработан, включает набор основных элементов управления WPF и может сочетаться с модулями GraphWPF, WPFObjects и Graph3D

В папке PABCWork.NET/Samples/Graph/Controls - более 20 новых примеров. 

Пример программы с Controls:

```pascal
uses GraphWPF,Controls;

begin
  Window.Title := 'Цвета';
  Font.Size := 40;
  LeftPanel(150);

  var r := Slider('Красный: ',0,255,255,16);
  var g := Slider('Зеленый: ',0,255,255,16);
  var b := Slider('Синий: ',0,255,255,16);
  
  Button('Выход').Click := procedure → Window.Close;

  var p: procedure := () → begin
    var c := RGB(r.Value,g.Value,b.Value);
    Window.Clear(c);
    DrawText(GraphWindow.ClientRect,$'R={c.R}, G={c.G}, B={c.B}');
  end;
  r.ValueChanged := p;
  g.ValueChanged := p;
  b.ValueChanged := p;
  p;
end.
```

### Стандартные функции SeekEof SeekEoln Eoln

Добавлены стандартные функции `SeekEof` и `SeekEoln` для совместимости с Delphi

Исправлено поведение стандартной `Eoln` - теперь на конце файла она возвращает True

### Стандартные функции и операции для матриц и одномерных массивов

Для массивов и матриц добавлены новые операции и методы

```pascal
x in matr

MatrEqual(m,m1)
m.MatrEqual(m1)

ArrEqual(a,a1)
a.ArrEqual(a1)

MatrByRow, MatrByCol - создают матрицу из последовательности
```

Изменено поведение методов `a.Rows` и `a.Cols` - теперь они возвращают массив массивов

Это позволяет просто решать задачи с матрицами, где надо выполнить какие-то операции со строками-столбцами. 
Для этого мы вначале расщепляем матрицу на строки (столбцы), делаем с ними нужные преобразования и потом снова собираем матрицу.

**Пример**. Упорядочить все столбцы матрицы по сумме столбца

```pascal
##
var a := MatrRandom(3,4);
a.Println;
a := MatrByCol(a.Cols.OrderBy(col->col.Sum));
a.Println;
```

### Тип RealRange

Добавлен стандартный тип RealRange
```pascal
if x in 2.5..3.4 then
  Print(1);
```

### Стандартные функции DictStr и DictStrInt

`DictStr` и `DictStrInt` генерируют Dictionary<string,string> и Dictionary<string,integer> соответственно. Могут вызываться с пустым списком параметров

### Изменения в GraphWPF

При вызове `DrawGraph` теперь рисуется сетка, оси, заголовок и значения на осях (автоматически)

Имя типа `FontType` заменено на `FontOptions`

`DrawText` и `TextOut` могут вызываться с дополнительным параметром `FontOptions`

Методы `Font.WithSize`, `Font.WithStyle`, `Font.WithColor`, `Font.WithName` возвращают изменённый шрифт

Добавлен класс `Vector` и операции над ним и точкой, а также создающая функция `Vect`

Метод `Line` можно вызывать в виде `Line(p1,p2)`

Метод `Lines(n)`

Функции `RandomPoint` и `RandomPoints(n)`, возвращающие случайную точку и набор `n` случайных точек 


## IDE

###  В IDE интегрирована библиотека NUnit 

Простейшее unit-тестирование можно осуществить если набрать, например, программу

```pascal
uses NUnitABC;

[Test]
procedure Test1;
begin
  Assert.AreEqual(3,3);
end;

[Test]
procedure Test2;
begin
  Assert.Contains(2.5,Arr(1,2,3));
end;

function IsPrime(n: integer) := (2..n-1).All(i -> n mod i <> 0);

[TestCase(2)]
[TestCase(3)]
[TestCase(5)]
procedure Test3(value: integer);
begin
  Assert.IsTrue(IsPrime(value));
end;

begin
end.
```

и выбрать из меню "Запустить модульные тесты"

###  Улучшена подсветка форматных строк (реализовано @Kotov)

###  В заголовке IDE отображается версия компилятора

Заголовок IDE теперь имеет вид 

```
PascalABC.NET 3.7
```

## Другое

###  Определён глобальный символ PASCALABC

Это позволяет писать программы, ориентированные на различные компиляторы:

```pascal
begin
  {$ifdef PASCALABC}
  Print(1);  
  {$endif}
  {$ifdef FPC}
  fpc specific 
  {$endif}
end.
```

###  Оптимизирована распаковка из кортежа констант в переменные

Код

```pascal
begin
  var (a,b) := (1,2);
  var c: integer;
  var d: char;
  (c,d) := (3,'4');
end.
```

раскрывается в 

```pascal
begin
  var a := 1;
  var b := 2;
  var c,d: integer;
  c := 3;
  d := '4';
end.
```



{% include links.html %}

