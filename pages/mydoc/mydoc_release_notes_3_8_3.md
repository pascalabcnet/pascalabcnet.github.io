---
title: Версия 3.8.3
keywords: release notes, what's new, announcements, new features
last_updated: 08.03.22
sidebar: mydoc_sidebar
permalink: mydoc_release_notes_3_8_3.html
toс: false
folder: mydoc
---

## Изменения в языке 3.8.3

### Цикл for с шагом
Цикл for допускает теперь указание шага step:

```pascal
begin
  for var i:=1 to 6 step 2 do
    Print(i);
  Println;
  for var c:='f' to 'a' step -2 do
    Print(c);
end.
```
Вывод:
```pascal
1 3 5 
f d b 
```
Для цикла for с шагом запрещен модификатор downto.

Нулевой шаг вызывает исключение ZeroStepException

### Цикл foreach c индексом
В цикле foreach допустимо использовать индекс:

```pascal
begin
  foreach var x in Arr(1,2,3) index i do
    Println(i,x);
end.
```
Вывод:
```pascal
0 1 
1 2 
2 3 
```

## Стандартная библиотека

### Cтандартная функция TypeName
В стандартном модуле появился стандартный поток ErrOutput для вывода шибок:

```pascal
begin
  var o: (integer,integer)->() := (x,y)->Print(1);
  Println(TypeName(o));
  var o1 := new List<integer>[2,3];
  Println(TypeName(o1));
end.
```
Вывод:
```pascal
array [,] of List<integer>
(integer,integer) -> () 
```

## Исправление важных ошибок

### Исправлен баг с перенаправлением ввода

[Баг и его исправление описаны здесь](https://github.com/pascalabcnet/pascalabcnet/issues/2647). 

Исправление бага позволяет решать интерактивные олимпиадны задачи, в т.ч. с сайта acmp.ru.

Спасибо Фёдору Меньшикову.






{% include links.html %}

