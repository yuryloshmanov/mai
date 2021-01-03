#№ Отчет по лабораторной работе №2
## по курсу "Логическое программирование"

## Решение логических задач

### студент: Лошманов Ю.А.

## Результат проверки

| Преподаватель     | Дата         |  Оценка       |
|-------------------|--------------|---------------|
| Сошников Д.В. |              |               |
| Левинская М.А.|              |      5-       |

> *Комментарии проверяющих (обратите внимание, что более подробные комментарии возможны непосредственно в репозитории по тексту программы)*
Добавьте в отчет запрос и ответ к задаче.

## Введение

Существует множество методов решений логических задач, таких как метод рассуждений, таблиц истинности, логики высказываний, блок-схем и других.

Prolog удобен для решения логических задач, так как он позволяет их решать "в лоб", основываясь на изначально известных фактах. Такие решения не претендуют на красоту и эффективность, зато их проще понять и воплотить.

## Задание

  15. Корнеев, Докшин, Мареев и Скобелев  жители нашего города. Их профессии  пекарь, врач, инженер и милиционер. Корнеев и Докшин  соседи и всегда на работу ездят вместе. Докшин старше Мареева. Корнеев регулярно обыгрывает Скобелева в пинг-понг. Пекарь на работу всегда ходит пешком. Милиционер не живет рядом с врачом. Инженер и милиционер встречались один-единственный раз, когда милиционер оштрафовал инженера за нарушение правил уличного движения. Милиционер старше врача и инженера. Определите, кто чем занимается.

## Принцип решения

Объявим известные факты:

```prolog
neighbours('Корнеев', _, 'Докшин', _).

rides('Корнеев', _).
rides('Докшин', _).

walks(_, пекарь).

older('Докшин', _, 'Мареев', _).
older(_, милиционер, _, врач).
older(_, милиционер, _, инженер).

metManyTimes('Корнеев', _, 'Докшин', _).
metManyTimes('Корнеев', _, 'Скобелев', _).

metOnce(_, инженер, _, милиционер).
```


Будем производить полный перебор всех жителей и всех возможных профессий этих жителей, так как факты завязаны не только на фамилиях или профессиях.

```prolog
permute1([K, D, M, S], [пекарь, врач, инженер, милиционер]),
permute1([Baker, Doctor, Engineer, Policeman], ['Корнеев', 'Докшин', 'Мареев', 'Скобелев']),
```


Далее, будем исключать варианты, которые точно не могут быть. Например, мы точно знаем, что Корнеев и Докшин не ходят, а ездят на работу:

```prolog
not(walks('Корнеев', K)),
not(walks('Докшин', D)),
```


Полный предикат `solve` будет выглядеть так:

```prolog
solve :-
    permute1([K, D, M, S], [пекарь, врач, инженер, милиционер]),
    permute1([Baker, Doctor, Engineer, Policeman], ['Корнеев', 'Докшин', 'Мареев', 'Скобелев']),

		not(walks('Корнеев', K)),
    not(walks('Докшин', D)),
    
    not(rides(Baker, пекарь)),
	
    not(older('Мареев', M, 'Докшин', D)),
		not(older(Doctor, врач, Policeman, милиционер)),
    not(older(Engineer, инженер, Policeman, милиционер)), 

    not(neighbours(Policeman, милиционер, Doctor, врач)),

    not(metManyTimes(Engineer, инженер, Policeman, милиционер)),

		not(metOnce('Корнеев', K, 'Скобелев', S)),
		not(metOnce('Корнеев', K, 'Докшин', D)),

    write('Корнеев - '), write(K), nl,
    write('Докшин - '), write(D), nl,
    write('Мареев - '), write(M), nl,
    write('Скобелев - '), write(S), nl, !.
```

Решение неэффективно, так как оно использует полный перебор. То есть её сложность равна `O((n!)^2)`, где `n` - количество жителей, оно же и количество профессий.

Решение безопасно, так как используется предикат перебора `permute` со заданными списками, которые не расширятся в процессе, и используется отсечение после первого верно найденного ответа.

Решение непротиворечиво, так как используется отсечение.

## Выводы

В этой лабораторной работе я впервые применил язык программирования для решения логической задачи. Как ни странно, пролог идеально для этого подошёл. Такие мощные его возможности, как полный перебор, исключение и отсечение помогают коротко и красиво решать логические задачи.