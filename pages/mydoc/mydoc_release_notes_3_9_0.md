---
title: Версия 3.9.0
keywords: release notes, what's new, announcements, new features
last_updated: 08.05.23
sidebar: mydoc_sidebar
permalink: mydoc_release_notes_3_9_0.html
toс: false
folder: mydoc
---


## Изменения в языке 3.9.0

### Ковариантность параметров обобщенных типов

При неявном приведении типов учитывается ковариантность параметров обобщений из стандартной библиотеки .NET. Это означает, что если параметр обобщения - ковариантный, то вместо него можно использовать любой производный тип. Например:

```pascal
begin
  var a: sequence of object;
  var b: sequence of integer;
  a := b;
  a := Arr(1,2,3);
  a := new List<DateTime>;
end.
```

Реализация ковариантности позволила подключать пакет Microsoft.ML машинного обучения к PascalABC.NET. Для этого следует создать проект, добавить в него пакет Microsoft.ML. Вот пример простой программы, предсказывающей цену дома по известной площади на основании известных данных о продажах домов:

```pascal
uses Microsoft.ML;
uses Microsoft.ML.Data;
uses Microsoft.ML.Trainers;
uses Microsoft.ML.Transforms;

type 
  HouseData = auto class
  public  
    Size: single;
    Price: single;
  end;
  Prediction = class
  public  
    [ColumnName('Score')] 
    Price: single;
  end;
  
function HD(sz,pr: single) := new HouseData(sz,pr);  

begin
  var mlContext := new MLContext;
  var hData := Arr(HD(1.1,1.2),HD(1.9,2.3),HD(2.8,3.0),HD(3.4,3.7));
  var data := mlContext.Data.LoadFromEnumerable(hData);

  var opt := new SdcaRegressionTrainer.Options; 
  opt.LabelColumnName := 'Price'; // Зависимое значение
  opt.maximumNumberOfIterations := 100;
// Все независимые значения - Features - объединяются в массив
  var pipeline := mlContext.Transforms.Concatenate('Features', Arr('Size')) 
    .Append(mlContext.Regression.Trainers.Sdca(opt)); 
    
  var model: ITransformer := pipeline.Fit(data);
  
  var size := new HouseData(3.5,0);
  var engine := mlContext.Model.CreatePredictionEngine&<HouseData, Prediction>(model);
  var price := engine.Predict(size);
  
  Println(price.Price);
end.
```


### Именованные аргументы
При вызове подпрограмм теперь можно использовать именованные атрибуты. Они могут менять значения параметров по умолчанию.

```pascal
procedure p(s: string; x: integer := 1; y: integer := 2; z: integer := 3);
begin
  Print(s,x,y,z);
end;

begin
  p('Hello', z := 33, y := 22);
end.
```
Вывод:
```pascal
Hello 1 22 33 
```

## Linux версия IDE

Появилась Linux-версия IDE PascalABC.NET (пока без отладчика). В качестве просмотровщика справки используется kchmviewer.

В Alt Linux 10.1 PascalABC.NET включен в репозиторий

Из графических модулей под Linux работает GraphABC

## Новые стандартные модули

### Модуль LightPT

Модуль легковесного задачника LightPT обеспечивает бесшумную легковесную автоматическую проверку заданий, выполняемых учащимися. Этот модуль является лучшей альтернативой использованию систем олимпиадной проверки, позволяя преподавателю быстро разрабатывать 10-20 проверяемых заданий к каждому уроку и проводить контрольные работы с автоматической проверкой.

Для его работы в папке должен находиться текстовый файл lightpt.dat, единственная строка в котором - имя урока.

Также в папке должен находиться файл Tasks.pas, осуществляющий легковесную проверку заданий. Его примерное содержимое:

```pascal
unit Tasks;

{$savepcu false}

uses LightPT;

procedure CheckTaskT(name: string);
begin
  case name of
    'Var_4': begin 
      CheckData(Input := Empty);
      CheckOutput(25,3.14,100,0.75);
    end;  
   'Simple2': begin 
      FilterOnlyNumbers;
      CheckData(Input := |cInt|*2);
      CheckOutput(Int(0)+Int(1));
   end;
  'MinMax': begin 
    FilterOnlyNumbers;
    CheckData(Input := |cInt|*2);
    CheckOutput(Min(Int(0),Int(1)),Max(Int(0),Int(1)))
   end;
   'ArrSum': begin 
    FilterOnlyNumbers;        // Фильтрация только чисел – на случай вывода строк-подсказок 
    CheckData(Input := |cInt|*10); // Проверка типов и количества введенных элементов
    var a := IntArr(10);      // Считывание введенного массива целых
    var out := new ObjectList; // Подготовка списка для вывода
    out.AddRange(a);           // Добавление в него всех элементов массива a 
    out.Add(a.Where(x -> x mod 2 = 0).Sum); // Добавление в него суммы четных
    CheckOutputSeq(out);   // Проверка совпадения с выводом ученика      
    end;
  end;
end;

initialization
  CheckTask := CheckTaskT;
finalization
end.
```

