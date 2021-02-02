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

### Срезы многомерных массивов (пока не реализовано)

```pascal
begin
  var a := Matr(|1,2,3|,|4,5,6|);
  Print(a[1,1:],a[:,2]);
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

