# Система Доставки Еды

Система доставки еды представляет собой комплексный сервис, включающий три роли: Администратор, Пользователь и Курьер.

## Пользователь

Пользовательская часть системы ориентирована на удобство и интуитивность. Пользователь может:
- Просматривать ассортимент доступных товаров
- Детально изучать информацию о каждом продукте, включая описание, цену и отзывы
- Добавлять товары в корзину, корректировать количество и наименование товаров
- Формировать заказ, указывая адрес доставки и способ оплаты
- Отслеживать статус своего заказа в реальном времени
- Получать уведомления о текущем состоянии доставки

## Курьер

Курьерская часть системы предназначена для упрощения работы курьеров. Курьеры могут:
- Видеть список назначенных им заказов
- Просматривать детальную информацию о каждом заказе, включая адрес доставки и контактную информацию пользователя
- Отмечать заказ как выполненный после успешной доставки
- Доступ к отчетам о выполненных заказах, где отображается сумма заработанных денег за каждый выполненный заказ
Это позволяет курьерам легко отслеживать свой доход и планировать рабочее время.

## Администратор

Администратор играет ключевую роль в обеспечении качества сервиса и решении возникающих проблем. Администратор может:
- Иметь доступ к истории заказов всех пользователей и курьеров, что позволяет анализировать и решать проблемы, связанные с доставкой
- Просматривать статистику работы сервиса, включая количество выполненных заказов, среднее время доставки, оценки пользователей и другие важные показатели
Это помогает в стратегическом планировании и улучшении качества сервиса.

## UML Диаграммы
### Диаграмма использований
![Диаграмма использований](UML/1.jpg)
скрипт PlantUML
```plantuml
@startuml
left to right direction
actor "Пользователь" as User
actor "Курьер" as Courier
actor "Администратор" as Admin

rectangle DeliverySystem {
    User --> (Просмотр информации о товаре)
    User --> (Добавление товара в корзину)
    User --> (Формирование заказа)

    Courier --> (Просмотр информации о заказе)
    Courier --> (Просмотр отчета о заработке)

    Admin --> (Просмотр истории заказов)
    Admin --> (Просмотр статистики работы сервиса)
}
@enduml```

### Диаграмма последовательности
![Диаграмма последовательности](UML/2.jpg)
скрипт PlantUML
```@startuml
actor Пользователь as User
participant "Приложение" as App
participant "Сервер" as Server
participant "База данных" as DB
User -> App: Просмотр информации о товаре
activate App
App -> Server: Запрос информации о товаре
activate Server
Server -> DB: Получить информацию о товаре
activate DB
DB -> Server: Информация о товаре
deactivate DB
Server -> App: Информация о товаре
deactivate Server
App -> User: Отображение информации о товаре
deactivate App
User -> App: Добавление товара в корзину
activate App
App -> Server: Обновить корзину
activate Server
Server -> DB: Сохранить данные корзины
activate DB
DB -> Server: Подтверждение сохранения
deactivate DB
Server -> App: Подтверждение обновления корзины
deactivate Server
App -> User: Товар добавлен в корзину
deactivate App
User -> App: Формирование заказа
activate App
App -> Server: Создать заказ
activate Server
Server -> DB: Сохранить заказ и данные пользователя
activate DB
DB -> Server: Подтверждение сохранения
deactivate DB
Server -> App: Подтверждение создания заказа
deactivate Server
App -> User: Заказ сформирован
deactivate App
@enduml```

### Диаграмма состояний заказа
![Диаграмма состояний заказа](UML/3.jpg)
скрипт PlantUML
```@startuml
[*] --> Заказ_сформирован
Заказ_сформирован : Статус изменен на "Заказ сформирован"
Заказ_сформирован --> Курьер_назначен : Назначить курьера
Курьер_назначен : Статус изменен на "Курьер назначен"
Курьер_назначен --> Курьер_забрал_заказ : Курьер забрал заказ
Курьер_забрал_заказ : Статус изменен на "Курьер забрал заказ"
Курьер_забрал_заказ --> Курьер_отдал_заказ : Курьер отдал заказ
Курьер_отдал_заказ : Статус изменен на "Курьер отдал заказ"
Курьер_отдал_заказ --> [*]
Заказ_сформирован --> Заказа_нет_в_наличии : Проверка наличия
Заказа_нет_в_наличии : Статус изменен на "Заказа нет в наличии"
Заказа_нет_в_наличии --> [*]
@enduml```

### Диаграмма деятельности
![Диаграмма деятельности](UML/4.jpg)
скрипт PlantUML
```@startuml
|Пользователь|
start
:Выбор товаров;
:Добавление товаров в корзину;
:Формирование заказа;
:Отправка заказа;
|Приложение|
:Получение заказа;
:Проверка наличия товаров;
if (Все товары в наличии?) then (да)
    :Создание заказа;
    :Назначение курьера;
:Отправка уведомления курьеру;
else (нет)
    :Уведомление пользователя о нехватке товара;
    :Отмена заказа;
    stop
endif
|Курьер|
:Получение уведомления о заказе;
:Просмотр информации о заказе;
:Забор заказа;
:Доставка заказа пользователю;
:Подтверждение доставки;
|Приложение|
:Получение подтверждения доставки;
:Обновление статуса заказа;
|Администратор|
:Просмотр истории заказов;
:Анализ и решение проблем с заказами (если есть);
stop
@enduml```

### Диаграмма классов для системы
![Диаграмма классов для системы](UML/5.jpg)
скрипт PlantUML
```@startuml
package "Система доставки еды" {
    class Пользователь {
        - id: int
        - имя: string
        - адрес: string
        - телефон: string
        + просматриватьТовар(): void
        + добавлятьТоварВКорзину(): void
        + формироватьЗаказ(): void
}
    class Курьер {
        - id: int
  - имя: string
        - телефон: string
        + просматриватьИнформациюОЗаказе(): void
        + просматриватьОтчетОЗаработке(): void
}
    class Администратор {
        - id: int
        - имя: string
        + просматриватьИсториюЗаказов(): void
        + просматриватьСтатистикуРаботыСервиса(): void
}
    class Товар {
        - id: int
        - название: string
        - описание: string
        - цена: float
        + получитьИнформацию(): void
}
    class Заказ {
        - id: int
        - дата: datetime
        - статус: string
        + создать(): void
        + изменитьСтатус(): void
}
    Пользователь --> Заказ : формирует
    Пользователь --> Товар : просматривает
    Заказ --> Товар : содержит
    Курьер --> Заказ : доставляет
    Администратор --> Заказ : просматривает
    Администратор --> Курьер : управляет
    Администратор --> Пользователь : управляет
} @enduml```
# tz3
