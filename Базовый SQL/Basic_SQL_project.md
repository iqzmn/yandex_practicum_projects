1. Отобразите все записи из таблицы `company` по компаниям, которые закрылись.
```SQL
SELECT *
FROM company
WHERE status = 'closed';
```
2. Отобразите количество привлечённых средств для новостных компаний США. Используйте данные из таблицы `company`. Отсортируйте таблицу по убыванию значений в поле `funding_total`.
```SQL
SELECT funding_total
FROM company
WHERE category_code = 'news'
  AND country_code = 'USA'
ORDER BY funding_total DESC;
```
3. Найдите общую сумму сделок по покупке одних компаний другими в долларах. Отберите сделки, которые осуществлялись только за наличные с 2011 по 2013 год включительно.
```SQL
SELECT sum(price_amount)
FROM acquisition
WHERE term_code = 'cash'
  AND (EXTRACT(YEAR
               FROM CAST(acquired_at AS date)) BETWEEN 2011 AND 2013);
```
4.  Отобразите имя, фамилию и названия аккаунтов людей в поле network_username, у которых названия аккаунтов начинаются на `Silver`.
```SQL
SELECT first_name,
       last_name,
       network_username
FROM people
WHERE network_username like 'Silver%';
```
5.  Выведите на экран всю информацию о людях, у которых названия аккаунтов в поле network_username содержат подстроку `money`, а фамилия начинается на `K`
```SQL
SELECT *
FROM people
WHERE network_username like '%money%'
  AND last_name like 'K%';
```
6.  Для каждой страны отобразите общую сумму привлечённых инвестиций, которые получили компании, зарегистрированные в этой стране. Страну, в которой зарегистрирована компания, можно определить по коду страны. Отсортируйте данные по убыванию суммы.
```SQL
SELECT country_code,
       sum(funding_total) AS summa
FROM company
GROUP BY country_code
ORDER BY summa DESC;
```
7.  Составьте таблицу, в которую войдёт дата проведения раунда, а также минимальное и максимальное значения суммы инвестиций, привлечённых в эту дату.
	Оставьте в итоговой таблице только те записи, в которых минимальное значение суммы инвестиций не равно нулю и не равно максимальному значению.
```SQL
SELECT *
FROM
  (SELECT funded_at,
          min(raised_amount) AS min_sum,
          max(raised_amount) AS max_sum
   FROM funding_round
   GROUP BY funded_at) AS f
WHERE min_sum <> 0
  AND min_sum <> max_sum;
```
8.  Создайте поле с категориями:

	- Для фондов, которые инвестируют в 100 и более компаний, назначьте категорию `high_activity`.
	- Для фондов, которые инвестируют в 20 и более компаний до 100, назначьте категорию `middle_activity`.
	- Если количество инвестируемых компаний фонда не достигает 20, назначьте категорию `low_activity`.

	Отобразите все поля таблицы `fund` и новое поле с категориями.
```SQL
SELECT *,
       CASE
           WHEN invested_companies >= 100 THEN 'high_activity'
           WHEN invested_companies >= 20 THEN 'middle_activity'
           ELSE 'low_activity'
       END
FROM fund;
```
9. Для каждой из категорий, назначенных в предыдущем задании, посчитайте округлённое до ближайшего целого числа среднее количество инвестиционных раундов, в которых фонд принимал участие. Выведите на экран категории и среднее число инвестиционных раундов. Отсортируйте таблицу по возрастанию среднего.
```SQL
SELECT round(avg(investment_rounds)) AS AVG,
       CASE
           WHEN invested_companies>=100 THEN 'high_activity'
           WHEN invested_companies>=20 THEN 'middle_activity'
           ELSE 'low_activity'
       END AS activity
FROM fund
GROUP BY activity
ORDER BY AVG ASC;
```
10. Проанализируйте, в каких странах находятся фонды, которые чаще всего инвестируют в стартапы. 
		Для каждой страны посчитайте минимальное, максимальное и среднее число компаний, в которые инвестировали фонды этой страны, основанные с 2010 по 2012 год включительно. 
		Исключите страны с фондами, у которых минимальное число компаний, получивших инвестиции, равно нулю. Выгрузите десять самых активных стран-инвесторов: отсортируйте таблицу по среднему количеству компаний от большего к меньшему. Затем добавьте сортировку по коду страны в лексикографическом порядке.
