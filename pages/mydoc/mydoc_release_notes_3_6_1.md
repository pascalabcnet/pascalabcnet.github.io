---
title: Версия 3.6.1
keywords: release notes, announcements, what's new, new features
last_updated: 05.01.20
sidebar: mydoc_sidebar
permalink: mydoc_release_notes_3_6_1.html
toс: false
folder: mydoc
---

## Ускорение перенаправленного ввода

В новой версии ввод, перенаправленный из файла, станет работать в 3-6 раз быстрее (важно ждя олимпиадных задач).
Это будет сделано за счёт ручной буферизации ввода (автоматическая в .NET не работает или работает медленно). 

В частности, для сравнения приволится время работы кода в старой и новой версии:

|  | Старая версия | Новая версия |
|-------|--------|---------|
| ReadString | 1300 мс | 280 мс |
| loop N do ReadLexem | - | 370 мс |
| loop N do ReadInteger | 1570 мс | 280 мс |
| ReadString.ToIntegers | 1450 мс | 380 мс |

## Pred и Succ с двумя параметрами 

Функция `Pred(c,n)` возвращает символ с кодом на `n` меньше чем у символа `c`.

Функция `Succ(c,n)` возвращает символ с кодом на `n` больше чем у символа `c`.

## s.JoinToString

Старое название метода последовательности `s.JoinIntoString` будет заменено на `s.JoinToString`


{% include links.html %}
