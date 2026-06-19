# Практика: TaxiOrder

## 1. Описание предметной области и сущностей

TaxiService - сервис управления поездками такси. Создаёт новые поездки, назначает и снимает водителей, изменяет маршрут и переводит поездку между этапами выполнения.

Ride - Основная сущность поездки. Хранит информацию о пассажире, маршруте, назначенном водителе, текущем состоянии и истории выполнения заказа.

Passenger - пассажир, который оформил поездку. Содержит персональные данные клиента.

Driver - водитель такси. Хранит сведения о человеке и используемом автомобиле.

DriverAssignment - объект назначения водителя на поездку. Содержит ссылку на водителя и время назначения.

Route - маршрут поездки. Описывает начальную и конечную точки следования.

RideTimeline - история жизненного цикла поездки. Содержит даты создания, назначения водителя, начала, завершения или отмены поездки.

DriverCatalog - источник данных о водителях. Используется для поиска и получения информации о доступных водителях.

TripWorkflow - компонент бизнес-логики переходов между состояниями поездки. Отвечает за правила назначения, отмены, начала и завершения поездки.

Vehicle - автомобиль водителя.

PersonName - фамилия и имя человека.

Location - адресная точка маршрута.

TripStage - перечисление этапов жизненного цикла поездки

## 2. Диаграмма классов (Mermaid)
```mermaid
classDiagram
    direction LR

    class TaxiService {
        -DriverCatalog catalog
        -TripWorkflow workflow
        -Func~DateTime~ clock

        +CreateRide(...)
        +AssignDriver(ride, driverId)
        +ChangeDestination(ride, street, building)
        +CancelRide(ride)
        +StartRide(ride)
        +FinishRide(ride)
    }

    class Ride {
        +int Number
        -Passenger passenger
        -Route route
        -DriverAssignment assignment
        -RideTimeline timeline
        -TripStage stage

        +UpdateRoute(Route)
        +Assign(Driver)
        +Unassign()
    }

    class Passenger {
        +PersonName Name
    }

    class Driver {
        +PersonName Name
        +Vehicle Vehicle
    }

    class DriverAssignment {
        +Driver AssignedDriver
        +DateTime AssignedAt
    }

    class Route {
        +Location StartPoint
        +Location DestinationPoint
    }

    class RideTimeline {
        +DateTime CreatedAt
        +DateTime AssignedAt
        +DateTime StartedAt
        +DateTime FinishedAt
        +DateTime CancelledAt
    }

    class DriverCatalog {
        +FindDriver(int) Driver
    }

    class TripWorkflow {
        +Assign(Ride)
        +Cancel(Ride)
        +Start(Ride)
        +Finish(Ride)
    }

    class Vehicle {
        +string Model
        +string Color
        +string Registration
    }

    class PersonName {
        +string FirstName
        +string LastName
    }

    class Location {
        +string Street
        +string Building
    }

    class TripStage {
        <<enumeration>>
        Created
        Assigned
        Active
        Finished
        Cancelled
    }

    TaxiService --> DriverCatalog : ищет водителей
    TaxiService --> TripWorkflow : меняет состояние
    TaxiService --> Ride : управляет поездкой

    Ride *-- Passenger : клиент
    Ride *-- Route : маршрут
    Ride *-- RideTimeline : история
    Ride o-- DriverAssignment : назначение

    DriverAssignment --> Driver : водитель

    Driver *-- Vehicle : автомобиль
    Driver *-- PersonName : данные

    Passenger *-- PersonName : данные

    Route *-- Location : точки маршрута

    Ride --> TripStage : текущее состояние
```
