
## Интерполяционные многочлены

Пусть даны значения $f_k$ функции $f$ в конечном наборе точек $x_k$, $k=0..K$.
Задача интерполяции заключается в восстановлении значений функции в промежуточных точках.
Так как существует бесконечное число функций, принимающих данные значения, то восстановить значения однозначно невозможно. 
Однако если предположить, что функция имеет некий специальный вид, то оказывается возможным однозначно восстановить в промежуточных точках, т.е. построить интерполяционную функцию.
Из функционального анализа известно, что любую непрерывную функцию на отрезке можно приблизить многочленом, поэтому  в качестве интерполяционной функции естественно взять многочлен (однако это не обязательно лучший выбор). 
Интерполяционный многочлен степени $K$ можно записать в виде:
$$P(x)=a_0+a_1x+a_2x^2+\ldots+a_Kx^K,$$
где $a_k$ - коэффициенты многочлена.
Так как существует единственный многочлен степени $K$, проходящий через $K+1$ различных точек, то по данным $x_k$ и $f_k$ можно однозначно восстановить многочлен, т.е. найти коэффициенты $a_k$.
Мы можем исходить из того, что значения многочлена в точках $x_n$ равны $f_n$, что приводит нас к следующей системе:
$$\begin{cases}
f_0=P(x_0)=a_0+a_1x_0+a_2x_0^2+\ldots+a_Kx_0^K,\\
f_1=P(x_1)=a_0+a_1x_1+a_2x_1^2+\ldots+a_Kx_1^K,\\
\cdots\\
f_K=P(x_K)=a_0+a_1x_K+a_2x_K^2+\ldots+a_Kx_K^K.\\
\end{cases}$$
В матрично виде систему можно записать как $F=WA$,
где $A=(a_0,\ldots,a_K)^T$ - столбец неизвестных, 
$F=(f_0,\ldots,f_K)^T$ - столбец свободных членов, 
а $W$ - матрица системы, составленная из степенй чисел $x_k$:
$$W=\begin{pmatrix}
1 & x_0 & x_0^2 & \cdots & x_0^K \\
1 & x_1 & x_1^2 & \cdots & x_1^K \\
\cdots & \cdots & \cdots & \cdots & \cdots \\
1 & x_K & x_K^2 & \cdots & x_K^K 
\end{pmatrix}.$$
Матрица такого вида называется матрицей Вандермонда.
Для нахождения коэффициентов многочлена достаточно решить эту линейную систему, однако эта задача часто плохо обусловленна, что не позволяет нам точно восстановить коэффициенты.
Также решение системы требует больше вычислительных ресурсов, чем необходимо.

## Многочлены Лагранжа

