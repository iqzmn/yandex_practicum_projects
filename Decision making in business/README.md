## Цель проекта
Имеем набор гипотез для оценки увеличения выручки интернет-магазина. Наша задача как аналитика данных провести приоритизацию гипотез по следующим параметрам: `Reach`, `Impact`, `Confidence`, `Effort`. Также необходимо проанализировать результаты проведённого A/B-теста.

## Вопросы и инструменты анализа
Работа состоит из двух основных частей: приоритизация гипотез и анализ A/B-теста.  
В первой части проекта нам требуется применить фреймворки `ICE` и `RICE` для оценки гипотез и сделать вывод на основе полученных результатов.   
Во второй части проекта на основе уже проведённого A/B-теста необходимо провести анализ готовых результатов. Для решения поставленных задач мы изучим параметры продаж и воспользуемся следующим инструментарием: 
1. Построим график кумулятивной выручки по группам. 
2. Построим график кумулятивного среднего чека по группам. 
3. Построим график относительного изменения кумулятивного среднего чека группы B к группе A. 
4. Построим график кумулятивного среднего количества заказов на посетителя по группам. 
5. Построим график относительного изменения кумулятивного среднего количества заказов на посетителя группы B к группе A.
6. Построим точечный график количества заказов по пользователям. 
7. Посчитаем 95-й и 99-й перцентили количества заказов на пользователя. Выберем границу для определения аномальных пользователей.
8. Построим точечный график стоимостей заказов. 
9. Посчитаем 95-й и 99-й перцентили стоимости заказов. Выберем границу для определения аномальных заказов.
10. Посчитаем статистическую значимость различий в среднем количестве заказов на посетителя между группами между группами по «сырым» данным.
11. Посчитаем статистическую значимость различий в среднем чеке заказа между группами по «сырым» данным. 
12. Посчитаем статистическую значимость различий в среднем количестве заказов на посетителя между группами между группами по «очищенным» данным. 
13. Посчитаем статистическую значимость различий в среднем чеке заказа между группами по «очищенным» данным. 

На основе проведённого анализа и результатов теста, примем одно из следующих решений:
1. Остановить тест, зафиксировать победу одной из групп.
2. Остановить тест, зафиксировать отсутствие различий между группами.
3. Продолжить тест.


## Описание данных
**Данные для приоритизации гипотез**

Файл `hypothesis.csv`.

- `Hypothesis` — краткое описание гипотезы;
- `Reach` — охват пользователей по 10-балльной шкале;
- `Impact` — влияние на пользователей по 10-балльной шкале;
- `Confidence` — уверенность в гипотезе по 10-балльной шкале;
- `Efforts` — затраты ресурсов на проверку гипотезы по 10-балльной шкале. Чем больше значение `Efforts`, тем дороже проверка гипотезы.

**Данные для анализа A/B теста**

Файл `orders.csv`.

- `transactionId` — идентификатор заказа;
- `visitorId` — идентификатор пользователя, совершившего заказ;
- `date` — дата, когда был совершён заказ;
- `revenue` — выручка заказа;
- `group` — группа A/B-теста, в которую попал заказ.

Файл `visitors.csv`.

- `date` — дата;
- `group` — группа A/B-теста;
- `visitors` — количество пользователей в указанную дату в указанной группе A/B-теста

## Основные этапы проекта
1. Предобработка данных
2. Приоритизация гипотез
3. Анализ A/B теста
4. Общий вывод

## Краткий итог
После приоритизации гипотез и проведения расчётов мы получили следующие результаты:
- гипотеза о том, что различий в среднем количестве заказов между группами по "сырым" данным нет - не подтвердилась;
- гипотеза о том, что различий в среднем чеке заказа между группами по 'сырым' данным нет - опровергнуть не удалось;
- гипотеза о том, что различий в среднем количестве заказов между группами по "очищенным" данным нет - не подтвердилась;
гипотеза о том, что различий в среднем чеке заказа между группами по 'очищенным' данным нет - опровергнуть не удалось.  

По результатам теста принято решение остановить тест и зафиксировать результат в виде увеличения за 1 месяц выручки группы В, а также увеличении конверсии данной группы по сравнению с группой A.