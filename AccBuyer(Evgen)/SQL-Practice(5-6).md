# MS-SQLServer-Практика

# Практическая работа № 5-6
##### Цель работы: Приобретение навыков практической работы спредставлениями в среде SQL SERVER.

# Отчет

### 1.осуществить поиск публикаций по названию, фамилии автора, названию издательства. Вывести название публикации, фамилию автора, цену, название издательства.]
***SELECT dbo.authors.au_lname, dbo.authors.au_fname, dbo.titles.title_id, dbo.titles.pub_id, dbo.titles.price, dbo.publishers.pub_name
FROM dbo.titles INNER JOIN
dbo.publishers ON dbo.titles.pub_id = dbo.publishers.pub_id INNER JOIN
dbo.titleauthor ON dbo.titles.title_id = dbo.titleauthor.title_id INNER JOIN
dbo.authors ON dbo.titleauthor.au_id = dbo.authors.au_id
WHERE (dbo.authors.au_lname = 'Green') AND (dbo.authors.au_fname = 'Marjorie') AND (dbo.publishers.pub_name = 'Algodata Infosystems')***

![alt-У вас не загрузилось :( ](http://ipic.su/img/img7/fs/Nomer1.1569523693.png "SQLServer5-6")

### 2. осуществить поиск заказов по номеру заказа, дате заказа, номеру магазина. Вывести номер заказа, номер магазина, адрес магазина, название товара, цену, количество, стоимость заказа c учётом скидки и без неё.

***SELECT dbo.stores.stor_id, dbo.stores.stor_address, dbo.sales.title_id, dbo.titles.price, dbo.discounts.discount, dbo.sales.qty, dbo.titles.price * dbo.sales.qty AS sum,
dbo.titles.price * dbo.sales.qty - dbo.discounts.discount * dbo.titles.price * dbo.sales.qty / 100 AS sum_disc
FROM dbo.stores INNER JOIN
dbo.sales ON dbo.stores.stor_id = dbo.sales.stor_id INNER JOIN
dbo.discounts ON dbo.stores.stor_id = dbo.discounts.stor_id INNER JOIN
dbo.titles ON dbo.sales.title_id = dbo.titles.title_id
WHERE (dbo.stores.stor_id = '8042') AND (dbo.sales.ord_num = 'P723') AND (dbo.sales.ord_date = '1993 - 03 - 11 00:00:00.000')***

![alt-У вас не загрузилось :( ](http://ipic.su/img/img7/fs/nomer2.1569523726.png "SQLServer5-6")

### 3. осуществить поиск заказов по названию публикации, список заказов упорядочить по номеру заказа.

***SELECT TOP (100) PERCENT dbo.publishers.pub_name, dbo.titles.title_id
FROM dbo.publishers INNER JOIN
dbo.titles ON dbo.publishers.pub_id = dbo.titles.pub_id INNER JOIN
dbo.sales ON dbo.titles.title_id = dbo.sales.title_id
WHERE (dbo.publishers.pub_name = 'Algodata Infosystems')
ORDER BY dbo.sales.ord_num***

![alt-У вас не загрузилось :( ](http://ipic.su/img/img7/fs/nomer3.1569523744.png "SQLServer5-6")

### 4. осуществить поиск публикаций, посвящённым базам данных.
***SELECT title
FROM dbo.titles
WHERE (title LIKE '%Database%')***

![alt-У вас не загрузилось :( ](http://ipic.su/img/img7/fs/Nomer4.1569523768.png "SQLServer5-6")

### 5. выбрать фамилии и инициалы авторов, и их публикации, отсортировав список по фамилии авторов.

***SELECT TOP (100) PERCENT dbo.authors.au_lname + ' ' + LEFT(dbo.authors.au_fname, 1) + '. ' AS inital, dbo.titles.title
FROM dbo.authors INNER JOIN
dbo.titleauthor ON dbo.authors.au_id = dbo.titleauthor.au_id INNER JOIN
dbo.titles ON dbo.titleauthor.title_id = dbo.titles.title_id
ORDER BY inital DESC***

![alt-У вас не загрузилось :( ](http://ipic.su/img/img7/fs/nomer5.1569523787.png "SQLServer5-6")

### 6. *выбрать публикации, которые заказывают чаще всего.

***SELECT TOP (1) dbo.titles.title, dbo.sales.title_id, COUNT(dbo.sales.ord_num) AS Expr1
FROM dbo.titles INNER JOIN
dbo.titleauthor ON dbo.titles.title_id = dbo.titleauthor.title_id INNER JOIN
dbo.sales ON dbo.titles.title_id = dbo.sales.title_id AND dbo.titleauthor.title_id = dbo.titles.title_id
GROUP BY dbo.titles.title, dbo.sales.title_id
ORDER BY Expr1 DESC***

![alt-У вас не загрузилось :( ](http://ipic.su/img/img7/fs/nomer6.1569523808.png "SQLServer5-6")

### 7. выбрать фамилии авторов, которые сотрудничают с заданным издательством.

***SELECT dbo.titles.pub_id, dbo.publishers.pub_name, dbo.authors.au_lname
FROM dbo.publishers INNER JOIN
dbo.titles ON dbo.publishers.pub_id = dbo.titles.pub_id INNER JOIN
dbo.titleauthor ON dbo.titles.title_id = dbo.titleauthor.title_id INNER JOIN
dbo.authors ON dbo.titleauthor.au_id = dbo.authors.au_id
WHERE (dbo.publishers.pub_name = 'Algodata Infosystems')***

![alt-У вас не загрузилось :( ](http://ipic.su/img/img7/fs/nomer7.1569523825.png "SQLServer5-6")

### 8. сформировать заказ из введённых Вами публикаций и вывести информацию о нём.

***ЕСЛИ НОМЕР ЗАКАЗА
SELECT ord_num, SUM(sum) AS sum_order
FROM dbo.[Задача 8]
GROUP BY ord_num
WHERE (ord_num = 'QA879.2')***

![alt-У вас не загрузилось :( ](http://ipic.su/img/img7/fs/nomer8.1569523897.png "SQLServer5-6")

### 9. вывести фамилию И.О. сотрудника, должность и возраст.

***SELECT fname,job_lvl,date AS inital, job_lvl, DATEDIFF(YY, hire_date, GETDATE()) AS work_time
FROM dbo.employee***

![alt-У вас не загрузилось :( ](http://ipic.su/img/img7/fs/9.1569524637.jpg "SQLServer5-6")

### 10. вывести фамилию И.О. и возраст самого молодого редактора.

***TOP(1) DATEDIFF(YY,hire_date,GETDATE()) as AGE,fname 
FROM dbo.employee
ORDER.BY age ASC***
![alt-У вас не загрузилось :( ](http://ipic.su/img/img7/fs/nomer10.1569523922.png "SQLServer5-6")

### 11. *вывести минимальный, средний и максимальный возраст сотрудников.

**SELECT MIN(DATEDIFF(YY, hire_date, GETDATE())) AS min, MAX(DATEDIFF(YY, hire_date, GETDATE())) AS max, AVG(DATEDIFF(YY, hire_date, GETDATE())) AS avg
FROM dbo.employee***
![alt-У вас не загрузилось :( ](http://ipic.su/img/img7/fs/saasdasd.1570196679.png "SQLServer5-6")

### 12. *вывести сколько публикаций осуществлено каждым издательством.

***SELECT dbo.publishers.pub_name, COUNT(dbo.titles.title) AS Expr1
FROM dbo.publishers INNER JOIN
dbo.titles ON dbo.publishers.pub_id = dbo.titles.pub_id
GROUP BY dbo.publishers.pub_name ***

![alt-У вас не загрузилось :( ](http://ipic.su/img/img7/fs/Snimok.1570196614.png "SQLServer5-6")

### 13. вывести размер гонорара, полученного каждым автором.
***SELECT dbo.authors.au_lname, dbo.authors.au_fname, SUM(dbo.titles.price * dbo.sales.qty / 100 * dbo.roysched.royalty) AS sum
FROM dbo.authors INNER JOIN
dbo.titleauthor ON dbo.authors.au_id = dbo.titleauthor.au_id INNER JOIN
dbo.titles ON dbo.titleauthor.title_id = dbo.titles.title_id INNER JOIN
dbo.roysched ON dbo.titles.title_id = dbo.roysched.title_id INNER JOIN
dbo.sales ON dbo.titles.title_id = dbo.sales.title_id
GROUP BY dbo.authors.au_lname, dbo.authors.au_fname***

![alt-У вас не загрузилось :( ](http://ipic.su/img/img7/fs/swassdasdaasd.1569525752.jpg "SQLServer5-6")

### 14. *вывести количество авторов, работающих с каждым издательством.

***SELECT dbo.publishers.pub_name, COUNT(dbo.authors.au_id) AS count_au
FROM dbo.authors INNER JOIN
dbo.titleauthor ON dbo.authors.au_id = dbo.titleauthor.au_id INNER JOIN
dbo.titles ON dbo.titleauthor.title_id = dbo.titles.title_id INNER JOIN
dbo.publishers ON dbo.titles.pub_id = dbo.publishers.pub_id
GROUP BY dbo.publishers.pub_name***

![alt-У вас не загрузилось :( ](http://ipic.su/img/img7/fs/fyvyvfyvfyvfyv.1570196809.png "SQLServer5-6")
