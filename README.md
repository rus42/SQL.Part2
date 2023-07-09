SQL. Часть 2. Васёв А.В.

## Задание 1
### Одним запросом получите информацию о магазине, в котором обслуживается более 300 покупателей, и выведите в результат следующую информацию:

    - фамилия и имя сотрудника из этого магазина;
    - город нахождения магазина;
    - количество пользователей, закреплённых в этом магазине.

```java
SELECT CONCAT(s.last_name, ' ', s.first_name) as name, 
(SELECT COUNT(1) FROM customer c WHERE c.store_id = s2.store_id) as customer_count, c.city
FROM staff s
INNER JOIN store s2 on s.store_id = s2.store_id
INNER JOIN address a on s2.address_id = a.address_id
INNER JOIN city c on a.city_id = c.city_id
HAVING customer_count > 300
```

![alt text](https://github.com/rus42/SQL.Part2/blob/main/Task_1.png)

## Задание 2
### Получите количество фильмов, продолжительность которых больше средней продолжительности всех фильмов.
```java
SELECT COUNT(1)
FROM film 
WHERE `length` > (SELECT AVG(`length`) FROM film) 
```

![alt text](https://github.com/rus42/SQL.Part2/blob/main/Task_2.png)

## Задание 3
### Получите информацию, за какой месяц была получена наибольшая сумма платежей, и добавьте информацию по количеству аренд за этот месяц.

```java
SELECT MONTH(payment_date), SUM(amount) as amount_sum, COUNT(1) as rent 
from payment
group by MONTH(payment_date)
HAVING amount_sum = (SELECT MAX(sum_table.amount2) from (Select SUM(amount) as amount2 from payment group by MONTH(payment_date)) sum_table)
```

![alt text](https://github.com/rus42/SQL.Part2/blob/main/Task_3.png)

## Задание 4
### Посчитайте количество продаж, выполненных каждым продавцом. Добавьте вычисляемую колонку «Премия». Если количество продаж превышает 8000, то значение в колонке будет «Да», иначе должно быть значение «Нет».

```java
select count(1), CONCAT(s.last_name, ' ', s.first_name) as name,
CASE 
	WHEN count(1) > 8010 THEN 'YES'
	WHEN count(1) < 8010 THEN 'NO'
END as premium
from rental r
INNER JOIN staff s ON r.staff_id = s.staff_id
group by s.staff_id
```

![alt text](https://github.com/rus42/SQL.Part2/blob/main/Task_4.png)

## Задание 5
### Найдите фильмы, которые ни разу не брали в аренду.

```java
SELECT f.title 
FROM film f
LEFT JOIN inventory i on f.film_id = i.film_id
LEFT JOIN rental r on i.inventory_id = r.inventory_id
WHERE r.rental_id is NULL 
```

![alt text](https://github.com/rus42/SQL.Part2/blob/main/Task_5.png)
