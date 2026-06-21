# Практика: TaxiOrder

## 1. Описание предметной области и сущностей

TaxiService - основной сервис управления поездками. Создаёт заказы, назначает водителей, изменяет маршрут и переводит поездку между состояниями.

Ride - сущность поездки. Хранит информацию о пассажире, маршруте, назначении водителя, текущем состоянии и истории выполнения.

Passenger - пассажир, оформивший поездку.

Driver - водитель такси. Содержит персональные данные и сведения об автомобиле.

DriverAssignment - информация о назначении водителя на поездку. Хранит ссылку на водителя и время назначения.

Route - маршрут поездки, содержащий точку отправления и точку назначения.

RideTimeline - история жизненного цикла поездки. Содержит даты основных событий заказа.

DriverCatalog - источник данных о водителях. Используется для поиска водителя по идентификатору.

TripWorkflow - компонент управления состояниями поездки. Выполняет переходы между этапами выполнения заказа.

Vehicle - автомобиль водителя.

PersonName - имя и фамилия человека.

Location - адрес маршрута.

TripStage - перечисление состояний поездки.

## 2. Диаграмма классов (Mermaid)
```mermaid
classDiagram
    direction LR

    class TaxiApi {
        -DriversRepository repo
        -TaxiOrderWorkflow workflow
        -Func~DateTime~ clock

        +CreateOrderWithoutDestination(...)
        +AssignDriver(order, driverId)
        +UpdateDestination(order, street, building)
        +Cancel(order)
        +StartRide(order)
        +FinishRide(order)
    }

    class TaxiOrder {
        +int Id

        -PersonName client
        -Address route
        -DriverAssignment assignment
        -OrderTimeline timeline
        -TaxiOrderStatus status

        +UpdateDestination(Address)
        +AssignDriver(Driver)
        +UnassignDriver()
    }

    class Driver {
        +PersonName Name
        +Car Car
    }

    class DriverAssignment {
        +Driver Driver
        +DateTime AssignedAt
    }

    class OrderTimeline {
        +DateTime CreationTime
        +DateTime DriverAssignmentTime
        +DateTime StartRideTime
        +DateTime FinishRideTime
        +DateTime CancelTime
    }

    class Address {
        +string Street
        +string Building
    }

    class PersonName {
        +string FirstName
        +string LastName
    }

    class Car {
        +string Model
        +string Color
        +string PlateNumber
    }

    class DriversRepository {
        +GetDriverById(int) DriverInfo
    }

    class DriverInfo {
        +string FirstName
        +string LastName
        +string CarModel
        +string CarColor
        +string CarPlateNumber
    }

    class TaxiOrderWorkflow {
        +Assign(TaxiOrder)
        +Cancel(TaxiOrder)
        +Start(TaxiOrder)
        +Finish(TaxiOrder)
    }

    class TaxiOrderStatus {
        <<enumeration>>
        WaitingForDriver
        WaitingCarArrival
        InProgress
        Finished
        Canceled
    }

    TaxiApi --> DriversRepository : получает водителей
    TaxiApi --> TaxiOrderWorkflow : изменяет состояние
    TaxiApi --> TaxiOrder : управляет заказом

    TaxiOrder *-- OrderTimeline : история заказа
    TaxiOrder *-- PersonName : клиент

    TaxiOrder o-- Address : адреса
    TaxiOrder o-- DriverAssignment : назначение

    DriverAssignment --> Driver : водитель

    Driver *-- PersonName : ФИО
    Driver *-- Car : автомобиль

    DriversRepository ..> DriverInfo : данные водителя

    TaxiOrder --> TaxiOrderStatus : состояние
```