```SQL
SELECT country_code,
       min(invested_companies),
       max(invested_companies),
       avg(invested_companies)
FROM fund
WHERE EXTRACT(YEAR
              FROM CAST(founded_at AS date)) BETWEEN 2010 AND 2012
GROUP BY country_code
HAVING min(invested_companies) > 0
ORDER BY AVG DESC, country_code
LIMIT 10;
```
11.  Отобразите имя и фамилию всех сотрудников стартапов. Добавьте поле с названием учебного заведения, которое окончил сотрудник, если эта информация известна.
```SQL
WITH e AS
  (SELECT *
   FROM education)
SELECT first_name,
       last_name,
       instituition
FROM people AS p
LEFT JOIN e ON p.id = e.person_id;
```
12.  Для каждой компании найдите количество учебных заведений, которые окончили её сотрудники. Выведите название компании и число уникальных названий учебных заведений. Составьте топ-5 компаний по количеству университетов.
```SQL
WITH e AS
  (SELECT *
   FROM education),
     c AS
  (SELECT *
   FROM company)
SELECT name,
       count(DISTINCT instituition)
FROM people AS p
LEFT JOIN e ON p.id = e.person_id
LEFT JOIN c ON p.company_id = c.id
WHERE name IS NOT NULL
GROUP BY name
ORDER BY COUNT DESC
LIMIT 5;
```
13. Составьте список с уникальными названиями закрытых компаний, для которых первый раунд финансирования оказался последним.
```SQL
SELECT name
FROM company
WHERE id in
    (SELECT company_id
     FROM funding_round
     WHERE is_first_round = 1
       AND is_last_round = 1 )
  AND status = 'closed';
```
14. Составьте список уникальных номеров сотрудников, которые работают в компаниях, отобранных в предыдущем задании.
```SQL
SELECT DISTINCT id
FROM people
WHERE company_id in
    (SELECT id
     FROM company
     WHERE id in
         (SELECT company_id
          FROM funding_round
          WHERE is_first_round = 1
            AND is_last_round = 1)
       AND status = 'closed' );
```
15. Составьте таблицу, куда войдут уникальные пары с номерами сотрудников из предыдущей задачи и учебным заведением, которое окончил сотрудник.
```SQL
SELECT DISTINCT p.id,
                instituition
FROM people AS p
LEFT JOIN education AS e ON p.id = e.person_id
WHERE company_id in
    (SELECT id
     FROM company
     WHERE id in
         (SELECT company_id
          FROM funding_round
          WHERE is_first_round = 1
            AND is_last_round = 1)
       AND status = 'closed' )
  AND instituition IS NOT NULL;
```
16. Посчитайте количество учебных заведений для каждого сотрудника из предыдущего задания. При подсчёте учитывайте, что некоторые сотрудники могли окончить одно и то же заведение дважды.
```SQL
WITH pe AS
  (SELECT p.id,
          instituition
   FROM people AS p
   LEFT JOIN education AS e ON p.id = e.person_id
   WHERE company_id in
       (SELECT id
        FROM company
        WHERE id in
            (SELECT company_id
             FROM funding_round
             WHERE is_first_round = 1
               AND is_last_round = 1)
          AND status = 'closed' )
     AND instituition IS NOT NULL)
SELECT id,
       count(instituition)
FROM pe
GROUP BY id;
```
17. Дополните предыдущий запрос и выведите среднее число учебных заведений (всех, не только уникальных), которые окончили сотрудники разных компаний. Нужно вывести только одну запись, группировка здесь не понадобится.
```SQL
WITH pe AS
  (SELECT p.id,
          instituition
   FROM people AS p
   LEFT JOIN education AS e ON p.id = e.person_id
   WHERE company_id in
       (SELECT id
        FROM company
        WHERE id in
            (SELECT company_id
             FROM funding_round
             WHERE is_first_round = 1
               AND is_last_round = 1)
          AND status = 'closed' )
     AND instituition IS NOT NULL ),
     ii AS
  (SELECT id,
          count(instituition)
   FROM pe
   GROUP BY id)
SELECT avg(COUNT)
FROM ii;
```
18. Напишите похожий запрос: выведите среднее число учебных заведений (всех, не только уникальных), которые окончили сотрудники Socialnet.
```SQL
WITH pe AS
  (SELECT p.id,
          instituition
   FROM people AS p
   LEFT JOIN education AS e ON p.id = e.person_id
   WHERE company_id =
       (SELECT id
        FROM company
        WHERE name like 'Socialnet')
     AND instituition IS NOT NULL ),
     ii AS
  (SELECT id,
          count(instituition)
   FROM pe
   GROUP BY id)
SELECT avg(COUNT)
FROM ii;
```
19. Составьте таблицу из полей:

	- `name_of_fund` — название фонда;
	- `name_of_company` — название компании;
	- `amount` — сумма инвестиций, которую привлекла компания в раунде.

	В таблицу войдут данные о компаниях, в истории которых было больше шести важных этапов, а раунды финансирования проходили с 2012 по 2013 год включительно.
```SQL
SELECT f.name AS name_of_fund,
       c.name AS name_of_company,
       fr.raised_amount AS amount
FROM investment AS i
LEFT JOIN company AS c ON i.company_id = c.id
LEFT JOIN fund AS f ON i.fund_id = f.id
LEFT JOIN funding_round AS fr ON i.funding_round_id = fr.id
WHERE c.milestones > 6
  AND (EXTRACT(YEAR
               FROM CAST(fr.funded_at AS date)) BETWEEN 2012 AND 2013)
ORDER BY c.name;
```
20. Выгрузите таблицу, в которой будут такие поля:

	- название компании-покупателя;
	- сумма сделки;
	- название компании, которую купили;
	- сумма инвестиций, вложенных в купленную компанию;
	- доля, которая отображает, во сколько раз сумма покупки превысила сумму вложенных в компанию инвестиций, округлённая до ближайшего целого числа.

	Не учитывайте те сделки, в которых сумма покупки равна нулю. Если сумма инвестиций в компанию равна нулю, исключите такую компанию из таблицы.

	Отсортируйте таблицу по сумме сделки от большей к меньшей, а затем по названию купленной компании в лексикографическом порядке. Ограничьте таблицу первыми десятью записями.
