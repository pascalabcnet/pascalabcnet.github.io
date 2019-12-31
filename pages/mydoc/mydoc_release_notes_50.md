---
title: Версия 3.5.0
keywords: release notes, announcements, what's new, new features
last_updated: 25.05.19
sidebar: mydoc_sidebar
permalink: mydoc_release_notes_3_5.html
folder: mydoc
---

## Версия 3.5.0.2067 (25.05.19)


* Экспериментальный модуль OpenCL (разработчик Латченко Сергей aka @Sun_Serega)
*    Улучшения Pattern Matching - Pattern Matching с IList, кортежами, константами
*    Добавлены директивы компилятора {$Title} и {$Description}
*    В Graph3D учтён Autoreverse при анимации поворота
*    В модуле WPFObjects добавлен компонент TextWPF и реализовано выравнивание дочерних элементов
*    В компиляторе командной строки pabcnetclear добавлены директивы в командную строку и переработана диагностика ошибок
*    Увеличено разрешение иконки приложения
*    Реализованы f.Lines для текстового файла и f.Elements для бестипового
*    Задачник PT4 обновлен до версии 4.19
*    Реализованы f.ReadТип для базовых типов в бестиповых файлах, f.Rewrite и f.Append в текстовых и f.Reset(Encoding), f.Rewrite(Encoding) в бестиповых файлах
*    Кардинально улучшена скорость и плавность графики в GraphWPF, реализованы методы SetPixel, SetPixels и DrawPixels.
*    Более эффективные TakeLast и SkipLast
*    Реализован метод Product для последовательностей
*    Реализовано событие OnKeyPress для GraphWPF
*    Ряд улучшений при отображении в High DPI. В т.ч. Робот и Чертежник переведены на High DPI
*    Реализованы методы ReadBytes и WriteBytes для бестиповых файлов
*    При открытии типизированных и бестиповых файлов возможно указание кодировки (для типизированных - только однобайтовая)
*    В модуле PABCSystem методы расширения ...Indexes... замнены на ...Indices...
*    Реализованы автосвойства и расширенные индексные свойства
*    a.Print для простых переменных, массивов и матриц работает теперь и в задачнике PT4
*    Partition(a,b,n) переназван в PartitionPoints
*    Новые задания в задачнике PT4: ArrFilter, ArrSlice, ArrOp, ArrShift, ArrSlice, ArrTransf, ArrGen, ObjArray

{% include links.html %}
