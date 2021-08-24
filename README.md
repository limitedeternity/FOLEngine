# FOLEngine

> – Программа, написанная на языке C++ (-std=c++17), которой можно передавать утверждения и запросы, представленные в логике первого порядка. 
> Для выполнения запросов используется *метод резолюции*.

### Метод резолюции

* [Википедия](https://ru.wikipedia.org/wiki/%D0%9F%D1%80%D0%B0%D0%B2%D0%B8%D0%BB%D0%BE_%D1%80%D0%B5%D0%B7%D0%BE%D0%BB%D1%8E%D1%86%D0%B8%D0%B9#%D0%9C%D0%B5%D1%82%D0%BE%D0%B4_%D1%80%D0%B5%D0%B7%D0%BE%D0%BB%D1%8E%D1%86%D0%B8%D0%B8)
* [Resolution Theorem Proving (First Order Logic - Cornell CS)](https://www.cs.cornell.edu/courses/cs4700/2011fa/lectures/16_FirstOrderLogic.pdf)
* [Resolution (Technische Universität München)](https://www21.in.tum.de/teaching/logik/SS16/Slides/resolution-fol.pdf)

### Тестовый пример 1

1. Лиз – мать Чарли.
2. Чарли – отец Билли
3. Если *x* – мать *y*, тогда *x* – один из родителей *y*
4. Если *x* – отец *y*, тогда *x* – один из родителей *y*
5. Если *x* – родитель *y*, тогда *x* – предок *y*
6. Если *x* – родитель *y* и *y* – предок *z*, тогда *x* – тоже предок *z*.

Эти высказывания можно представить в логике первого порядка следующим образом:

1. `Mother(Liz,Charley)`
2. `Father(Charley,Billy)`
3. `∀x,y (Mother(x,y) → Parent(x,y))`
4. `∀x,y (Father(x,y) → Parent(x,y))`
5. `∀x,y (Parent(x,y) → Ancestor(x,y))`
6. `∀x,y,z ((Parent(x,y) ∧ Ancestor(y,z)) → Ancestor(x,z))`

Затем из выражений удаляются кванторы, после чего первые можно передавать программе:

```
!- Mother(Liz,Charley)
!- Father(Charley,Billy)
!- Mother(x,y) => Parent(x, y)
!- Father(x,y) => Parent(x, y)
!- Parent(x,y) => Ancestor(x, y)
!- Parent(x,y) & Ancestor(y,z) => Ancestor(x,z)
```

Передадим интересующие нас запросы:

```
?- Ancestor(Liz,Billy)
?- Ancestor(Liz,Bob)
```

И получим вывод:

```
Y:\FOLEngine\Debug>FOLEngine.exe ..\test1.txt
Ancestor(Liz,Billy) :: YES
Ancestor(Liz,Bob) :: NO
```

### Тестовый пример 2

```
!- Cat(x) => Animal(x)
!- Dog(x) => Animal(x)
!- Cat(Tuna)
!- Dog(D)
!- Owns(Jack,D)
!- Kills(Jade,Tuna)
!- Animal(y) & Owns(x,y) => AnimalLover(x)
!- Animal(y) & Owns(x,y) => HasOwner(y)
!- ~(AnimalLover(x) & Animal(y) & Kills(x,y))

# Комментарий 
?- AnimalLover(Jade)
?- HasOwner(D) 
```

Ответ:

```
Y:\FOLEngine\Debug>FOLEngine.exe ..\test2.txt
AnimalLover(Jade) :: NO
HasOwner(D) :: YES
```