Представленный выше вид многочлена $P$ в виде суммы степеней аргумента $x$ с коэффициентами $a_k$ называется разложением по одночленам.
Такой вид чаще всего встречается в литературе как определение многочлена.
Однако для вычислений могут оказаться более удобны другие представления многочлена,
например, если многочлен задан своими нулями $\xi_k$, $k=1..K$, то его удобнее представить в виде
$$P(x)=a_K(x-\xi_1)\cdots(x-\xi_K).$$
При построении интерполяционного многочлена по точкам $(x_k,f_k)$
удобно представить многочлен в виде [интерполяционного многочлена Лагранжа](https://ru.wikipedia.org/wiki/%D0%98%D0%BD%D1%82%D0%B5%D1%80%D0%BF%D0%BE%D0%BB%D1%8F%D1%86%D0%B8%D0%BE%D0%BD%D0%BD%D1%8B%D0%B9_%D0%BC%D0%BD%D0%BE%D0%B3%D0%BE%D1%87%D0%BB%D0%B5%D0%BD_%D0%9B%D0%B0%D0%B3%D1%80%D0%B0%D0%BD%D0%B6%D0%B0):
$$P(x)=\sum_{k=0}^K y_k l_k(x),$$
где се многочлены $l_k$ имеют степень $K$ и зависят только от узлов интерполяции $x_k$:
$$l_k(x)=\prod_{j\neq k}\frac{x-x_k}{x_j-x_k}.$$
Многочлены $l_k$ выбраны так, чтобы $l_k(x_k)=1$ и $l_k(x_j)=0$ для всех $j\neq k$.
Благодаря этому свойству легко находятся коэффициенты $y_k$ разложения по $l_k$:
$$P(x_j)=\sum_{k=0}^K y_k l_k(x_j)=y_j l_j(x_j)=y_j,$$
но так как по определение интерполяционного многочлена $f_j=P(x_j)$, то $y_j=f_j$.
Это значит, что построение интерполяционного многочлена Лагранжа производится за постоянное время по данным $f_k$. 
Однако вычисление многочленов $l_k$ в точках может потребовать больше времени, чем вычисление одночленов.
Чтобы ускорить расчеты мы можем разложить $l_k(x)$ по степеням $x$ и проссумировать их с коэффициентами $y_k=f_k$.

Если мы не знаем изначально, сколько нужно взять точек, чтобы получить хорошее прилижение, то мы можем последовательно увеличивать число узлов интерполяции, каждый раз строя многочлен Лагранжа по данным точкам, и проверяя точность приближения.
[Схема Эйткена](https://ru.wikipedia.org/wiki/%D0%A1%D1%85%D0%B5%D0%BC%D0%B0_%D0%AD%D0%B9%D1%82%D0%BA%D0%B5%D0%BD%D0%B0) дает эффективный способ делать это.

## Многочлены Чебышева

Многочлены Чебышева имеют вид
$$T_n(x)=cos(n\cdot\arccos x),$$
для $n=0,1,2,\ldots$.
Их также можно задать рекуррентно
$$T_0(x)=1,\quad T_1(x)=x,\quad T_{n+1}(x)=2xT_n(x)-T_{n-1}(x).$$
Нули многочлена $T_n(x)$ расположены в точках
$$\zeta_j^{(n)}=\cos\frac{\pi(k-1/2)}n.$$ 
Многочлены Чебышева ортогональны относительно скалярного произведения
\begin{equation}
\langle f_1|f_2\rangle=\sum_{j=1}^n f_1(\zeta_j^n)f_2(\zeta_j^n).
\end{equation}
Указанная ортогональность позволяет легко находить коэффициенты
разложения по многочленам Чебышева через скалярные произведения.
Построенное таким способом разложение, однако, не дает
наиболее аккуратного равномерного приближения.

Метод Ланцоша дает другой способ нахождения разложения
функции по полиномам Чебышева.
Рассмотрим сначала дифференциальное уравнение, решением
которого является искомый логарифм
$$x\frac{dy}{dx}-1=0,\quad y(1)=0.$$
Относительно переменной $u$ уравнение принимает вид
$$(1-4u^2/9)\frac{dy}{du}-4/3=0.$$
Для решения этого уравнения разложим производную решения
по многочленам Чебышева:
$$\frac{dy}{du}=\sum_{k=0}^Na_kT_k(x).$$
Подставляя разложение производной по многочленам в дифференциальное
уравнение и замечая, что
$$u^2T_k(u)=\frac14(T_{k-2}(u)+2T_k(u)+T_{k+2}(u)),$$
мы преобразуем дифференциальное уравнение в алгебраическое
$$\sum_{k=0}^{N+2}\tau_k T_k(u)=0,$$
для подходящих коэффициентов $\tau_k=\tau_k(a_0,\ldots,a_N)$.
Для нахождения разложения по Ланцошу необходимо решить
уравнения 
$$\tau_0=\tau_1=\cdots=\tau_N=0,$$
найдя тем самым $a_k$ и $dy/du$.
Интегрируя $dy/du$ можно найти разложение для $y(u)$,
воспользовавшись тождествами:
$$\int T_k(u)du=\frac1{2(k+1)}T_{k+1}(u)-\frac1{2(k-1}T_{k-1}(u)
+\frac{\sin(\pi k/2)}{k^2-1}.$$



```python

```