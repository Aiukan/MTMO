Методы и технологии машинного обучения.

Репозиторий для лабораторных работ по дисциплине "Методы и технологии машинного обучения".
Вариант 12.

Лабораторная работа №1: Оценка точности модели снепрерывной зависимой переменной
В практических примерах ниже показано:
как делить данные на выборки (обучающую и тестовую);
как считать MSE: среднеквадратическую ошибку модели;
как меняются MSE на тестовой и обучающей выборках с изменением гибкости (числа степеней
свободы) модели.
Модели: сглаживающие сплайны.
Данные: сгенерированные.

Задача 1. На данных своего варианта повторить расчёты и построить графики из первой лабораторной. 
Пояснить выбор наилучшего количества степеней свободы.

Задача 2. Повторить расчёты, меняя характеристики согласно своему варианту. 
Проанализировать, как меняется MSE при изменении характеристик.

Задача 1: Решение.
1. Зададим исходные данные и функцию. Построим по ним обучающую и тестирующую выборки. Представим исходные данные на графике.
2. Построим при помощи библиотеки R сплайны на обучающей выборке для df = 38. Проверим MSE значение для обучающей и тестирующей выборок. Представим полученный сплайн на графике с исходной функцией.
3. Опробуем разные значения df [2, 40] и найдем такое, при котором тестовое MSE значение будет наименьшим. Представим на графике обучающие и тестовые MSE значения для разных значений df.

Задача 1: Результаты.
1. При переборе разных значений df мы определили, что для  функции f(X) = 18 - 0.1 * x лучшее значение df = 2. Это значение является интуитивным, так как функция представляет собой прямую, для описания которой достаточно только двух точек. При df > 2 сплайн будет фокусироваться только на обучающих данных и вредить представлению тестовых данных.
2. При df = 2 обучающее MSE близко к 1, что выражает заданное отклонение sigma = 1. Тестовое MSE равно 1.13, что соответствует sigma^ = 1.06.

Задача 2: Решение.
1. Повторим перебор df значений [2; 40] из Задачи 1 для разных значений sigma [1.5, 2, 2.5]. Полученные значения сохраним в фрейм (формой 38x6), и выберем для каждого sigma такое df, чтобы тестовое MSE было наименьшим. Представим графики обучающих и тестовых MSE для разных df и для разных sigma, выделяя точки с наименьшим тестовым MSE.

Задача 2: Результаты.
1. Для всех значений sigma при увеличении df обучающая MSE будет понижаться. Несмотря на тренд роста тестового MSE (видно при sigma = 2.0, sigma = 2.5), фактически значение может также понижаться (sigma = 1.5), что означает что обучающая выборка достаточно хорошо представляет тренировочные значения.
2. Соответственно, лучшим значением df в общем случае является df = 2 (sigma = 2.0, sigma = 2.5), в некоторых случаях минимум может достигаться и при других значениях, например df = 26 (sigma = 1.5).
3. Для sigma = 1.5, полученное минимальное тестовое MSE = 3.49, что соответсвутет sigma^ = 1.87.
4. Аналогично, для sigma = 2.0, полученное минимальное тестовое MSE = 5.42, что соответсвутет sigma^ = 2.33.
4. Аналогично, для sigma = 2.5, полученное минимальное тестовое MSE = 6.40, что соответсвутет sigma^ = 2.53.



Вариант 12 Лабораторная работа №2. Линейные модели. Кросс-валидация.
В практических примерах ниже показано:
как пользоваться инструментами предварительного анализа для поиска линейных взаимосвязей;
как строить и интерпретировать линейные модели с логарифмами;
как оценивать точность моделей с перекрёстной проверкой (LOOCV, проверка по блокам).

Модели: множественная линейная регрессия
Данные: https://raw.githubusercontent.com/ania607/ML/main/data/Carseats.csv
Проверка методом K-FOLD, K-VAL = 5;
Зависимая переменная - Sales / продажа (в тысячах штук) в каждом магазине
Независимые переменные:
Price / цены компании на автокресла в каждом магазине;
Income / уровень дохода сообщества (в тысячах долларов);
ShelveLoc / качество стеллажа для размещения автокресел в каждом магазине: Плохое, Хорошее и Среднее.

