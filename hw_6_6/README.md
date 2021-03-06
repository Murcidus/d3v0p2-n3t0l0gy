# Домашнее задание к занятию "6.6. Troubleshooting"

## Задача 1

Перед выполнением задания ознакомьтесь с документацией по [администрированию MongoDB](https://docs.mongodb.com/manual/administration/).

Пользователь (разработчик) написал в канал поддержки, что у него уже 3 минуты происходит CRUD операция в MongoDB и её 
нужно прервать. 

Вы как инженер поддержки решили произвести данную операцию:
- напишите список операций, которые вы будете производить для остановки запроса пользователя
- предложите вариант решения проблемы с долгими (зависающими) запросами в MongoDB

---
### Ответ

**Список операций для остановки запроса пользователя:**  
`db.currentOp({"active" : true, "secs_running" : { "$gt" : 180 } })` -- получение подвисшей операции  
`db.killOp(<opId>)` -- прерывает операцию  


**Вариант решения проблемы с долгими (зависающими) запросами:**   
Использовать в запросах параметер `.maxTimeMS()`, который ограничивает время выполнения операции 

---

## Задача 2

Перед выполнением задания познакомьтесь с документацией по [Redis latency troobleshooting](https://redis.io/topics/latency).

Вы запустили инстанс Redis для использования совместно с сервисом, который использует механизм TTL. 
Причем отношение количества записанных key-value значений к количеству истёкших значений есть величина постоянная и
увеличивается пропорционально количеству реплик сервиса. 

При масштабировании сервиса до N реплик вы увидели, что:
- сначала рост отношения записанных значений к истекшим
- Redis блокирует операции записи

Как вы думаете, в чем может быть проблема?


---
### Ответ

Одновременно заканчивается срок жизни у большого количества ключей и процесс активного удаления не успевает их удалить и зацикливается.

---

## Задача 3

Перед выполнением задания познакомьтесь с документацией по [Common Mysql errors](https://dev.mysql.com/doc/refman/8.0/en/common-errors.html).

Вы подняли базу данных MySQL для использования в гис-системе. При росте количества записей, в таблицах базы,
пользователи начали жаловаться на ошибки вида:
```
InterfaceError: (InterfaceError) 2013: Lost connection to MySQL server during query u'SELECT..... '
```

Как вы думаете, почему это начало происходить и как локализовать проблему?

Какие пути решения данной проблемы вы можете предложить?

---
### Ответ

Запрос возвращает большое количество данных.  
Следует попробовать увеличить значение `net_read_timeout` с 30 секунд, по умолчанию, до 60 секунд или более, достаточного для завершения передачи данных.

Если дополнительно возникает ошибка `ER_NET_PACKET_TOO_LARGE`, то это значит что имеется проблема со значениями BLOB, превышающими `max_allowed_packet` и нужно увеличить `max_allowed_packet` 

---


## Задача 4

Перед выполнением задания ознакомтесь со статьей [Common PostgreSQL errors](https://www.percona.com/blog/2020/06/05/10-common-postgresql-errors/) из блога Percona.

Вы решили перевести гис-систему из задачи 3 на PostgreSQL, так как прочитали в документации, что эта СУБД работает с 
большим объемом данных лучше, чем MySQL.

После запуска пользователи начали жаловаться, что СУБД время от времени становится недоступной. В dmesg вы видите, что:

`postmaster invoked oom-killer`

Как вы думаете, что происходит?

Как бы вы решили данную проблему?

---
### Ответ

**Как вы думаете, что происходит?**  
Недостаточно памяти.

**Как бы вы решили данную проблему?**  
- увеличил ОЗУ на сервере;
- убрал, если они есть, ненужные процессы или уменьшил потребляемую ими память;
- уменьшил потребляемую PostgreSQL память (`shared_buffers`, `temp_buffers` и др.).



---