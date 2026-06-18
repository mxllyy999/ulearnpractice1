# Практика: HoMM

## 1. Описание предметной области и сущностей
В игре есть объекты карты и система взаимодействий, через которую игрок взаимодействует с ними. Объекты могут иметь владельца, участвовать в бою или содержать награду.

Player описывает игрока. У него есть Id, а также логика боя и взаимодействия: он может проверить, способен ли победить армию через CanBeat(Army), получать награду через Consume(Treasure) и погибать через Die(). Игрок сравнивает свою силу с армией и получает ресурсы из сокровищ.

Army задаёт боевую силу через поле Power, которое используется при сражениях. Treasure хранит количество ресурсов в поле Value, которое может получить игрок.

MapObject - абстрактный объект карты. От него наследуются все игровые объекты, с которыми может взаимодействовать игрок.

Interaction - система управления взаимодействиями. Метод Make(Player, MapObject) связывает игрока с объектами карты и работает через интерфейсы взаимодействия, не завися от конкретного типа объекта.

IOwned описывает объекты с владельцем (Owner). Его реализуют:

Dwelling - объект с владельцем и параметром роста (Growth)
Mine - объект с владельцем и ежедневным доходом (DailyIncome)

ICombat описывает объекты, связанные с боем и имеющие армию (Army). Его реализуют:

Wolves - противники со стаей (PackSize)
Creeps - нейтральные противники с именем (Name)
Mine - объект, который необходимо захватить через сражение

ILoot описывает объекты, содержащие награду (Treasure). Его реализуют:

ResourcePile - источник ресурсов, который можно собрать
Creeps - противники, выдающие награду после победы
Mine - объект, содержащий награду после захвата

Связи взаимодействия:

Interaction работает с объектами карты через MapObject, назначает владельца через IOwned, проверяет бой через ICombat и выдаёт награду через ILoot.
Player взаимодействует с Army при проверке боя и получает Treasure в качестве награды.

## 2. Диаграмма классов (Mermaid)

```mermaid
classDiagram
    direction TB

    class Player {
        +Id: int
        +CanBeat(Army): bool
        +Consume(Treasure)
        +Die()
    }

    class MapObject {
        <<abstract>>
        +Interact(Player)
    }

    class IOwned {
        <<interface>>
        +Owner: int
    }

    class ICombat {
        <<interface>>
        +Army: Army
    }

    class ILoot {
        <<interface>>
        +Treasure: Treasure
    }

    class Dwelling {
        +Owner: int
        +Growth: int
    }

    class Mine {
        +Owner: int
        +DailyIncome: int
    }

    class Creeps {
        +Treasure: Treasure
        +Name: string
    }

    class Wolves {
        +Army: Army
        +PackSize: int
    }

    class ResourcePile {
        +Treasure: Treasure
        +IsCollected: bool
    }

    class Army {
        +Power: int
    }

    class Treasure {
        +Value: int
    }

    class Interaction {
        +Make(Player, MapObject)
    }

    MapObject <|-- Dwelling : наследование
    MapObject <|-- Mine : наследование
    MapObject <|-- Creeps : наследование
    MapObject <|-- Wolves : наследование
    MapObject <|-- ResourcePile : наследование

    IOwned <|.. Dwelling : реализует
    IOwned <|.. Mine : реализует

    ICombat <|.. Mine : реализует
    ICombat <|.. Creeps : реализует
    ICombat <|.. Wolves : реализует

    ILoot <|.. Mine : реализует
    ILoot <|.. Creeps : реализует
    ILoot <|.. ResourcePile : реализует

    Mine *-- Army : содержит армию
    Mine *-- Treasure : содержит сокровище

    Creeps *-- Army : содержит армию
    Creeps *-- Treasure : содержит добычу

    Wolves *-- Army : содержит стаю

    ResourcePile *-- Treasure : содержит ресурсы

    Interaction ..> MapObject : обрабатывает объект
    Interaction ..> IOwned : назначает владельца
    Interaction ..> ICombat : проверяет бой
    Interaction ..> ILoot : выдаёт награду

    Player ..> Army : сравнивает силы
    Player ..> Treasure : получает ресурсы
```
