# Лабораторная работа 3 "Растровая развертка отрезка"

### В ходе выполнения практической части этой лабораторной ра¬боты необходимо выполнить следующие пункты задания:
1. Реализовать алгоритмы ЦДА, Брезенхема (действительный, целочисленный, с устранением ступенчатости), By.
2. Сравнить визуально отрезки, построенные в соответствии с каждым алгоритмом, а также с отрезком, построенным процедурой языка высокого уровня. Проверить попадание отрезка в заданную конечную точку.
3. Определить время, затрачиваемое на построение отрезка по каждому из алгоритмов.
4. Для заданного алгоритма получить зависимость длины максимальной ступеньки от угла наклона отрезка и отобразить ее в виде графика или гистограммы.
Программы, реализующие алгоритмы, должны обеспечивать построение произвольного отрезка, то есть располагающегося в любом из восьми октантов, включая горизонтальный и вертикальный, а также предусматривать высвечивание точки в случае вырож¬денного отрезка. Визуальное сравнение отрезков, построенных по разным алгоритмам, можно осуществить либо высвечиванием близко расположенных параллельных отрезков, либо рисованием на одном и том же месте одного отрезка разными алгоритмами. При этом каждый раз новый отрезок высвечивается новым цветом. В случае не¬совпадения результатов не все старые пиксели высветятся новым цветом, что и будет свидетельствовать о различающихся результа¬тах. Проверку попадания отрезка в требуемую конечную точку мож¬но осуществить путем сравнения цвета конечной точки с цветом рисования отрезка.
Исследование временных характеристик построения отрезков в силу высокой скорости построения одного отрезка следует прово¬дить путем замера времени, затрачиваемого на высвечивание нес¬кольких десятков отрезков (например, пятидесяти или ста отрез¬ков). При этом целесообразно сначала установить текущее время в ноль, а затем снять показания текущего времени. При этом доста¬точно учитывать значения сотых, десятых долей и целых значений секунд, так как в силу высокого быстродействия процесс рисова¬ния не превысит минуты. Если же не устанавливать предварительно время в ноль, то необходимо будет учитывать возможное изменение минут и даже часов.
Для выполнения последнего пункта задания достаточно иссле¬довать отрезки, расположенные в пределах одного октанта. При этом ступеньку будут образовывать те пиксели, у которых значе¬ние одной из координат остается неизменным при изменяющейся другой координате. Построенный график (гистограмма) должны и¬меть обозначение и оцифровку осей координат


### Рализация

