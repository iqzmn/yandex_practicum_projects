Задача 1.  
Найдите количество вопросов, которые набрали больше 300 очков или как минимум 100 раз были добавлены в «Закладки».
```SQL
SELECT COUNT(*)
FROM stackoverflow.posts p
JOIN stackoverflow.post_types pt ON p.post_type_id = pt.id
WHERE (score > 300 OR favorites_count >= 100) AND type = 'Question';
```
Задача 2.  
Сколько в среднем в день задавали вопросов с 1 по 18 ноября 2008 включительно? Результат округлите до целого числа.
```SQL
SELECT ROUND(AVG(month_count))
FROM (
    SELECT COUNT(creation_date::date) AS month_count
    FROM stackoverflow.posts p
    JOIN stackoverflow.post_types pt ON p.post_type_id = pt.id
    WHERE type = 'Question' AND creation_date::date BETWEEN '2008-11-01' AND '2008-11-18'
    GROUP BY creation_date::date
) AS d;
```
Задача 3.  
Сколько пользователей получили значки сразу в день регистрации? Выведите количество уникальных пользователей.
```SQL
SELECT COUNT(DISTINCT u.id) AS users_with_badges
FROM stackoverflow.users u
JOIN stackoverflow.badges b ON u.id = b.user_id
WHERE DATE_TRUNC('day', u.creation_date) = DATE_TRUNC('day', b.creation_date);
```
Задача 4.  
Сколько уникальных постов пользователя с именем Joel Coehoorn получили хотя бы один голос?
```SQL
SELECT COUNT(DISTINCT post_id)
FROM stackoverflow.users u
JOIN stackoverflow.posts p ON u.id = p.user_id
JOIN stackoverflow.votes v ON p.id = v.post_id
WHERE display_name = 'Joel Coehoorn' AND v.id > 0;
```
Задача 5.  
Выгрузите все поля таблицы `vote_types`. Добавьте к таблице поле `rank`, в которое войдут номера записей в обратном порядке. Таблица должна быть отсортирована по полю `id`.
```SQL
SELECT *, RANK() OVER (ORDER BY id DESC) AS rank
FROM stackoverflow.vote_types
ORDER BY id;
```
Задача 6.  
Отберите 10 пользователей, которые поставили больше всего голосов типа `Close`. Отобразите таблицу из двух полей: идентификатором пользователя и количеством голосов. Отсортируйте данные сначала по убыванию количества голосов, потом по убыванию значения идентификатора пользователя.
```SQL
WITH votes_tab AS (
    SELECT
        user_id,
        ROW_NUMBER() OVER (PARTITION BY user_id ORDER BY vt.id DESC) AS votes_number
    FROM
        stackoverflow.votes v
        JOIN stackoverflow.vote_types vt ON v.vote_type_id = vt.id
    WHERE
        vt.name = 'Close'
)

SELECT
    user_id,
    MAX(votes_number) AS max_votes
FROM
    votes_tab vt
GROUP BY
    user_id
ORDER BY
    max_votes DESC,
    user_id DESC
LIMIT 10;
```
Задача 7.  
Отберите 10 пользователей по количеству значков, полученных в период с 15 ноября по 15 декабря 2008 года включительно. Отобразите несколько полей:

- идентификатор пользователя;
- число значков;
- место в рейтинге — чем больше значков, тем выше рейтинг.

Пользователям, которые набрали одинаковое количество значков, присвойте одно и то же место в рейтинге.

Отсортируйте записи по количеству значков по убыванию, а затем по возрастанию значения идентификатора пользователя.
```SQL
WITH badge AS (
    SELECT
        user_id,
        COUNT(*) OVER (PARTITION BY user_id) AS badges,
        ROW_NUMBER() OVER (PARTITION BY user_id) AS icon
    FROM
        stackoverflow.badges
    WHERE
        creation_date::date BETWEEN '2008-11-15' AND '2008-12-15'
),
badge2 AS (
    SELECT
        user_id,
        badges
    FROM
        badge
    GROUP BY
        user_id, badges
    ORDER BY
        badges DESC, user_id
    LIMIT 10
)

SELECT
    *,
    DENSE_RANK() OVER (ORDER BY badges DESC) AS dense_rank
FROM
    badge2;
```
Задача 8.  
Сколько в среднем очков получает пост каждого пользователя?

Сформируйте таблицу из следующих полей:

- заголовок поста;
- идентификатор пользователя;
- число очков поста;
- среднее число очков пользователя за пост, округлённое до целого числа.

Не учитывайте посты без заголовка, а также те, что набрали ноль очков.
```SQL
SELECT
    title,
    user_id,
    score,
    ROUND(AVG(score) OVER (PARTITION BY user_id)) AS post_score
FROM
    stackoverflow.posts
WHERE
    title IS NOT NULL
    AND score <> '0';
```
Задача 9.  
Отобразите заголовки постов, которые были написаны пользователями, получившими более 1000 значков. Посты без заголовков не должны попасть в список.
```SQL
WITH t1 AS (
    SELECT title, user_id
    FROM stackoverflow.posts
    WHERE title IS NOT NULL
),
t2 AS (
    SELECT user_id, COUNT(*) AS num_badge
    FROM stackoverflow.badges
    GROUP BY user_id
)
SELECT title
FROM t1
LEFT JOIN t2 ON t1.user_id = t2.user_id
WHERE num_badge > 1000
ORDER BY num_badge DESC;
```
Задача 10.  
Напишите запрос, который выгрузит данные о пользователях из Канады (англ. Canada). Разделите пользователей на три группы в зависимости от количества просмотров их профилей:

