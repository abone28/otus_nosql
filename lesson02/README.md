Классификация MongoDB, MS SQL, Apache Cassandra по классам CAP
==============================================================

MongoDB
-------

MongoDB по-умолчанию относят к CP.

Только на один основной узел отвечает за запись. Он выбирается
кворумом доступной части кластера. Пока кластер не распался и
чтение и клиенты работают с основным узлом C обеспечивается.

Если клиент также требует, подтверждения записи на большинство
узлов то, шансы добиться C при потере связи между узлами, хотя,
возможны варианты.

Если клиент не требует записи на большинство узлов и кластер
распадется так, что бывший leader останется в части без
кворума, а кворум достанется отставшим репликам, есть вероятность,
что часть записанных данных будет потеряна и от CP останется только P.

От A в части записи MongoDB официально отказался. При разделении
кластера, система не принимает новые данные пока не будет
выбран новый лидер. Выборы занимают до 12 секунд.


MS SQL
------

Реляционные БД в одноузловом варианте, грубо, относят к CA.

В случае Always on, наверное, можно отнести к CP, по крайней
мере, пока не нарушена синхронизация или пока работаем с основным
узлом. Если читать с отставшей реплики C уже не будет, и запись
реплика сделать не даст.


Apache Cassandra
----------------

Apache Cassandra относят к AP. Все узлы обеспечивают как
чтение, так и запись, поэтому потеря связи между узлами меньше
сказывается на доступности. Нет прекращения записи пока жив
координатор и хотябы одна реплика шарда.

Относительно C, по умолчанию поддерживается eventual consistency,
но можно выкрутить настройки и ценой снижения доступности и
увеличения задержек получить более сильные гарантии. Вплоть до
того, что отказ любого из узлов будет выводить из строя весь
кластер, зато данные будут целы.