Проверяемые задания должны находиться в файлах Simple2.pas, MinMax.pas, ArrSum.pas - возможно, пустых. Обычной практикой является формулировка задания в этом файле в виде комментария, например:


```pascal
{ Simple2.pas. Задание. Вводится 2 целых числа. Найти их сумму
}

{ MinMax.pas. Задание. Напишите программу, в которой вводятся числа x,y
    и выводятся их минимум и максимум 
}

{ ArrSum.pas. Задание. Дан массив 10 целых (введите его с клавиатуры или заполните случайными значениями). 
    Выведите его. Найдите сумму его элементов и также выведите
}
```

При выполнении задания под управлением LightPT легковесный задачник обеспечивает проверку задания и выдает предупреждения о частичном неправильном решении, например:
```pascal
Требуется ввести 2 значения

Ошибка вывода. При выводе 1-го элемента типа integer выведено значение типа real

Задание выполнено
```
Результаты запуска заносятся в локальный файл db.txt следующего формата:
```
Conf2023_MC Simple2 2023-03-30 14:41:43Z IOError OutputCount(0,1)
Conf2023_MC Simple2 2023-03-30 14:42:29Z Solved NoInfo
Conf2023_MC ArrSum 2023-03-30 16:12:03Z IOError InputCount(0,10)
Conf2023_MC ArrSum 2023-03-30 16:12:40Z BadSolution NoInfo
Conf2023_MC ArrSum 2023-03-30 16:12:15Z Solved NoInfo
```
Данный файл может быть легко обработан скриптом для сбора информации о заданиях, рещшенных учеником. 

Защиты от поддельных данных и обмана системы в данной версии не предусмотрено.

В модуль LightPT интегрированы Робот, Чертежник, задачник PT - задания, выполненные в них, автоматически фиксируются в файле db.txt.

### Модуль Мозаика

Модуль Мозаика предназначен для обучения дошкольников.

Пример программы:

```pascal
uses Мозаика;

begin
  Window.Title := 'цыплёнок';  

  НарисоватьРяд(П*9+кк2*2);
  НарисоватьРяд(П*9+ж*2+жк2);
  НарисоватьРяд(П*9+ж*2+ч+кк2);
  НарисоватьРяд(П*6+жк2+П*2+жк4+ж+жк3);
  НарисоватьРяд(П*5+жк2+ж+жк2+П*2+ж);
  НарисоватьРяд(П*5+ж*6);
  НарисоватьРяд(П*5+жк4+ж*5);
  НарисоватьРяд(П*6+жк4+ж*3+жк3);
  НарисоватьРяд(П*3+бк1+бк2+П*2+ж+П+ж);
  НарисоватьРяд(П*3+б*2+П*2+о+п+о);
  НарисоватьРяд(П*3+бк4+бк3+П*2+о+ок2+о+ок2);
  НарисоватьРяд(З*15);
end.
```
### Модуль TurtleWPF

Модуль TurtleWPF реализует черепашью графику на основе модуля GraphWPF

Пример программы:

```pascal
uses TurtleWPF;

begin
  Down;
  SetSpeed(11);
  SetColor(Colors.Red);
  for var i:=1 to 450 do
  begin
    SetColor(RGB(128+i,0,i));
    Forw(i);
    Turn(96);
  end;
end.
```


## Изменения в модулях

### Стандартный модуль 

1. a.Cartesian(n) переименован в a.CartesianPower(n). Теперь методы расширения Permutations, Combinations, CartesianPower возвращают последовательность массивов, а Cartesian - последовательность кортежей
2. Тип Convert сделан синонимом System.Convert
3. Permutations, Cartesian, CartesianPower, Combinations для строк
```pascal
begin
  'abc'.Permutations.Println;
  'abc'.Permutations(2).Println;
  'abc'.Combinations(2).Println;
  'abc'.CartesianPower(2).Println;
  'abc'.Cartesian('def').Println;
end. 
```
Вывод:
```pascal
abc acb bac bca cab cba
ab ba ac ca bc cb
ab ac bc
aa ab ac ba bb bc ca cb cc
(a,d) (a,e) (a,f) (b,d) (b,e) (b,f) (c,d) (c,e) (c,f)
```
4. x.Sqr возвращает теперь int64
5. Короткие создающие функции DictStr, DictInt, DictStrInt, LstLin, LstStr, HSetInt, HSetStr, SSetInt, SSetStr для создания пустых словарей, списков и множеств соответствующих типов
```pascal
##
var d := DictStr;
d['кошка'] := 'cat';
d.Println;
var L := LstInt;
L.Add(2); L.Add(3);
L.Println;
```
6. Dict для последовательностей пар 
```pascal
##
var pairs := Arr((1,'один'),(2,'два'));
var d := Dict(pairs);
Println(pairs);
Println(d);
```
Вывод:
```
[(1,один),(2,два)] 
{(1,один),(2,два)} 
```
7. Метод расширения последовательности Each с проекцией элемента на значение
```pascal
##
var dct := Arr(1..10).Each(x -> 0);
Print(dct); // словарь
```
Вывод:
```
{(1,0),(2,0),(3,0),(4,0),(5,0),(6,0),(7,0),(8,0),(9,0),(10,0)} 
```


