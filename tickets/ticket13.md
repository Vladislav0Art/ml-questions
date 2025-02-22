## Предисловие

1. Смотри презентацию 4: [ML_04_VC_Dimension.pdf](https://docs.yandex.ru/docs/view?url=ya-disk-public%3A%2F%2FTce3Hg4R521%2FAeGvN14%2FuhhBJbYmfaf3PaCuY7embqZnn%2BiIO%2BBq00rZ5aTL40zE%2Bb3nCKLCVTJ%2BSInaOUvvHQ%3D%3D%3A%2F%D0%9B%D0%B5%D0%BA%D1%86%D0%B8%D0%B8%2FML_04_VC_Dimension.pdf&name=ML_04_VC_Dimension.pdf).
1. Смотри лекцию 6: [YouTube (01:11:45)](https://youtu.be/8RM6OYFjW1g?list=PLxMpIvWUjaJsttwLkYi-uEydy6R9Hk2-v&t=4305) (начало с 31:20; данная тема с 01:11:45).

**О чем тут речь:**

1. Что такое Perceptron.
1. Нер-во Вапника-Червоненкиса.
1. Точки поломки и дихотемии.

Данный билет связан с 12.1, так как использует некоторые его определения. Можно глянуть по диагонали. Снизу приведен контекст, необходимый для части 13.1.


### Perceptron (Перцептрон)

Это первая нейронная созданная сетка. Она решает задачу **линейной классификации множества точек**. Т.е., дано пространство с точками из **двух классов**, и нам нужно провести прямую (гиперплоскость) так, чтобы разбить это пространство на 2 кластера в соответствии с классами.

![Постановка задачи](./images/ticket-13/1.png)

#### Как определить принадлежность точки к классу

Перцептрон - это наша гиперплоскость. Тогда построим к ней нормаль и проведем проекцию от рассматриваемой точки до этой нормали. Если координата спроецированной точки больше координаты точки, полученной при проекции перцептрона, то один класс; иначе - второй.

![Как определить принадлежность точки к классу](./images/ticket-13/2.png)

Как решается задача нахождения весов для перцептрона:

![Постановка задачи](./images/ticket-13/6.png)

Т.е. $h(x)$ по $x$ определяет его принадлежность к классу $+1$ или $-1$. **Важно, что мы дописали свое слагаемое-свободный член в виде $w_0 x_0$**.


### Perceptron training

См. лекцию [34:26](https://youtu.be/8RM6OYFjW1g?list=PLxMpIvWUjaJsttwLkYi-uEydy6R9Hk2-v&t=2066) или [слайд 3](https://docs.yandex.ru/docs/view?url=ya-disk-public%3A%2F%2FTce3Hg4R521%2FAeGvN14%2FuhhBJbYmfaf3PaCuY7embqZnn%2BiIO%2BBq00rZ5aTL40zE%2Bb3nCKLCVTJ%2BSInaOUvvHQ%3D%3D%3A%2F%D0%9B%D0%B5%D0%BA%D1%86%D0%B8%D0%B8%2FML_04_VC_Dimension.pdf&name=ML_04_VC_Dimension.pdf).

Pocket algorithm и Feature engineering там же.

### Theory of error

![Theory of error](./images/ticket-13/7.png)

Определения:

1. $E_{in}(h)$ - это ошибка внутри выборки $X$, которое равно среднему по ошибке в точке. Т.е. доля примеров, на которых мы с нашей гипотезой $h$ ошиблись.

1. Ошибка в точке $e(h(x_n), f(x_n))$ равна либо $0$ (если наша гипотеза $h$ правильно классифицирована $x_n$), либо $1$, если неправильно.

1. $E_{out}(h)$ - матожидание ошибки по $x$ (его мы не знаем).


### Hoeffding's inequality

![Hoeffding's inequality](./images/ticket-13/8.png)

Данное нер-во описывает как хорошо у нас $E_{in}(h)$ приблизит $E_{out}(h)$.

Если много гипотез подставлять в неравенство, то мы должны умножать то, что справа, на $M$ - кол-во гипотез, которое мы перепробовали:

![Hoeffding's M inequality](./images/ticket-13/9.png)

Мы вводим понятие дехотемии:

![Дихотемии](./images/ticket-13/10.png)

Дехотемия - это почти как гипотеза $h$, но она определена только на множестве $x_i$ из датасета, а не на всем пространстве $X$. Это надо, чтобы $M$ в нер-ве выше ограничить.

**$m_{H}(N)$ = max кол-во дихотемий на $N$ точках при классе гипотез $H$.**

Далее определяем **точку поломки** (**breakpoint**), как минимальное кол-во точке $x_i$, для которых (при их хитром расположении) нельзя построить дихотемию, которая бы правильно их определяла по их классам $y_i$:

![breakpoint](./images/ticket-13/11.png)

Зная $m_H(N)$ от _Hoeffding's inequality_ мы переходим к _Vapnik–Chervonenkis Inequality (VC inequality)_:

![Vapnik–Chervonenkis Inequality](./images/ticket-13/12.png)

Далее мы показывали, что $M_H(N)$ полиномиально зависит от $N$, что дает хорошую оценку в нер-ве при больших $N$.

_Теперь у нас есть вся база для части 13.1_.

---

## 13.1 Размерность Вапника-Червоненкиса. VC-размерность для перцептрона, доказательство.

**Def**: Размерность Вапника-Червоненкиса (_VC-dimention_) $d_{VC}(H)$ для гипотез типа $H$ — это максимальное $N$ такое, что $m_H(N) = 2^N$.

При этом, есть явная формула для $d_VC(H)$ (за 1 шаг до точки поломки):

$$ d_{VC}(H) = k - 1 $$

Где $k$ - это _точка поломки_.

### Подсчет размерности Вапника-Червоненкиса для перцептрона

Пусть $d$ - размерность рассматриваемого пространства. Тогда размерность Вапника-Червоненкиса для перцептрона равна:

$$ d_{VC} = d+1 $$

**Док-во:**

См. лекцию на [01:13:21](https://youtu.be/8RM6OYFjW1g?list=PLxMpIvWUjaJsttwLkYi-uEydy6R9Hk2-v&t=4401).

Чтобы это доказать, нужно показать 2 свойства:

1. **(1): Доказать, что $d+2$ - это точка поломки**.
1. **(2): Доказать, что для $d+1$ все возможные _дихотамии_ получаются (их всего $2^{d+1}$)**.

**Док-во для (2):**

Напомним, что размерность перцептрона (т.е. его весов) равна $d$. Построим матрицу размера $d+1$ x $d+1$, где первый столбец - $x_0$, который есть добавочный член в веса перцептрона (см. лекцию 6 [31:28](https://youtu.be/8RM6OYFjW1g?list=PLxMpIvWUjaJsttwLkYi-uEydy6R9Hk2-v&t=1888), минуты 3-4):

![Матрица](./images/ticket-13/3.png)

Теперь мы хотим, чтобы наш перцептрон правильно классифицировал заданные точки (т.е. надо $w$ правильно подобрать). Мы это определяли, как равенство знака. Тогда решим такое уравнение:

$$y \in \{1, -1\}$$

$$ sign(Xw) = y $$

Усложним себе задачу и положим не равенство знака классу точки (т.е. $y$), а просто равенство:

$$ Xw = y $$

Так как $X$ мы определили как матрицу выше, и данная матрица обратима (т.к. нижнетреугольная), то $w$ явно выражается:

$$ w = X^{-1} y $$

Так мы построили решение (т.е. нахождение $w$) для любого переданной дихотемии $y$ в пространстве размера $d+1$.



**Док-во для (1):**

Отвечаем на вопрос, почему нельзя получить все возможные дихотемии на $d+2$ точках.

При такой размерности мы получаем матрицу размера $(d+1)$ x $(d+2)$:

![Матрица](./images/ticket-13/4.png)

Тогда в этой матрице есть линейная зависимость:

![Матрица](./images/ticket-13/5.png)

$$ x_j = \sum_{i!=j} x_i a_i $$

Сейчас мы явно построим пример, на котором мы не сможем решить уравнение со знаком выше ($sign(Xw)=y$).

Домножим на $w^T$:

$$ w^T x_j = \sum_{i!=j} w^T x_i a_i  \ \ (*) $$

Теперь сконструируем дихотемию, которая не получится ни при каком $w$:

Положим $y_i := sign(a_i)$ и $y_j := -1$.

Предположим, что мы нашли такой $w$, что он решает уравнение **(*)**, тогда:

$$ sign(Xw) = y \implies sign(w^T x_i) = y_i = a_i $$

Получаем, что $w^T x_i$ и $a_i$ одного знака, тогда наша сумма в **(*)** всегда положительна. При этом, $w^T x_j = y_j = -1 \implies$ левая сторона всегда отрицательна $\implies$ противоречие с тем, что $w$ - решение.

Получаем, что заданный $y$ - это дихотемия, которая не получается никаким $w$ $\implies$ $d+2$ - это точка поломки.






## 13.2 Глобальный поиск. Случайный поиск, grid search, случайное блуждание.

Смотри презентацию [ML_12_Bayes_and_DFO.pdf](https://docs.yandex.ru/docs/view?url=ya-disk-public%3A%2F%2FTce3Hg4R521%2FAeGvN14%2FuhhBJbYmfaf3PaCuY7embqZnn%2BiIO%2BBq00rZ5aTL40zE%2Bb3nCKLCVTJ%2BSInaOUvvHQ%3D%3D%3A%2F%D0%9B%D0%B5%D0%BA%D1%86%D0%B8%D0%B8%2FML_12_Bayes_and_DFO.pdf&name=ML_12_Bayes_and_DFO.pdf&nosw=1) (слайды 22-24)


Смотри [лекцию 13](https://drive.google.com/drive/folders/10gvaRHs_NQW4v5ULc5b6L83-pPs2zq6g) (с 59:00 по 01:04:06).

Задача: научиться минимизировать/максимизировать функцию неградиентным способом. Рассматриваем 3 способа:

### Случайный поиск

Случайно кинули точки, посмотрели значение функции и выбрали минимум:

![Матрица](./images/ticket-13/13-2/1.png)


### Grid search

Кидаем точки по сетке не случайно, а по сетке. Не для всех задача такой способ минимизации подходит. Например, в задаче про коммивояжёра мы имеем пространство перестановок, на котором с данным способом минимум не найти.

![Матрица](./images/ticket-13/13-2/2.png)



### Cлучайное блуждание

Случайно выбираем направление (чаще фиксированного) шага с текущей позиции и обновляем эту самую позицию.

![Матрица](./images/ticket-13/13-2/3.png)