![7](https://user-images.githubusercontent.com/62243773/161815699-b86dc755-c91d-453f-94a3-c19f9949d466.png)

### Исследование времени

![image](https://user-images.githubusercontent.com/62243773/161815539-7a595b07-ebe0-40cc-9a75-ccd9ceb077e4.png)

### Исследование ступенчатости

![6](https://user-images.githubusercontent.com/62243773/161815669-b9be8e25-b6ce-412c-8d52-1975d3701b61.png)

### Требования

- 1 - Отрезок должен выглядеть как отрезок прямой, начинаться и заканчиваться в заданных точках
- 2 -Интенсивность (яркость) вдоль отрезка должна быть постоянной. Отрезки, имеющие разные углы наклона, должны быть одной интенсивности. Восприятие человека зависит не только от интенсивности свечения объекта, но и от расстояния между светящимися объектами //чтобы удовлетворить этому требованию, надо высвечивать точки с переменной интенсивностью от расстояния – потребует дополнительных вычислений, без особой нужды не используется
- 3 - Алгоритмы (особенно нижнего уровня) должны работать быстро

## Теория и вопросы

### Растровая развертка отрезка (разложение в растр)

Процесс определения пикселей, наилучшим образом аппроксимирующих заданный отрезок. Простейшие случаи – горизонтальный, вертикальный и под 45°. В большинстве случаев проявляется лестничный (ступенчатый) эффект.
Все алгоритмы имеют пошаговый характер – на очередном шаге высвечиваем пиксель, и производим вычисления, используемые в следующем шаге.

### Цифровой дифференциальный анализатор
Использует достаточно общий принцип, известный в математике: изучение какого-либо явления на основе дифференциального уравнения или системы таких уравне¬ний, описывающей это явление.
Поскольку прямая линия на плоскости описывается уравнением вида
```
AX+BY+C=0, где A,B,C - коэффициенты этого уравнения, то производная dy/dx = const.
```
Заменив дифференциалы конечными разностями, получим
```
dy    Yk - Yн
-- = --------
dx    Xk - Xн
```
где `Xн,Yн и Xк,Yк` - координаты начальной и конечной точек отрезка

Ордината очередного пикселя Yi+1 может быть вычислена по известной ординате предыдущего пикселя Yi следующим образом:
```
Y_i+1 = Yi + dY
```
При условии заданности двух точек высчитывается delta(y) или delta(x) и округляетя. Если угол наклона <45, то |dX|=1, если > то |dY|=1

#### Как в ЦДА мы выбираем ближайший пиксель?
За счет округления по одной из координат, а по второй шаг равен 1

Другая формулировка 1: На каждом шаге увеличиваем предыдущее значение. Округляем до ближайшего целого, получаем пиксел, центр которого находится на наименьшем удалении от идеального отрезка.

Другая формулировка 2: В алгоритме ЦДА берется пиксель с минимальным расстоянием от своего центра до идеальной прямой.(именно поэтому мы производим округление при рисовании точки)

#### Почему ЦДА работает медленнее Брезенхема?
Недостатки – работает с целочисленной арифметикой (координаты текущей точки). Работает медленнее за счет операции округления.

### Алгоритм Брезенхема float
Ошибка еi – величина изменяющаяся на каждом шаге, расстояние между идуальным отрезком и ближайшей точкой растра.
0 <= ei <= 1, если е < 0.5, то ордината пикселя не меняется, Если > 0.5, то выбирается верхний пиксель (yi+1 = yi + 1). Начальное значение ошибки e = m, где m – тангенс угла наклона. У yид ордината идеального отрезка, в вещественных а не целых координатах.

Если на очередном шаге мы корректируем значение у, то мы должны также скорректировать и ошибку.

Допускается начальное e=m-0.5 и дальше сравнивать ошибку с нулем, а не с 0.5. (С 0 сравнение идет быстрее)

Недостатки - не все переменные являются переменными целого типа (e, m - действительные).

### Алгоритм Брезенхема integer

Для перехода к алгоритму работающему только с целыми мы ошибку умножаем на 2dx

### Алгоритм Брезенхема c устранением ступенчатости (сглаживание)

Используется при отображении ребёр многоугольника, который закрашивается. Идея состоит в сглаживании резких переходов от ступени к ступени. Сглаживание основывается на том, что каждый пиксель высвечивается со своим уровнем интенсивности. Уровень выбирается пропорционально площади части пикселя. 1 пиксель – квадрат с единичной стороной, а не математическая точка.

Так как интенсивность I~Si площади, то

1. отрезок связан (покрывает) на i шаге с одним пикселем. Обозначим Yi расстояние по вертикали от точки пересечения отрезка с пикселем, до левой нижней границы пикселя. Обозначим тангенс угла наклона отрезка через m, тогда Si = Sпр+Sтр = Yi1 + 1m/2 = Yi + m/2

### Ву
[Wikipedia VU](https://ru.wikipedia.org/wiki/%D0%90%D0%BB%D0%B3%D0%BE%D1%80%D0%B8%D1%82%D0%BC_%D0%92%D1%83)

## Возможные вопросы

### Что такое разложение отрезка в растр?
Процесс определения пикселов, наилучшим образом аппроксимирующих заданный отрезок, называется разложением в растр.

### В каком режиме, какой характер имеют алгоритмы разложения?
Пошаговый.

### А что это означает?
Мы не вычисляем сразу координаты всех пикселей отрезка. Результат следующего пикселя зависит от прошлого.

Другая формулировка: Можем вычислить значение следующего пиксела на основе полученного на предыдущем шаге.

#### Как в ЦДА мы выбираем ближайший пиксель?
За счет округления по одной из координат, а по второй шаг равен 1

Другая формулировка 1: На каждом шаге увеличиваем предыдущее значение. Округляем до ближайшего целого, получаем пиксел, центр которого находится на наименьшем удалении от идеального отрезка.

Другая формулировка 2: В алгоритме ЦДА берется пиксель с минимальным расстоянием от своего центра до идеальной прямой.(именно поэтому мы производим округление при рисовании точки)

### Почему ЦДА работает медленнее Брезенхема?
Так как на каждой итерации в нем происходит округление вещественных величин, а это затратная по времени операция.

### Что такое ошибка?
Ошибкой называется расстояние между точкой идеального отрезка и текущим пикселом, который ее аппроксимирует.

### В каком диапазоне она лежит и как помогает выбрать пиксел?
При первом пикселе - 0 (по определению).

Если мы определяем ошибку как dy/dx, то ошибка принимает значения от 0 до 1. Если e < 1/2, то выбираем пиксел с той же ординатой, что у предыдущего, если е >= 1/2 - пиксел с ординатой на единицу больше.

Но при реализации в ЭВМ удобнее анализировать не само значение ошибки, а ее знак, поэтому ошибку мы определяем, как dy/dx - 0.5.
Если e < 0, то выбираем пиксел с той же ординатой, что у предыдущего, если e >= 0 - пиксел с ординатой на единицу большей.

### Зачем нужна строка e -= 0.5? (в брензенхеме с вещественными коэфф-тами зачем мы от тангенса угла наклона отнимаем 0.5?)
См. выше:

> При реализации в ЭВМ удобнее анализировать не само значение ошибки, а ее знак, поэтому ошибку мы определяем, как dy/dx - 0.5.
Если e < 0, то выбираем пиксел с той же ординатой, что у предыдущего. Если e >= 0, то выбираем пиксел с ординатой на единицу большей, чем у предыдущего пиксела.

### Сколько вариантов выбора пикселей есть при построении отрезка и какой будет лучший?
2 варианта; зависит от расстояния между точкой идеального отрезка и текущим пикселем. Выберем пиксел, который будет ближе к идеальному отрезку.

### Как Брезенхем предложил формализовать реальную ситуацию?
Он предложил ввести понятие ошибки, которая будет означать расстояние от идеальной точки до пикселя; если размер ошибки больше, чем 0.5, то выбираем следующую ординату (верхнюю).

### Какие отрезки мы рассматриваем (с произвольным углом наклона или нет)?
С конкретным углом наклона. Угол наклона от 0 до 45° (тангенс угла наклона не больше 1).
