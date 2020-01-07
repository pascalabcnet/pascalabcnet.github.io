---
title: Программы и алгоритмы с использованием циклов
keywords: beginners, examples, programs, algorithms
last_updated: 31.12.19
tags: [getting_started]
summary: "Здесь приведены примеры программ и алгоритмов, используемых в курсе Основы программирования для студентов 1 курса ФИИТ мехмата ЮФУ"
sidebar: mydoc_sidebar
permalink: progr_loops.html
folder: mydoc
---

### Количество по условию
**Задача.** Сколько нечетных среди n введенных

**Решение 1.** Ввод данных в цикле
```pascal
var c := 0;
loop n do
begin
  var x := ReadInteger;
  if x mod 2 <> 0 then
    c += 1;
end;
```
**Решение 2.** Отделение ввода данных от обработки данных

Используем `ReadSeqInteger`, возвращающую последовательность введенных целых.
Далее используем цикл `foreach` по последовательности.
```pascal
var sq := ReadSeqInteger(n);
var c := 0;
foreach var x in sq do
  if x mod 2 <> 0 then
    c += 1;
```
### Защита от неверного ввода
**Решение 1.** Используем `TryRead` и цикл `while`
```pascal
var i: integer;
Print('Введите i (>0):');
while not TryRead(i) or not (i>0) do
  Print('Неверный ввод. Повторите:');
```
**Решение 2.** Используем цикл `repeat`
```pascal
var i: integer;
repeat
  Print('Введите i (>0):');
until TryRead(i) and (i>0);
```
### Табулирование функции 
**Задача.** Затабулировать функцию f(x) на [a,b] в точках, разбивающих [a,b] на n частей

**Решение 1.** С помощью `loop`
```pascal
Assert(n>0);
var h := (b-a)/n;
var x := a;
loop n+1 do
begin
  Println($'{x,5:f2} {Sin(x),9:f4}');
  x += h;
end;
```
**Решение 2.** С помощью `while`

Величину h/2 добавляем чтобы учесть влияние вычислительной погрешности
```pascal
var h := (b-a)/n;
var x := a;
while x <= b+h/2 do
begin
  Println($'{x,5:f2} {Sin(x),9:f4}');
  x += h;
end;
```

**Решение 3.** С помощью `PartitionPoints`

Функция `PartitionPoints` возвращает последовательность точек , разбивающих [a,b] на n частей
```pascal
foreach var x in PartitionPoints(a,b,n) do
  Println(x,f(x));
```

### Числа Фибоначчи
**Задача.** Вывести n первых чисел Фибоначчи

**Решение 1.** С помощью трёх переменных
```pascal
var n := ReadInteger;
var (a,b) := (1,1);
Print(a,b);
loop n-2 do
begin
  var c := a + b;
  Print(c);
  a := b;
  b := c;
end;
```

**Решение 2.** С помощью множественного присваивания
```pascal
var n := ReadInteger;
var (a,b) := (1,1);
Print(a,b);
loop n-2 do
begin
  (a,b) := (b,a+b);  
  Print(b);
end;
```

### Наибольший общий делитель
**Задача.** Найти НОД(a,b), используя алгоритм Евклида: 

    НОД(a,b) = НОД(b,a mod b);
    НОД(a,0) = a
 
**Решение 1.** С помощью трёх переменных
```pascal
var (a,b) := ReadInteger2;
repeat
  var c := a mod b;
  a := b;
  b := c;
until b=0;
Print(a);
```

**Решение 2.** С помощью множественного присваивания
```pascal
var (a,b) := ReadInteger2;
repeat
  (a,b) := (b,a mod b);
until b=0;
Print(a);
```

### Cумма цифр 
**Задача.** Найти сумму цифр целого положительного числа m

**Решение.** 
```pascal
var m := ReadInteger;
var s := 0;
while m>0 do
begin
  s += m mod 10;
  m := m div 10;
end;
```

### Максимумы и минимумы

