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
    [ColumnName('Score')] // Непонятно, почему Score. Без этого не работает
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


### Именованные аргументы при вызове подпрограмм
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

### Модуль легковесного задачника LightPT автоматической проверки заданий

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
   'Simple2': begin 
      FilterOnlyNumbers;
      CheckInput(Arr(cInt)*2);
      CheckOutputNew(Int(0)+Int(1));
   end;
  'MinMax': begin 
    CheckInputCount(2);
    FilterOnlyNumbers;
    CheckOutputNew(Min(Int(0),Int(1)),Max(Int(0),Int(1)))
   end;
   'ArrSum': begin 
    CheckInput(Arr(cInt)*10); // Проверка типов и количества введенных элементов
    FilterOnlyNumbers;        // Фильтрация только чисел – на случай вывода строк-подсказок 
    var a := IntArr(10);      // Считывание введенного массива целых
    var out := new ObjectList; // Подготовка списка для вывода
    out.AddRange(a);           // Добавление в него всех элементов массива a 
    out.Add(a.Where(x -> x mod 2 = 0).Sum); // Добавление в него суммы четных
    CheckOutputSeqNew(out);   // Проверка совпадения с выводом ученика      
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

### Модуль Мозаика для дошкольников

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
### Модуль TurtleWPF, реализующий черепашью графику с использованием GraphWPF

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

1. a.Cartesian(n) переименован в a.CartesianPower(n)
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

Метод расширения целых Digits с основанием системы счисления
```pascal
##
uses School;
1234.Digits(2).Println;
```
Вывод:
```
1 0 0 1 1 0 1 0 0 1 0 
```

### Модуль Graph3D 

Методы графического объекта LocalAxisX, LocalAxisY, LocalAxisZ, MoveByLocal

### Модуль Utils 

Benchmark

### Модуль GraphWPF 

Событие OnMouseWheel

Графический примитив Arrow

TextOut и DrawText - параметр text имеет теперь тип object - можно выводить кортежи и автоклассы

### Модуль Timers

Событие таймера TimerProc переименовано в OnTimer


## Оптимизация

Sort(a,x->x) - ускорен в несколько раз

Ускорена функция Abs для целых

[cache] для одного параметра не оборачивает его в Tuple - за счет этого ускорена в 10 раз

В [cache] Tuple заменен на ValueTuple (ускорить кеширование функций с 2 параметрами и более примерно в 3 раза)

## Оболочка

### Цветовой вывод в окне вывода (Windows)

Новый файл теперь создается в папке файла из текущей вкладки

Окно About - улучшено оформление и добавлена гипетрссылка на Telegram канал

При инсталляции nuget-пакета в окне проекта производится инсталляция сборок с учетом папки netstandard2.0
и копирование нативных dll из папок runtimes\win-x64\native\








{% include links.html %}

