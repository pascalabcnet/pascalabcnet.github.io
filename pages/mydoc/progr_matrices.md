---
title: Программы и алгоритмы для матриц
keywords: examples, programs, algorithms
last_updated: 31.12.19
summary: "Здесь приведены примеры программ и алгоритмов, используемых в курсе Основы программирования для студентов 1 курса ФИИТ мехмата ЮФУ"
sidebar: mydoc_sidebar
permalink: progr_matrices.html
folder: mydoc
---

<script src="//i.upmath.me/latex.js"></script>

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
