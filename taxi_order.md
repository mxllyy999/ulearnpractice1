# Практика: TaxiOrder

## 1. Описание предметной области и сущностей

TaxiApi - сервисный класс, который управляет жизненным циклом заказа такси и делегирует всю бизнес-логику доменной сущности TaxiOrder.

TaxiOrder - основная доменная сущность заказа, содержащая состояние заказа, клиента, маршрут, назначенного водителя и временные метки всех этапов (создание, назначение, поездка, завершение, отмена), а также реализующая всю бизнес-логику изменения состояния.

Driver - доменная сущность водителя, содержащая имя (PersonName) и автомобиль (Car), а также поведение для получения полной информации.

Car - value object, описывающий автомобиль (модель, цвет, номер).

PersonName - value object, представляющий имя и фамилию человека.

Address - value object, описывающий адрес (улица и дом).

DriversRepository - инфраструктурный компонент, предоставляющий данные о водителе по id в виде DriverInfo (DTO) и не содержащий бизнес-логики.

DriverInfo - DTO объект для передачи данных о водителе из репозитория.

TaxiOrderStatus - перечисление состояний заказа (WaitingForDriver, WaitingCarArrival, InProgress, Finished, Canceled).

## 2. Диаграмма классов (Mermaid)
```mermaid
classDiagram
    direction LR

    class TaxiApi {
        -DriversRepository repo
        -Func~DateTime~ clock
        -int idSeq
        +CreateOrderWithoutDestination(...)
        +UpdateDestination(order, street, building)
        +AssignDriver(order, driverId)
        +UnassignDriver(order)
        +Cancel(order)
        +StartRide(order)
        +FinishRide(order)
        +GetDriverFullInfo(order)
        +GetShortOrderInfo(order)
    }

    class TaxiOrder {
        <<Entity~int~>>
        -TaxiOrderStatus status
        -DateTime createdAt
        -DateTime assignedAt
        -DateTime rideStartedAt
        -DateTime rideFinishedAt
        -DateTime cancelledAt

        -PersonName client
        -Address from
        -Address to
        -Driver driver

        +UpdateDestination(Address)
        +AssignDriver(Driver, DateTime)
        +UnassignDriver()
        +Cancel(DateTime)
        +StartRide(DateTime)
        +FinishRide(DateTime)
        +GetShortOrderInfo() string
        +GetDriverFullInfo() string
        +GetLastProgressTime() DateTime
    }

    class Driver {
        <<Entity~int~>>
        +PersonName Name
        +Car Car
        +GetFullInfo() string
    }

    class Car {
        <<ValueType~Car~>>
        +string Model
        +string Color
        +string PlateNumber
    }

    class PersonName {
        <<ValueType~PersonName~>>
        +string FirstName
        +string LastName
    }

    class Address {
        <<ValueType~Address~>>
        +string Street
        +string Building
    }

    class DriversRepository {
        +GetDriverById(int) DriverInfo
    }

    class DriverInfo {
        +int Id
        +string FirstName
        +string LastName
        +string CarModel
        +string CarColor
        +string CarPlateNumber
    }

    class TaxiOrderStatus {
        <<enumeration>>
        WaitingForDriver
        WaitingCarArrival
        InProgress
        Finished
        Canceled
    }

    TaxiApi --> DriversRepository : использует источник данных
    TaxiApi --> TaxiOrder : управляет заказом

    TaxiOrder *-- PersonName : клиент
    TaxiOrder *-- Address : маршрут
    TaxiOrder o-- Driver : назначенный водитель

    Driver *-- Car : транспорт
    Driver *-- PersonName : имя

    DriversRepository ..> DriverInfo : возвращает DTO
    TaxiOrder --> TaxiOrderStatus : состояние
    ```
    Driver *-- PersonName : имя

    DriversRepository ..> DriverInfo : возвращает DTO
    TaxiOrder --> TaxiOrderStatus : состояние
