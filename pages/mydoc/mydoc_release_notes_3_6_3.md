---
title: Версия 3.6.3
keywords: release notes, announcements, what's new, new features
last_updated: 05.01.20
sidebar: mydoc_sidebar
permalink: mydoc_release_notes_3_6_3.html
toс: false
folder: mydoc
---
## Разрешены вложенные безымянные записи с некоторыми ограничениями

Во вложенных безымянных записях разрешено захватывать внешние имена только из глобального контекста:
```
type int = integer;

var globalr := 2.5;
var globali: integer;

type Class1<T,T1> = class
  procedure p;
  var
    A: record
      i: int := globali;
      jlocal0: record
        k: real := globalr;
      end;
    end;
  begin
    
  end;  
end;  
  
begin
  
end.
```
Однако следующая программа является ошибочной (захватывается неглобальный тип T):
```
type Class1<T> = class
    A: record
      i: T;
      end;
end;  
  
begin
  
end.
```
Ошибка: Безымянные записи не могут захватывать имена из неглобального контекста


## Pred и Succ с двумя параметрами (не реализовано)

Функция `Pred(c,n)` возвращает символ с кодом на `n` меньше чем у символа `c`.

Функция `Succ(c,n)` возвращает символ с кодом на `n` больше чем у символа `c`.

**Примечание** Определиться с поведением функции `Pred` при получении отрицательного значения. Определиться с поведением функции `Succ` при получении значения, большего 65535 для Unicode.

## Расширенный foreach по последовательности кортежей и по словарям (не реализовано)
```
foreach var (i, v)
```

{% include links.html %}
