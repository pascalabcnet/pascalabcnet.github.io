---
title: Программы и алгоритмы с использованием массивов
keywords: beginners, examples, programs, algorithms
last_updated: 31.12.19
tags: [getting_started]
summary: "Здесь приведены примеры программ и алгоритмов, используемых в курсе Основы программирования для студентов 1 курса ФИИТ мехмата ЮФУ"
sidebar: mydoc_sidebar
permalink: progr_arrays.html
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


{% include links.html %}
