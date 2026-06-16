# Практика: Геомeтрия-2

## 1. Описание предметной области и сущностей

В системе представлены трёхмерные геометрические фигуры. Для выполнения операций над ними применяется паттерн Посетитель, что позволяет добавлять новые операции без изменения классов фигур.

IVisitor - обобщённый интерфейс. Определяет методы Visit для каждого типа фигуры. Возвращает результат типа TOut.

Body - абстрактный базовый класс. Хранит позицию фигуры. Содержит метод Accept для приёма посетителя.

Ball - шар. Наследник Body. Имеет радиус.

RectangularCuboid - параллелепипед. Наследник Body. Имеет размеры по трём осям.

Cylinder - цилиндр. Наследник Body. Имеет радиус и высоту.

CompoundBody - составное тело. Наследник Body. Содержит коллекцию вложенных тел.

Vector3 - структура координат. Хранит X, Y, Z.

BoundingBoxVisitor - посетитель. Вычисляет минимальный ограничивающий параллелепипед.
BoxifyVisitor - посетитель. Заменяет простые фигуры на параллелепипеды.

## 2. Диаграмма классов (Mermaid)

```mermaid
classDiagram
    direction TB
    
    class IVisitor~TOut~ {
        <<interface>>
        +Visit(Ball ballObject) TOut
        +Visit(RectangularCuboid cuboidShape) TOut
        +Visit(Cylinder cylinderObject) TOut
        +Visit(CompoundBody compoundObject) TOut
    }

    class Body {
        <<abstract>>
        #Vector3 Position
        +Accept~TOut~(IVisitor~TOut~ visitorInstance) TOut*
    }

    class Ball {
        +double Radius
        +Accept~TOut~(IVisitor~TOut~ visitorInstance) TOut
    }

    class RectangularCuboid {
        +double SizeX
        +double SizeY
        +double SizeZ
        +Accept~TOut~(IVisitor~TOut~ visitorInstance) TOut
    }

    class Cylinder {
        +double SizeZ
        +double Radius
        +Accept~TOut~(IVisitor~TOut~ visitorInstance) TOut
    }

    class CompoundBody {
        +IReadOnlyList~Body~ Parts
        +Accept~TOut~(IVisitor~TOut~ visitorInstance) TOut
    }

    class BoundingBoxVisitor {
        +Visit(Ball ballObject) RectangularCuboid
        +Visit(RectangularCuboid cuboidShape) RectangularCuboid
        +Visit(Cylinder cylinderObject) RectangularCuboid
        +Visit(CompoundBody compoundObject) RectangularCuboid
    }

    class BoxifyVisitor {
        +Visit(Ball ballObject) Body
        +Visit(RectangularCuboid cuboidShape) Body
        +Visit(Cylinder cylinderObject) Body
        +Visit(CompoundBody compoundObject) Body
    }

    class Vector3 {
        +double X
        +double Y
        +double Z
    }

    Body <|-- Ball : наследует
    Body <|-- RectangularCuboid : наследует
    Body <|-- Cylinder : наследует
    Body <|-- CompoundBody : наследует

    IVisitor~TOut~ <|.. BoundingBoxVisitor : реализует
    IVisitor~TOut~ <|.. BoxifyVisitor : реализует

    Body *-- Vector3 : композиция

    CompoundBody o-- Body : агрегация

    BoundingBoxVisitor ..> RectangularCuboid : создаёт
    BoxifyVisitor ..> Body : возвращает
    BoxifyVisitor ..> BoundingBoxVisitor : использует

    Ball ..> IVisitor~TOut~ : зависит
    RectangularCuboid ..> IVisitor~TOut~ : зависит
    Cylinder ..> IVisitor~TOut~ : зависит
    CompoundBody ..> IVisitor~TOut~ : зависит
```
