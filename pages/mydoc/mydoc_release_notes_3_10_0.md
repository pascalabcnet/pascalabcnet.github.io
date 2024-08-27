---
title: Версия 3.10.0
keywords: release notes, what's new, announcements, new features
last_updated: 26.08.24
sidebar: mydoc_sidebar
permalink: mydoc_release_notes_3_10_0.html
toс: false
folder: mydoc
---


## Изменения в языке 3.10.0 (26.08.24)

### Разделители _ в числовых константах

В любых числовых константах можно использовать разделители _. Их использование позволяет не запутаться в количестве разрядов в числах с большим количеством цифр
```pascal
##
var n := 10_000_100;
var m := 1_234.567_8;
var b := 1_000_000_000_000bi; 
```

### Многострочные строковые литералы

Многострочные строковые литералы заключаются в ''' В них могут находиться ' и ''

```pascal
begin
  var s := ''' 
  Многострочная строка
  Открывающие и закрывающие кавычки должны быть каждая на своей строке
  Лидирующие пробелы слева от закрывающих кавычек удаляются во всех строках многострочной строки
  Внутри могут быть апострофы ' и '' но не три апострофа подряд
  ''';
  Write(s, '!');
end.
```

### **1 to 2** - новый оператор в стиле Котлина для пары Ключ-Значение

Операция **to** позволяет построить пару KeyValuePair для словарей.

```pascal
var d := Dict('cat' to 'кошка', 'dog' to 'собака');
```

## Новые директивы
- Директива HiddenParams для скрытых модулей

## IDE
- Реализован отладчик под Linux
- Незначительное улучшение: возможность открывать гиперссылки из окна вывода

## Новые стандартные модули

### Модуль WPF

Данный модуль предназначен для создания оконных WPF-приложений из кода. Он содержит все типы WPF в "чистом виде", а также ряд сервисных функций для быстрого создания наиболее распространенных элементов.

**Пример 1.** Парсинг XAML в качестве содержимого окна. 
```pascal
uses WPF;

begin
  var s := '''
  <StackPanel Margin="30" Background="Aquamarine">
    <TextBox Height="23" TextWrapping="Wrap" Text="TextBox"/>
    <TextBlock/>
  </StackPanel>
  '''; 
  var scene := StackPanel(ParseXaml(s)).AsMainContent;
  var tb := scene.Children[0] as TextBox;
  var tbl := scene.Children[1] as TextBlock;
  
  tb.TextChanged += procedure (o,e) -> (tbl.Text := tb.Text);
end.
```

**Пример 2.** Изменеие свойств графического объекта элементами управления

```pascal
uses WPF;

begin
  MainWindow.Title := 'Rectangle свойства';
  MainWindow.FontSize := 14;
  MainWindow.WindowStartupLocation := WindowStartupLocation.CenterScreen;
  var mainpanel := CreateDockPanel(Margin := 5).AsMainContent;
  var leftpanel := CreateStackPanel(Margin := 0, Width := 150)
    .With(Background := Brushes.Bisque).AddTo(mainpanel,Dock.Left);
  var rect := CreateRectangle(Width := 300, Height := 200).With(HA := HA.Center, VA := VA.Center).AddTo(mainpanel);
  rect.Fill := Brushes.Blue;
  var widthslider := CreateSlider(50,500,10,50);
  var tbwidth := CreateTextBlock();
  var heightslider := CreateSlider(50,500,10,50);
  var tbHeight := CreateTextBlock();
  var radiusslider := CreateSlider(0,50,10,50);
  var tbRadius := CreateTextBlock();
  
  leftpanel.AddElements(widthslider, tbwidth, heightslider, tbHeight, radiusslider, tbRadius);

  widthslider.ValueChanged += procedure(o,e) -> tbwidth.Text := 'Ширина: ' + widthslider.Value.ToString(0);
  heightslider.ValueChanged += procedure(o,e) -> tbheight.Text := 'Высота: ' + heightslider.Value.ToString(0);
  radiusslider.ValueChanged += procedure(o,e) -> begin
    tbRadius.Text := 'Радиус скругления: ' + radiusslider.Value.ToString(0);
    rect.RadiusX := radiusslider.Value;
    rect.RadiusY := radiusslider.Value;
  end; 
  radiusslider.Value := 10;

  widthslider.SetBinding(Slider.ValueProperty, rect, 'Width');
  heightslider.SetBinding(Slider.ValueProperty, rect, 'Height');
end.
```

### Модуль Turtle.pas (полностью обновлён)
- Осуществляется просмотр виртуального экрана с перемещением окна отображения мышью и изменением масштаба колёсиком мыши.
- Режим отображения координат переключается тройным нажатием пробела.
- Осуществляется рисование набора точек функцией DrawPoints
- Черепаха может рисовать окружности

**Пример.** Рисование кривой Гильберта.

```pascal
uses Turtle;

procedure Hilbert(level: integer; angle,step: real);
begin
  if level = 0 then
    exit;
  TurnRight(angle);
  Hilbert(level-1, -angle, step);
  
  Forw(step);
  TurnLeft(angle);
  Hilbert(level-1, angle, step);
  
  Forw(step);
  Hilbert(level-1, angle, step);
  
  TurnLeft(angle);
  Forw(step);
  Hilbert(level-1, -angle, step);
  TurnRight(angle);
end;

begin
  SetWidth(2);
  ToPoint(-9,-9);
  Down;
  Hilbert(6,90,0.3);
end.
```
Отображение:

![111](https://github.com/user-attachments/assets/b7c3ae0b-7a18-42b2-9d9a-702bb0615edb)

**Пример.** Рисование массива точек.

```pascal
uses Turtle;

begin
  Window.Title := 'DrawPoints';
  var n := 1000;
  var d := 10;
  var a := ArrRandomReal(n,-d,d);
  DrawPoints(a[::2],a[1::2]);
  a := ArrRandomReal(n,-d,d);
  DrawPoints(a[::2],a[1::2]);
end.
```

![222](https://github.com/user-attachments/assets/b9e0cc10-8b7d-499a-9546-7d8b6c79ffe6)

## Изменения в модулях

### Системный модуль PABCSystem - новые функции

#### Метод расширения строк s.ToLines

#### Функция RealRange

Функция `RealRange` возвращает последовательность вещественных значений между двумя данными с данным шагом. Важной её особенностью является то, что она не подвержена риску округления вещественных чисел. Так,

```pascal
RealRange(1,3,0.5).Print
```
напечатает
```
1 1.5 2 2.5 3
```
и выпадения последней точки вследствие ошибок округления не произойдёт.

#### Метод расширения последовательностей Scan 

Метод `Scan` выполняет то же, что и `Aggregate`, но возвращает элемент новой последовательности после обработки каждого элемента исходной последовательности.

```pascal
begin
  var a := ArrRandom(10,1,9);
  a.Println.PartialSum.Println;
  a.Scan(min).Println;
  a.Scan(max).Println;
  a.Scan((x,y) -> x+y).Println;
  var s := 'abcdef';
  s.Scan('',(x,y) -> x+y).Println;
end.
```


#### Внешние функции Zip и Cartesian 

Внешние функции `Zip` и `Cartesian` реализованы для 2-5 параметров и с проекцией на значение

```pascal
begin
  Zip(1..5,2..6).Println;
  Zip(1..5,2..6,(x,y) -> x+y).Println;
  Zip(1..5,2..6,3..7).Println;
  Zip(1..5,2..6,3..7,(x,y,z) -> x+y+z).Println;

  Cartesian(1..4,3..5).Println;
  Cartesian(1..4,3..5,(x,y) -> x*y).Println;
  Cartesian(1..2,4..5,6..7).Println;
  Cartesian(1..2,4..5,6..7,(x,y,z) -> x*y*z).Println;
  
  foreach var (a,b) in Cartesian(1..2,3..5) do
    Print($'{a}+{b}')
end.
```

#### Функция SetOf и метод расширения ToSet

Функция SetOf и метод расширения ToSet введены как синонимы для HSet и ToHashSet

```pascal
begin
  var s := SetOf(1,2,3);
  var s2 := SetOf(3,4,5);
  var symmDiff := (s - s2) + (s2 - s);
  Print(symmDiff);
end.
```

```pascal
begin
  var x := 1668922;
  var y := 90192697672838;
  var s1 := x.ToString.ToSet;
  var s2 := y.ToString.ToSet;
  Println(x <= y);
end.
```

#### Функция Pair

Функция Pair введена как синоним функции KV и возвращает пару KeyValuePair<K,V>

```pascal
var d := Dict(1 to 2, 3 to 4);
var d1 := Dict(Pair(1,2),Pair(3,4);
```
#### Новые конструкторы, методы и операции для словарей

`d := Dict(d1)` - копия словаря
`d := Dict(keys,values)` - построение словаря по ключам и значениям
`d.Update(d1)` - обновляет данные словаря d данными словаря d1. Пары с новыми ключами добавляются, с таким же значением ключа - обновляются.
`d += d1` - синоним `d.Update(d1)`
`d + d1` - возвращает новый словарь, который получается обновлением словаря d словарем d1
`d -= Seq(ключ)` - исключает из словаря пары с указанным значением ключа
`d - Seq(ключ)` - возвращает новый словарь с исключенными из первого словаря парами с указанными значениями ключа

Интегральный пример:

```pascal
begin
  // Новая операция 1 to 2 - синоним KV(1,2)
  var d := Dict('cat' to 'кошка', 'dog' to 'собака');
  // Новая функция Pair - синоним KV
  var d0 := Dict(Pair('cat', 'кот'), Pair('camel', 'верблюд'));
  // Более сложная структура
  var dd := Dict('Иванов' to |5, 3, 3|, 'Петров' to |4, 4|);
  // Копия словаря
  var d1 := Dict(d);
  // Инициализация массивами ключей и значений
  var d2 := Dict(|11, 22|, |333, 444|);
  // Обновление значений и добавление новых
  d.Update(d0);
  // То же
  d += d0;
  // Слияние словарей
  var d3 := d + Dict('world' to 'мир', 'cloud' to 'облако');
  Println(d3);
  // Вычитание пар с данными ключами
  d3 -= Seq('cat', 'camel');
  Println(d3);
  d3 := d3 - 'world';
  Println(d3);
end.
```



### Системный модуль - незначительные улучшения
- SeqRandomReal - добавлен параметр digits, определяющий количество цифр после десятичной точки
- MatrRandomReal - добавлен параметр digits, определяющий количество цифр после десятичной точки
- dict.GeT(key,default) - добавлен параметр default
- _ObjectToString переименован в ObjectToString
- Zip как синоним ZipTuple
- Улучшенный вывод Complex

### LightPT 
- Появилась возможность генерировать тесты с помощью GenerateTests.
- CheckOutputNew и CheckOutputSeq заменены на CheckOutput

### School
- Калькулятор IP
- s.translate
- PrimeDivisors


## Оптимизация
- Драматически **ускорено кортежное присваивание**. В нескольких ситуациях количество присваиваний - оптимальное.
В некоторых тестах кортежное присваивание опережает аналогичное на C# в 2 раза: https://t.me/pascalabc_official/626


## Исправления
- Исправлена ошибка вывода лишнего пробела при s.Println







{% include links.html %}

