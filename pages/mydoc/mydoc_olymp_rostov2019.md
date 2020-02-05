---
title: Олимпиада Ростов 2019 
keywords: olympiads
last_updated: 12.01.2020
sidebar: mydoc_sidebar
permalink: olymp_rostov2019.html
toc: false
folder: mydoc
---

## Задачи муниципального этапа по программированию, Ростов, 2019 г.

Здесь приведены решения некоторых задач. Автор решений - М.Э.Абрамян.

[Формулировки задач](http://pascalabc.net/downloads/Olymp/Olymp2019Rostov.pdf)   

**Задача A**

```pascal
begin
  var cnt := ArrGen(3, i -> begin var r: int64; Read(r); result := r end);
  var a := ArrGen(6, i -> begin var r: int64; Read(r); result := r end);
  var min := int64.MaxValue;
  var max: int64 := 0;
  var num := 0;
  for var i := 0 to 5 do
  for var j := 0 to 5 do
  begin
    if j = i then continue;
    for var k := 0 to 5 do
    begin
      if (k = i) or (k = j) then
        continue;
      var res := Arr(a[i] div cnt[0], a[j] div cnt[1], a[k] div cnt[2]).Min;
      num += 1;
      Println(num, ':', i, j, k, ' -> ', res);
      if res < min then
        min := res;
      if res > max then
        max := res;
    end;
  end;
  Print(min, max);
end.

//2 8 4
//10 48 12 50 30 25
```

**Задача B**

```pascal
begin
  var w := ReadArrInteger(3);
  var c := ReadArrInteger(3);
  var r := ReadArrInteger(3);
  var res := ArrGen(3, i -> c[i]*r[i] - w[i]);
  var max := res.Max;
  if max <= 0 then
    Println(0)
  else
    for var i := 0 to 2 do
      if res[i] = max then
        Print(i+1);
end.

//30000 50000 100000 40 70 1000 1000 1000 90
//20000 40000 30000 100 200 300 150 100 50
//5000 5000 6000 60 60 70 100 100 100
```

**Задача D**

```pascal
begin
  var a:=new int64[26];
  var (n, m) := ReadlnInteger2;
  loop n do
  begin
    loop m do
      a[Ord(ReadChar)-Ord('A')] += 1;
    Readln;
  end;
  loop ReadlnInteger do
    foreach var c in ReadlnString do
      a[Ord(c)-Ord('A')] -= 1;
  for var i := 0 to a.Length-1 do
    loop a[i] do
      Write(Chr(Ord('A')+i));
end.

{
3 5
HEKEK
ELDLR
PLOWO
2
HELLO
WORLD

4 4
DLOL
FDON
SILT
AKEI
4 I
DONT
LIKE
DFS
}
```

**Задача F**

```pascal
begin
  var res:= new List<string>;
  var sum := 0.0;
  var price:= new Dictionary<string, real>;
  loop ReadlnInteger do
  begin
    var ss := ReadlnString.Split;
    var pr := StrToFloat(ss[1]);
    if ss[2] = '1' then
    begin
      res.Add(ss[0]);
      sum += pr;
    end
    else
      price[ss[0]] := pr;
  end;
  var b := ArrGen(ReadlnInteger,
    i -> begin var ss := ReadlnString.Split;
    result := (ss[0], ss[1]) end).ToList;
  var cur := 0;
  while cur < res.Count do
  begin
    foreach var e in b do
      if (res[cur] = e[1]) and not res.Contains(e[0]) then
      begin
        res.Add(e[0]);
        sum += price[e[0]];
      end;
    cur += 1;
  end;
  Println(res.Count);
  var resset := new HashSet<string>(res);
  // удаление пар, не связанных с результирующими городами
  for var i := b.Count - 1 downto 0 do
    if not resset.Contains(b[i][1]) then
      b.RemoveAt(i);
  var final := new List<string>;
  //список final заполняется с конца!
  while resset.Count > 0 do
  begin
    var tmpset := new HashSet<string>(resset);
    foreach var e in b do
      tmpset.Remove(e[0]);
    foreach var e in tmpset do
    begin
      final.Insert(0, e);
      resset.Remove(e);
      for var i := b.Count-1 downto 0 do
      if b[i][1] = e then
        b.RemoveAt(i);
    end;
  end;
  foreach var e in final do
  Println(e);
  Println(sum);
end.

{
7
Budapest 70.90 0
Riga 75.41 1
Berlin 95.12 0
Warsaw 77.77 1
Rome 100.25 1
Oslo 90.00 0
Paris 112.12 0
6
Budapest Riga
Riga Warsaw
Berlin Warsaw
Berlin Oslo
Berlin Paris
Oslo Paris

7
Budapest 70.90 0
Riga 75.41 1
Berlin 95.12 0
Warsaw 77.77 1
Rome 100.25 1
Oslo 90.00 0
Paris 112.12 0
2
Berlin Riga
Berlin Rome
}
```