Задания:
1. Данные своего варианта (см. таблицу ниже) разделить на выборку для построения моделей (80%) и отложенные наблюдения (20%). Оставить в таблице только указанные в варианте переменные. Отложенные наблюдения использовать только в задании 6.
2. Провести предварительный анализ данных с помощью описательных статистик и графиков, оценить взаимосвязь.
3. Проверить Y на нормальность. Если он распределён не по нормальному закону, прологарифмировать и снова провести анализ взаимосвязей переменных.
4. Составить список возможных спецификаций моделей множественной регрессии (на исходной Y и на логарифме Y ).
5. Оценить параметры моделей из списка. Оценить точность моделей методом перекрёстной проверки, указанным в варианте. Найти самую точную из моделей для Y . Найти самую точную из моделей для log(Y ).
6. Сделать прогноз с помощью самых точных моделей на отложенные наблюдения. Рассчитать MSEtest вручную и выбрать одну наиболее точную модель. Проинтерпретировать её параметры.

Задание 1. Решение.
Загрузим все необходимые для работы модули, определим константы и загрузим входные данные. Во входных данных отфильтруем только необходимые нам для работы признаки.
Определим фиктивные переменные для качественного признака и объединим их с исходными данными.
Разделим исходную выборку на тренировочную и тестовую.

Задание 1. Результаты.
Мы получили две выборки: тренировочную и тестовую, содержащие необходимые для работы данные.

Задание 2. Решение.
Проведем предварительный анализ данных на тренировочной выборке:
-Построим гистограммы распределения и графики зависимости количественных переменных;
-Построим гистограммы распределения и графики зависимости количественных переменных для разных значений качественной переменной;
-Построим матрицу корреляции количественных переменных;
-Построим матрицу корреляции количественных переменных для разных значений качественной переменной.

Задание 2. Результаты.
По гистограмме распределения "Sales" можно сделать предположение, что данные распределены нормально.
По графикам распределения видно, что переменная "Sales" слабо зависит от "Income" и сильно обратно зависима от переменной "Price". Это подтверждается матрицей корреляции.
Разные значения качественной переменной "ShelveLoc" заметно влияют на среднее значения распределения переменной "Sales".
По матрицам корреляции видно, что значение переменной "ShelveLoc" == "Good" усиливает коррелированность "Price" и "Sales", а значение переменной "ShelveLoc" == "Medium" усиливает коррелированность "Income" и "Sales".

Задание 3. Решение.
Проверим распределение переменной "Sales" на нормальность при помощи теста Шапиро-Уилка. Если распределение не является нормальным, проверим логарифм переменной на нормальность.

Задание 3. Результаты.
Мы определили, что распределение переменной "Sales" является нормальным.
Так как исходные данные обладают значениями "Sales" == 0, а также так как мы уже подтвердили нормальность переменной "Sales", логарифм переменной "Sales" далее рассматриваться не будет.

Задание 4. Решение. Результаты.
В соответствии с результатами задания 2 можно составить следующие линейные спецификации модели:
Sales = Income + Price + Medium + Good
Sales = Income + Price * Good + Medium + Good
Sales = Income * Medium + Price + Medium + Good
Sales = Income * Medium + Price * Good + Medium + Good
В соответствии с результатами задания 3, показательные модели рассматриваться не будут.

Задание 5. Решение.
Построим модели, определенные в задании 4.
Определим параметры моделей и оценим их точность методом K-FOLDS (K-VAl = 5).
Найдем самую точную из моделей.

Задание 5. Результаты.
Мы определили, что наименьшую ошибку методом K-FOLDS (K-VAL = 5) выдает модель №1:
Sales = Income + Price + Medium + Good

Задание 6. Решение.
Сделаем прогноз первой моделью на тестовые данные и оценим полученную ошибку тестирования. Оценим среднюю ошибку модели.
Построим первую модель на всех данных и проинтерпретируем ее параметры.

Задание 6. Результаты.
Средняя ошибка модели (построенной на тренировочных данных) равна 28.1% от среднего значения "Sales".
В результате построения на всех данных была получена модель:
Sales = 0.015 * Income - 0.056 * Price + 4.958 * Good + 1.936 * Medium
Ее параметры можно интерпретировать следующим образом:
1. При увеличении прибыли покупателей на 1 тыс. долларов, число продаж в среднем увеличится на 15 штук. Значит, приоритетной является работа с более состоятельными клиентами.
2. При увеличении цены кресел на 1 доллар, число продаж в среднем падает на 56 штук. Соотвественно, увеличение цены понижает продажи товара.
3. Качество стеллажа для размещения автокресел влияет на их продажи: продажи кресел на хороших стеллажах были выше на 3022 штук, а на плохих были ниже на 1936 штук (относительно среднего качества стеллажа).