### Модуль School 

Метод расширения целых Digits c параметром-основанием системы счисления и обратный метод lst.DigitsToInt64, преобразующий список в целое int64.
```pascal
##
uses School;
var lst := 1234.Digits(2);
lst.Println;
var n := lst.DigitsToInt64(2);
Println(n);
```
Вывод:
```
1 0 0 1 1 0 1 0 0 1 0 
1234
```

### Модуль Graph3D 

Реализованы методы графического объекта LocalAxisX, LocalAxisY, LocalAxisZ, MoveByLocal, позволяющие работать в локальных координатах объекта.

### Модуль Utils 

Реализована функция Benchmark для замера времени работы участка кода. Участок кода надо оформить в процедуру или передать в виде лямбда-выражения. По умолчанию берется среднее время за 100 запусков.

```pascal
uses Utils;

procedure Test1;
begin
  var n := 1000000;
  var s := 0.0;
  for var i:=1 to n do
    s += Sin(i);
end;

begin
  Benchmark(Test1).Println;

  var n := 100000000;
  var Sq := ArrRandomInteger(n,0,MaxInt-1);
  Benchmark(()->
  begin
    var Min := Sq.Min;
  end,1).Println;
end.
```

### Модуль GraphWPF 

Реализовано событие OnMouseWheel. Пимер иллюстрирует масштабирование изображения колёсиком мыши.

```pascal
uses GraphWPF;

var a := 10.0;

procedure Draw;
begin
  SetMathematicCoords(-a,a);
  var pp := |Pnt(4,-3),Pnt(-3,-1),Pnt(3,3)|;
  Polygon(pp,ARGB(50,255,0,0));
end;

begin
  Window.Title := 'Событие колёсика мыши';
  Window.SetSize(640,480);
  Draw;
  OnMouseWheel := delta -> begin
    a -= 0.3 * Sign(delta);
    a := a.Clamp(0.1,100);
    Draw;
  end;
end.
```

Реализован графический примитив Arrow и статический класс Parameters, содержащий в частности размеры наконечника стрелки по умолчанию.

```pascal
uses GraphWPF;

begin
  Parameters.ArrowSizeAcross := 4;
  Parameters.ArrowSizeAlong := 10;
  SetMathematicCoords;
  var (p1,p2,p3) := (Pnt(0,0),Pnt(2,3),Pnt(4,-1));
  Arrow(p1,p2);
  Arrow(p2,p3);
  Arrow(p3,p1);
end.
```

TextOut и DrawText - параметр text имеет теперь тип object - он преобразуется к строковому представлению

```pascal
uses GraphWPF;

begin
  SetMathematicCoords;
  var (p1,p2,p3) := (Pnt(0,0),Pnt(2,3),Pnt(4,-1));
  Circle(p1,0.1);
  Circle(p2,0.1);
  Circle(p3,0.1);
  TextOut(p1,p1);
  TextOut(p2,p2);
  TextOut(p3,p3);
end.
```

### Модуль Timers

Событие таймера TimerProc переименовано в OnTimer

## Оптимизация

1. Процедура Sort(a,x->x) ускорена в несколько раз

2. Ускорена функция Abs для целых

3. Атрибут [Cache] для одного параметра не оборачивает его в Tuple - за счет этого выполнение программы ускорено примерно в 10 раз

4. В атрибуте [Cache] Tuple заменен на ValueTuple, что позволило ускорить кеширование функций с двумя параметрами и более примерно в 3 раза

## Оболочка

### Цветовой вывод в окне вывода (Windows)

1. Использование указанных символьных констант переключает цвет вывода в окне вывода PascalABC.NET

```pascal
begin
  Writeln('123456789');
  Writeln(#65535'123456789'); // зеленый
  Writeln(#65534'123456789'); // красный
  Writeln(#65533'123456789'); // оранжевый
  Writeln(#65532'123456789'); // малиновый 
  Writeln(#65531'123456789'); // серый
end.
```

2. Новый файл теперь создается в папке файла из текущей вкладки

3.4Окно About - улучшено оформление и добавлена гипетрссылка на Telegram канал

5. При инсталляции nuget-пакета в окне проекта производится инсталляция сборок с учетом папки netstandard2.0
и копирование нативных dll из папок runtimes\win-x64\native\



{% include links.html %}

