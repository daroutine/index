# Домашнее задание к занятию "`Индексы`" - `Зарецкий Юрий`


### Задание 1

Напишите запрос к учебной базе данных, который вернёт процентное отношение общего размера всех индексов к общему размеру всех таблиц.

### ОТВЕТ:

![1](https://github.com/daroutine/index/blob/main/1.jpg)

1. `SELECT table_schema as DB_name,
CONCAT(ROUND((SUM(index_length))*100/(SUM(data_length+index_length)),2),'%') '% of index'
FROM information_schema.TABLES where TABLE_SCHEMA = 'sakila'`


### Задание 2

Выполните explain analyze следующего запроса:

select distinct concat(c.last_name, ' ', c.first_name), sum(p.amount) over (partition by c.customer_id, f.title)
from payment p, rental r, customer c, inventory i, film f
where date(p.payment_date) = '2005-07-30' and p.payment_date = r.rental_date and r.customer_id = c.customer_id and i.inventory_id = r.inventory_id
перечислите узкие места;
оптимизируйте запрос: внесите корректировки по использованию операторов, при необходимости добавьте индексы.

### ОТВЕТ:

В первом случае функция обрабатывает лишние таблицы: inventory, rental, film

![2](https://github.com/daroutine/index/blob/main/2.0.jpg)

Во втором случае данные таблицы исключены из запроса, что позволило значительно увеличить скорость обработки данных

![2.2](https://github.com/daroutine/index/blob/main/2.2.jpg)


`select distinct concat(c.last_name, ' ', c.first_name), sum(p.amount) over (partition by c.customer_id)
from payment p, customer c
where date(p.payment_date) = '2005-07-30' and p.customer_id = c.customer_id `