- пользователям с числом просмотров больше либо равным 350 присвойте группу `1`;
- пользователям с числом просмотров меньше 350, но больше либо равно 100 — группу `2`;
- пользователям с числом просмотров меньше 100 — группу `3`.

Отобразите в итоговой таблице идентификатор пользователя, количество просмотров профиля и группу. Пользователи с количеством просмотров меньше либо равным нулю не должны войти в итоговую таблицу.
```SQL
SELECT DISTINCT id, views,
    CASE
        WHEN views < 100 THEN 3
        WHEN views < 350 AND views >= 100 THEN 2
        WHEN views >= 350 THEN 1
    END AS user_group
FROM stackoverflow.users
WHERE location LIKE '%Canada%' AND views <> 0;
```
Задача 11.  
Дополните предыдущий запрос. Отобразите лидеров каждой группы — пользователей, которые набрали максимальное число просмотров в своей группе. Выведите поля с идентификатором пользователя, группой и количеством просмотров. Отсортируйте таблицу по убыванию просмотров, а затем по возрастанию значения идентификатора.
```SQL
WITH rank AS (
    SELECT
        DISTINCT id,
        views,
        CASE
            WHEN views < 100 THEN 3
            WHEN views < 350 AND views >= 100 THEN 2
            WHEN views >= 350 THEN 1
        END AS user_group
    FROM stackoverflow.users
    WHERE
        location LIKE '%Canada%'
        AND views <> 0
)
SELECT
    id,
    user_group,
    views
FROM (SELECT
          *,
          DENSE_RANK() OVER (PARTITION BY user_group ORDER BY views DESC) AS rank_in_group
       FROM rank
     ) AS ranked
WHERE
    rank_in_group = 1
ORDER BY
    views DESC, id;
```
Задача 12.  
Посчитайте ежедневный прирост новых пользователей в ноябре 2008 года. Сформируйте таблицу с полями:

- номер дня;
- число пользователей, зарегистрированных в этот день;
- сумму пользователей с накоплением.
```SQL
WITH tab1 AS (
    SELECT EXTRACT(DAY FROM creation_date) AS day_number,
           COUNT(*) OVER (PARTITION BY EXTRACT(DAY FROM creation_date)) AS total_day
    FROM stackoverflow.users
    WHERE EXTRACT(MONTH FROM creation_date) = 11
          AND EXTRACT(YEAR FROM creation_date) = 2008
),
tab2 AS (
    SELECT day_number,
           total_day
    FROM tab1
    GROUP BY day_number,
             total_day
)
SELECT day_number,
       total_day,
       SUM(total_day) OVER (ORDER BY day_number) AS running_total
FROM tab2;
```
Задача 13.  
Для каждого пользователя, который написал хотя бы один пост, найдите интервал между регистрацией и временем создания первого поста. Отобразите:

- идентификатор пользователя;
- разницу во времени между регистрацией и первым постом.
```SQL
WITH t1 AS (
    SELECT p.user_id,
           u.creation_date AS registration,
           MIN(p.creation_date) OVER (PARTITION BY p.user_id ORDER BY p.creation_date) AS post_creation
    FROM stackoverflow.posts p
         JOIN stackoverflow.users u ON p.user_id = u.id
)
SELECT user_id,
       post_creation - registration AS intervаl
FROM t1
GROUP BY user_id, 
         post_creation - registration;
```
Задача 14.  
Выведите общую сумму просмотров у постов, опубликованных в каждый месяц 2008 года. Если данных за какой-либо месяц в базе нет, такой месяц можно пропустить. Результат отсортируйте по убыванию общего количества просмотров.
```SQL
WITH t1 AS (
    SELECT
        SUM(views_count) OVER (PARTITION BY DATE_TRUNC('MONTH', creation_date)::date) AS suma,
        DATE_TRUNC('MONTH', creation_date)::date AS post_start
    FROM
        stackoverflow.posts
    WHERE
        EXTRACT(YEAR FROM creation_date) = 2008
)

SELECT
    DISTINCT suma,
    post_start
FROM
    t1
ORDER BY
    suma DESC, post_start;
```
Задача 15.  
Выведите имена самых активных пользователей, которые в первый месяц после регистрации (включая день регистрации) дали больше 100 ответов. Вопросы, которые задавали пользователи, не учитывайте. Для каждого имени пользователя выведите количество уникальных значений `user_id`. Отсортируйте результат по полю с именами в лексикографическом порядке.
```SQL
SELECT
    u.display_name,
    COUNT(DISTINCT p.user_id) AS user_count
FROM
    stackoverflow.posts AS p
    JOIN stackoverflow.users AS u ON p.user_id = u.id
    JOIN stackoverflow.post_types AS pt ON pt.id = p.post_type_id
WHERE
    p.creation_date::date BETWEEN u.creation_date::date AND (u.creation_date::date + INTERVAL '1 month')
    AND pt.type = 'Answer'
GROUP BY
    u.display_name
HAVING
    COUNT(p.id) > 100
ORDER BY
    u.display_name;
```
Задача 16.  
Выведите количество постов за 2008 год по месяцам. Отберите посты от пользователей, которые зарегистрировались в сентябре 2008 года и сделали хотя бы один пост в декабре того же года. Отсортируйте таблицу по значению месяца по убыванию.
```SQL
WITH u AS (
    SELECT id
    FROM stackoverflow.users
    WHERE DATE_TRUNC('month', creation_date)::date = '2008-09-01'
),
p AS (
    SELECT user_id
    FROM stackoverflow.posts
    WHERE DATE_TRUNC('month', creation_date)::date = '2008-12-01'
),
t1 AS (
    SELECT
        DATE_TRUNC('month', creation_date)::date AS month,
        COUNT(id) OVER (PARTITION BY DATE_TRUNC('month', creation_date)::date) AS count_for_month
    FROM
        stackoverflow.posts
    WHERE
        posts.user_id IN (
            SELECT u.id
            FROM u
            JOIN p ON u.id = p.user_id
        )
)
SELECT
    month,
    COUNT(*) AS count_for_month
FROM
    t1
GROUP BY
    month
ORDER BY
    month DESC;
```
Задача 17.  
Используя данные о постах, выведите несколько полей:

