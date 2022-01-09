---
title: Версия 3.8.2
keywords: release notes, what's new, announcements, new features
last_updated: 21.10.21
sidebar: mydoc_sidebar
permalink: mydoc_release_notes_3_8_2.html
toс: false
folder: mydoc
---

## Изменения в языке

### Операция not in
Операция `not in` является противоположной операции `in`:

```pascal
begin
  var a := Arr(1,3,5);
  if 4 not in a then
    Print('отсутствует');
end.
```

## Уточнения в языке

### 

## Стандартная библиотека

### PartialSum для числовых последовательностей

Для числовых последовательностей метод PartialSum возвраает последовательность частичных сумм:

```pascal
begin
  Arr(1,2,3,4).PartialSum.Println;
  |1bi,2bi,3bi,4bi|.PartialSum.Println;
  Arr(1,2,3,4).Select(x->x+0.5).PartialSum.Println;
end.
```
Вывод:
```pascal
1 3 6 10
1 3 6 10
1.5 4 7.5 12
```



## Стандартные модули

### Стандартный модуль XLSX
Стандартный модуль XLSX предназначен для считывания файлов в формате XLSX.

Пример. 
```pascal
uses XLSX;

begin
  var d: Dictionary<string,array of array of string> := ReadXLSX('3.xlsx').Values.ToArray;
  
end.
```





{% include links.html %}

