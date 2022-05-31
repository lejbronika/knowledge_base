# Инструкция оператора MSLA-принтера

!!! warning
    Соблюдайте технику безопасности!  
    При работе с фотополимерной смолой и изопропиловым спиртом надевайте респиратор и нитриловые перчатки.  
    При попадании фотополимерной смолы на кожу тщательно промойте это место под сильным потоком воды.

Для формирования очереди печати используется таблица [Production 1.1](https://docs.google.com/spreadsheets/d/1crMzWOvn4jlX4Vz1rxkNQ-YGdQovQkRZ4OENqAc8SqI/edit?usp=sharing).

### Печать 

!!! Tip
     Рекомендуется залить смолу и подготовить принтер к печати до подготовки файлов.  
     При работе с фотополимерной смолой важно следить за однородностью состава и отсуствием пузырьков воздуха. Непродолжительное время в состоянии покоя способствует их удалению. 

1. На листе `Orders` отфильтруйте список по статусам:
    - `Confirmed`
    - `Paid`
    - `PreProd`
    - `Ready to Print`
2. Выберите модели из описания заказа с наивысшим приоритетом.
3. Сформируйте из полученного списка файлы для печати. 

!!! Tip
    При добавлении моделей старайтесь максимально эффективно задейстовать доступную область печати.  
    Перед нарезкой не забудьте **проверить корректность расположения** всех моделей на платформе!

4. Для каждого файла печати на листе `MSLA Calc` в новой строке укажите данные о печати:
    - `ID печати` 
    - `ID принтера`
    - `NickName оператора`
    - `Краткое перечисление печатаемых моделей`
    - `ID смолы`
    - `Расчетное значение затраченной смолы [мл]`
    - `Количество слоев печати`
5. Сохраните софримрованные файлы для печати и проект `chitubox`, используя имя из колонки `File name`. 
6. Запустите печать. 
7. После завершения печати зафиксируйте фактическое время и оставьте платформу в положении под углом для стекания лишней смолы. 

!!! info
    Не тратьте время на ожидание, если планируется еще одна печать. Лишнюю смолу можно удалить пластиковым шпателем.  
    Минимальное время, на которое целесообразно переставлять платформу в положение под углом - **20 мин**.

### Постобработка

!!! info 
    Среднее время на постобработку моделей после печати: **30 мин**. 

1. Тщательно промойте платформу и модели в изопропиловом спирте первой очистки[^1]. 
2. Аккуратно удалите поддержки. При необходимости используйте *скальпель*, *пинцет* и *кусачки*.

!!! warning
    Соблюдайте технику безопасности! Надевайте защитные очки и следите, чтобы не рвались нитриловые перчатки.  
    Старатесь не повредить хрупкие детали моделей!

3. Промойте модели в ультразвоковой ванночке, используя чистый спирт. Необходимо убедиться в том, что на моделях не останется жидкой фотополимерной смолы. 
4. Тщательно просушите модели от спирта. 
5. Засветите модели ультфтраиолетовым светом с длиной волны 390-410 нМ со всех сторон.

!!! info 
    Время засветки моделей - **5-7 мин**.


[^1]: Спирт средней чистоты, допустим осадок на дне емкости. 