---
title: Символы и строки в PascalABC.NET
keywords: strings, PascalABC.NET, Pascal, Паскаль
last_updated: 19.12.2020
sidebar: mydoc_sidebar
permalink: school_strings.html
toc: true
folder: mydoc
---

## Символы и основные операции над ними

Символы имеют тип char, занимают 2 байта и хранятся в кодировке Unicode (UTF-16).

```pascal
var c1: char;
var c2 := 'z';
```

Для преобразования символа `c` в код используется функция `Ord(c)`, для обратного преобразования кода i в символ используется функция `Chr(i)`.
```pascal
begin
  var c := 'ю';
  Print(Ord(c)); // 1102
  Print(Chr(1102)); // ю
end.
```


{% include links.html %}