- идентификатор пользователя, который написал пост;
- дата создания поста;
- количество просмотров у текущего поста;
- сумма просмотров постов автора с накоплением.

Данные в таблице должны быть отсортированы по возрастанию идентификаторов пользователей, а данные об одном и том же пользователе — по возрастанию даты создания поста.
```SQL
SELECT
    user_id,
    creation_date,
    views_count,
    SUM(views_count) OVER (PARTITION BY user_id ORDER BY creation_date) AS running_total_views
FROM
    stackoverflow.posts;
```
Задача 18.  
Сколько в среднем дней в период с 1 по 7 декабря 2008 года включительно пользователи взаимодействовали с платформой? Для каждого пользователя отберите дни, в которые он или она опубликовали хотя бы один пост. Нужно получить одно целое число — не забудьте округлить результат.
```SQL
SELECT ROUND(AVG(t.days_count))
FROM (
      SELECT user_id,
             COUNT(DISTINCT creation_date::date)  AS days_count
      FROM stackoverflow.posts
      WHERE creation_date::date BETWEEN '2008-12-01' AND '2008-12-07' 
      GROUP BY user_id
) AS t
```
Задача 19.  
На сколько процентов менялось количество постов ежемесячно с 1 сентября по 31 декабря 2008 года? Отобразите таблицу со следующими полями:

- Номер месяца.
- Количество постов за месяц.
- Процент, который показывает, насколько изменилось количество постов в текущем месяце по сравнению с предыдущим.

Если постов стало меньше, значение процента должно быть отрицательным, если больше — положительным. Округлите значение процента до двух знаков после запятой.

Напомним, что при делении одного целого числа на другое в PostgreSQL в результате получится целое число, округлённое до ближайшего целого вниз. Чтобы этого избежать, переведите делимое в тип `numeric`.
```SQL
WITH t AS (
    SELECT
        EXTRACT(MONTH FROM creation_date) AS extracted_month,
        COUNT(DISTINCT id) AS num_posts
    FROM
        stackoverflow.posts
    WHERE
        creation_date BETWEEN '2008-09-01' AND '2008-12-31'
    GROUP BY
        extracted_month
)

SELECT
    extracted_month,
    num_posts,
    ROUND(
        (num_posts - LAG(num_posts) OVER (ORDER BY extracted_month)) * 100.00 /
        LAG(num_posts) OVER (ORDER BY extracted_month), 2
    ) AS post_percentage_change
FROM
    t;
```
Задача 20.  
Найдите пользователя, который опубликовал больше всего постов за всё время с момента регистрации. Выведите данные его активности за октябрь 2008 года в таком виде:

- номер недели;
- дата и время последнего поста, опубликованного на этой неделе.
```SQL
WITH post_counts AS (
    SELECT
        user_id,
        COUNT(id) AS post_count
    FROM
        stackoverflow.posts
    GROUP BY
        user_id
)

, max_post_user AS (
    SELECT
        user_id
    FROM
        post_counts
    ORDER BY
        post_count DESC
    LIMIT 1
)
    SELECT
        DISTINCT EXTRACT(WEEK FROM creation_date) AS week,
        MAX(creation_date) OVER (PARTITION BY EXTRACT(WEEK FROM creation_date)) AS max_creation_date
    FROM
        stackoverflow.posts
    WHERE
        user_id IN (SELECT user_id FROM max_post_user)
        AND DATE_TRUNC('MONTH', creation_date)::date = '2008-10-01'
```