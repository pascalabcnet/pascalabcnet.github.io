---
title: Версия 3.8.1
keywords: release notes, what's new, announcements, new features
last_updated: 21.08.21
sidebar: mydoc_sidebar
permalink: mydoc_release_notes_3_8_1.html
toс: false
folder: mydoc
---

## Уточнения в языке

### Возможность описания подпрограмм в коротких программах 
```pascal
##
function Add(a,b: integer) := a + b;

Print(Add(2,3);
```

### Директива {$string_nullbased+} и срезы
Директива {$string_nullbased+} теперь воздействует на срезы
```pascal
{$string_nullbased+}
begin
  var s := 'Hello Pascal.ABC';
  Print(s[0:5]); // 'Hello'
end.
```
Сравним со старым кодом:
```pascal
begin
  var s := Seq(('Умнова',16),('Иванов',23),
               ('Попова',17),('Козлов',24));
  Println('Совершеннолетние:');
  s.Where(x -> x[1] >= 18).Println;
  Println('Сортировка по фамилии:');
  s.OrderBy(x -> x[0]).Println;
end.
```

## Уточнения в языке

### Разрешена конструкция a as array of T

Ранее конструкция `a as array of T` была запрещена на уровне грамматики. Таким образом, невозможно было выполнить DownCast привести к типу массива.
Теперь это ограничение снято

```pascal
begin
  var ob: object := new integer[2,3];
  var a := ob as array [,] of integer;
end.  
```

{% include links.html %}

