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
  var (lg,tv,mg) := ReadXLSX('3.xlsx').Values.ToArray;
  var mgs := mg.Skip(1).Where(s->s[1]='Заречный').Select(s->s[0]).ToArray;
  var tvi := tv.Where(s->'Яйцо' in s[2]).First[0];
  var egg := lg.Where(s->(s[2] in mgs) and (s[3]=tvi));
  var sumInc := egg.Where(s->s[5]='Поступление').Sum(s->s[4].ToInteger);
  var decInc := egg.Where(s->s[5]='Продажа').Sum(s->s[4].ToInteger);
  Print(sumInc, decInc, sumInc-decInc);
end.
```





{% include links.html %}

