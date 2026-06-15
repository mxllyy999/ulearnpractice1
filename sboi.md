# Практика: Сбои

## 1. Описание предметной области и сущностей
Система предназначена для выявления устройств, на которых происходили серьёзные отказы до заданного момента времени

Device - оборудование, содержит числовой идентификатор Id и название Name

Failure - запись о сбое, включает тип неисправности FailureType, идентификатор устройства DeviceId и дату возникновения Date. Предоставляет метод IsSerious()

FailureType - перечисление типов сбоев: UnexpectedShutdown (0), ShortNonResponding (1), HardwareFailure (2), ConnectionProblems (3)

ReportMaker - класс, реализующий метод FindDevicesFailedBeforeDate (принимает дату, список Failure и список Device) и метод FindDevicesFailedBeforeDateObsolete

## 2. Диаграмма классов (Mermaid)
```mermaid
classDiagram
    class FailureType {
        <<enumeration>>
        UnexpectedShutdown
        ShortNonResponding
        HardwareFailure
        ConnectionProblems
    }

    class Failure {
        -FailureType Type
        -int DeviceId
        -DateTime Date
        +Failure(FailureType type, int deviceId, DateTime date)
        +bool IsSerious()
    }

    class Device {
        -int Id
        -string Name
        +Device(int id, string name)
    }

    class ReportMaker {
        +List~string~ FindDevicesFailedBeforeDate(DateTime targetDate, List~Failure~ failures, List~Device~ devices)
        +List~string~ FindDevicesFailedBeforeDateObsolete(int day, int month, int year, int[] failureTypes, int[] deviceId, object[][] times, List~Dictionary~string, object~~ devices)
    }

    ReportMaker ..> Failure : использует
    ReportMaker ..> Device : использует
    Failure --> FailureType : содержит
