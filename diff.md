# Практика: Дифференцирование

## 1. Описание предметной области и сущностей
Algebra - основной класс символьного дифференцирования. Анализирует дерево выражений и строит производную по набору правил.

ExpressionElement - абстрактный элемент дерева выражений, общий предок всех узлов.

ConstantExpression - узел числовой константы, производная всегда равна 0.

ParameterExpression - узел переменной функции, производная по себе равна 1.

BinaryExpression - бинарная операция над двумя выражениями (сложение, вычитание, умножение).

MethodCallExpression - вызов математической функции, для которой применяется соответствующее правило дифференцирования.

DerivativeEngine - выполняет обработку узлов выражения и выбирает нужное правило вычисления производной.

IDerivativeRule - интерфейс правила дифференцирования отдельного типа выражений.

ConstantRule - правило для констант.

ParameterRule - правило для переменных.

BinaryRule - правило для бинарных операций.

TrigonometricRule - правило для тригонометрических функций Sin и Cos.

ExpressionCategory — перечисление категорий узлов дерева выражений.
## 2. Диаграмма классов (Mermaid)
```mermaid
classDiagram
    direction TB

    class Algebra {
        <<static>>
        +Differentiate(Expression~Func~double,double~~) Expression~Func~double,double~~
        -DifferentiateExpr(Expression, ParameterExpression) Expression
        -DifferentiateAdd(BinaryExpression, ParameterExpression) Expression
        -DifferentiateSub(BinaryExpression, ParameterExpression) Expression
        -DifferentiateMul(BinaryExpression, ParameterExpression) Expression
        -DifferentiateMethodCall(MethodCallExpression, ParameterExpression) Expression
        -Throw(Expression) Expression
    }

    class Expression {
        <<abstract>>
        +NodeType
    }

    class ConstantExpression {
        +Value
    }

    class ParameterExpression {
        +Name
    }

    class BinaryExpression {
        +Left
        +Right
    }

    class MethodCallExpression {
        +Method
        +Arguments
    }

    class ExpressionType {
        <<enumeration>>
        Add
        Subtract
        Multiply
        Call
        Constant
        Parameter
    }

    class Func~double,double~ {
        <<delegate>>
    }

    Expression <|-- ConstantExpression : наследует
    Expression <|-- ParameterExpression : наследует
    Expression <|-- BinaryExpression : наследует
    Expression <|-- MethodCallExpression : наследует

    BinaryExpression o-- Expression : левый операнд
    BinaryExpression o-- Expression : правый операнд

    MethodCallExpression o-- Expression : аргументы вызова

    Algebra ..> Expression : анализирует дерево выражений
    Algebra ..> BinaryExpression : применяет правила дифференцирования
    Algebra ..> MethodCallExpression : обрабатывает функции
    Algebra ..> ParameterExpression : определяет переменную

    Algebra ..> ExpressionType : проверяет вид узла
    Algebra ..> Func~double,double~ : возвращает производную

    Expression --> ExpressionType : определяет категорию узла
```
