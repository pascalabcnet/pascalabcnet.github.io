---
title: Версия 3.7.3 (предполагаемые изменения)
keywords: release notes, what's new, announcements, new features
last_updated: 02.02.21
sidebar: mydoc_sidebar
permalink: mydoc_release_notes_3_7_3.html
toс: false
folder: mydoc
---

## Новые конструкции в языке

### Срезы многомерных массивов (реализовано)

```pascal
begin
  var m := MatrByRow(||1,2,3,4|,|5,6,7,8|,|9,10,11,12||);
  Println(m[:,:]);           // ||1,2,3,4|,|5,6,7,8|,|9,10,11,12||
  Println(m[::1,::1]);       // ||1,2,3,4|,|5,6,7,8|,|9,10,11,12||
  Println(m[1:3,1:4]);       // ||6,7,8|,|10,11,12||
  Println(m[::2,::3]);       // ||1,4|,|9,12||
  Println(m[::-2,::-1]);     // ||12,11,10,9|,|4,3,2,1||
  Println(m[^2::-1,^2::-1]); // ||7,6,5|,|3,2,1||
  Println(m[:^1,:^1]);       // ||1,2,3|,|5,6,7||
  Println(m[1,:]);           // |5,6,7,8|
  Println(m[^1,:]);          // |9,10,11,12|
  Println(m[:,^1]);          // |4,8,12|
end.  
```


### Литеральные константы для массивов [1,2,3], определяемые по контексту

```pascal
procedure p(a: array of integer);
begin
end;

begin
  p([1,2,3]);
end.
```

{% include links.html %}