```SQL
WITH acg AS
  (SELECT *
   FROM company),
     acd AS
  (SELECT *
   FROM company)
SELECT acg.name AS byer_name,
       a.price_amount,
       acd.name AS sold_name,
       acd.funding_total,
       round(a.price_amount/acd.funding_total) AS portion
FROM acquisition AS a
LEFT JOIN acg ON a.acquiring_company_id = acg.id
LEFT JOIN acd ON a.acquired_company_id = acd.id
WHERE a.price_amount <> 0
  AND acd.funding_total <> 0
ORDER BY a.price_amount DESC
LIMIT 10;
```
21. Выгрузите таблицу, в которую войдут названия компаний из категории `social`, получившие финансирование с 2010 по 2013 год включительно. Проверьте, что сумма инвестиций не равна нулю. Выведите также номер месяца, в котором проходил раунд финансирования.
```SQL
WITH fr AS
  (SELECT *
   FROM funding_round
   WHERE (EXTRACT(YEAR
                  FROM CAST(funded_at AS date)) BETWEEN 2010 AND 2013)
     AND raised_amount <> 0 )
SELECT c.name,
       EXTRACT(MONTH
               FROM CAST(fr.funded_at AS date))
FROM company AS c
RIGHT JOIN fr ON fr.company_id = c.id
WHERE c.funding_total <> 0
  AND c.category_code = 'social';
```
22. Отберите данные по месяцам с 2010 по 2013 год, когда проходили инвестиционные раунды. Сгруппируйте данные по номеру месяца и получите таблицу, в которой будут поля:

	- номер месяца, в котором проходили раунды;
	- количество уникальных названий фондов из США, которые инвестировали в этом месяце;
	- количество компаний, купленных за этот месяц;
	- общая сумма сделок по покупкам в этом месяце.

```SQL
WITH fd AS
  (SELECT name AS fund_name,
          id
   FROM fund
   WHERE country_code = 'USA' ),
     inv AS
  (SELECT fund_name,
          funding_round_id
   FROM investment AS i
   LEFT JOIN fd ON i.fund_id = fd.id),
     ac AS
  (SELECT EXTRACT(MONTH
                  FROM CAST(acquired_at AS date)) AS deal_month,
          count(acquired_company_id) AS count_acquired,
          SUM(price_amount) AS sum_acquired
   FROM acquisition
   WHERE EXTRACT(YEAR
                 FROM CAST(acquired_at AS date)) BETWEEN 2010 AND 2013
   GROUP BY EXTRACT(MONTH
                    FROM CAST(acquired_at AS date))),
     main AS
  (SELECT EXTRACT(MONTH
                  FROM funded_at) AS month_num,
          count(DISTINCT inv.fund_name) AS funds_name
   FROM funding_round AS f
   LEFT JOIN inv ON f.id = inv.funding_round_id
   WHERE EXTRACT(YEAR
                 FROM funded_at) BETWEEN 2010 AND 2013
   GROUP BY EXTRACT(MONTH
                    FROM funded_at))
SELECT main.month_num,
       main.funds_name,
       ac.count_acquired,
       ac.sum_acquired
FROM main
JOIN ac ON main.month_num = ac.deal_month;
```
23. Составьте сводную таблицу и выведите среднюю сумму инвестиций для стран, в которых есть стартапы, зарегистрированные в 2011, 2012 и 2013 годах. Данные за каждый год должны быть в отдельном поле. Отсортируйте таблицу по среднему значению инвестиций за 2011 год от большего к меньшему.
```SQL
WITH inv_2011 AS
  (SELECT country_code,
          avg(funding_total) AS avg2011
   FROM company
   WHERE EXTRACT(YEAR
                 FROM founded_at) = 2011
   GROUP BY country_code),
     inv_2012 AS
  (SELECT country_code,
          avg(funding_total) AS avg2012
   FROM company
   WHERE EXTRACT(YEAR
                 FROM founded_at) = 2012
   GROUP BY country_code),
     inv_2013 AS
  (SELECT country_code,
          avg(funding_total) AS avg2013
   FROM company
   WHERE EXTRACT(YEAR
                 FROM founded_at) = 2013
   GROUP BY country_code)
SELECT inv_2011.country_code,
       avg2011,
       avg2012,
       avg2013
FROM inv_2011
INNER JOIN inv_2012 ON inv_2011.country_code = inv_2012.country_code
INNER JOIN inv_2013 ON inv_2011.country_code = inv_2013.country_code
ORDER BY avg2011 DESC;
```