**Задача.** Найти max из введенных чисел

**Решение 1.** Ввод данных в цикле
```pascal
var x := ReadReal;
var max := x;
loop n-1 do
begin
  x := ReadReal;
  if max < x then
    max := x;
end;
```

**Решение 2.**  C помощью `ReadSeqReal`
```pascal
var max := real.MinValue;
foreach var x in ReadSeqReal(n) do
  if x > max then
    max := x;
```

**Задача.** Найти минимальный элемент x, удовлетворяющий условию p(x)

**Решение 1.** Ввод данных в цикле
```pascal
var min := real.MaxValue;
loop n do
begin
  var x := ReadReal;
  if (x < min) and p(x) then
    min := x;
end;
if min = real.MaxValue then 
  Println('Нет элемента, удовлетворяющего условию');
```

### Суммирование рядов

**Задача.** Вычислить Σ <sub><i>i</i>=0..<i>n</i></sub> <i>a</i><sup><i>i</i></sup>/<i>i</i>!

**Решение.**
```pascal
Read(a, n);
var x := a;
var s := x;
for var i := 2 to n do
begin
  x *= a / i;
  s += x;
end;
```

### Предел последовательности

**Задача.** Вычислить lim <sub>n→∞</sub>⁡ a<sub>n</sub>=√x.

Для решения используется следующая рекуррентная последовательность:

<i>a</i><sub>1</sub>⁡=<i>x</i>,
<i>a</i><sub><i>n</i>+1</sub>⁡=1/2 * (<i>a</i><sub><i>n</i></sub>⁡+<i>x</i>/<i>a</i><sub><i>n</i></sub>⁡)   

**Решение 1.**
```pascal
Assert(x>=0);
var x := ReadReal;
var eps := 1e-10;
var a := x;
var b := real.MaxValue;
while Abs(b - a) >= eps do
begin
  b := a;
  a := (a + x/a) / 2;
end;
Print(a,Sqrt(x));
```

**Решение 2.** С помощью множественного присваивания
```pascal
var (a,b) := (x,real.MaxValue);
while Abs(b - a) >= eps do
  (b,a) := (a, (a + x / a) / 2);
```
### Поиск значения

**Задача.** Есть ли среди введенных число k?

**Решение 1.** Неэффективное - без break
```pascal
var Exists := False;
loop n do
begin
  var x := ReadInteger;
  if x = k then
    Exists := True;
end;
```
**Решение 2.** Эффективное - с break
```pascal
var Exists := False;
loop n do
begin
  var x := ReadInteger;
  if x = k then
  begin
    Exists := True;
    break;
  end;
end;
```

**Решение 3.** С использованием while
```pascal
var Exists := False;
var i := 1;
while (i<=n) and not Exists do
begin
  var x := ReadInteger;
  i += 1;
  if x = k then
    Exists := True;
end;
```

### Определение простоты числа

**Задача.** Является ли число n>1 простым?
```pascal
var IsPrime := True;
for var i:=2 to Round(Sqrt(n)) do
  if n mod i = 0 then
  begin
    IsPrime := False;
    break;
  end;
```

### Разложение числа на простые множители

**Решение.** 
```pascal
Assert(x>=2);
var i := 2;
repeat
  if x mod i = 0 then
  begin
    Print(i);
    x := x div i;
  end
  else i += 1;
until x = 1;
```

###  Корень функции на отрезке 

**Задача.** Дана непрерывная на [a,b] функция f(x), имеющая на (a,b) ровно один корень (f(a) * f(b) < 0). Найти его методом половинного деления

**Решение.** 
```pascal
Assert(b>a);  
var (fa,fb) := (f(a), f(b));
Assert(fa*fb < 0);  
while (b-a) > eps do
begin
  var x := (b + a)/2;
  var fx := f(x);
  if fx = 0 then
      break;
  if fa*fx < 0 then
    b := x
  else (a,fa) := (x,fx);
end;
Println((b + a)/2);
```


{% include links.html %}
