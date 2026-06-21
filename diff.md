# Практика: Дифференцирование

## 1. Описание предметной области и сущностей
Algebra - основной класс символьного дифференцирования. Выполняет анализ дерева выражений и строит выражение производной по правилам математического дифференцирования.

Expression - абстрактный базовый класс для всех узлов дерева выражений.

ConstantExpression - узел числовой константы. При дифференцировании возвращает нулевое значение.

ParameterExpression - узел переменной функции. Производная переменной по самой себе равна единице.

UnaryExpression - узел унарной операции над выражением. Используется для обработки преобразований и отрицания.

BinaryExpression - узел бинарной операции над двумя выражениями. Представляет сложение, вычитание и умножение.

MethodCallExpression - узел вызова функции. Используется для обработки математических функций и применения правила цепочки.

Math - библиотечный класс математических функций. Используется для построения производных функций Sin и Cos.

ExpressionCategory - перечисление категорий узлов дерева выражений.
## 2. Диаграмма классов (Mermaid)
```mermaid
classDiagram
    direction TB

    class Algebra {
        <<static>>
        +Differentiate(...)
        -DifferentiateExpr(...)
        -DifferentiateUnary(...)
        -DifferentiateAdd(...)
        -DifferentiateSub(...)
        -DifferentiateMul(...)
        -DifferentiateMethodCall(...)
        -Throw(...)
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

    class UnaryExpression {
        +Operand
    }

    class BinaryExpression {
        +Left
        +Right
    }

    class MethodCallExpression {
        +Method
        +Arguments
    }

    class Math {
        <<framework>>
    }

    Expression <|-- ConstantExpression : является видом
    Expression <|-- ParameterExpression : является видом
    Expression <|-- UnaryExpression : является видом
    Expression <|-- BinaryExpression : является видом
    Expression <|-- MethodCallExpression : является видом

    UnaryExpression o-- Expression : использует операнд

    BinaryExpression o-- Expression : содержит левую часть
    BinaryExpression o-- Expression : содержит правую часть

    MethodCallExpression o-- Expression : содержит параметры вызова

    MethodCallExpression ..> Math : обращается к Sin и Cos

    Algebra ..> Expression : выполняет рекурсивный разбор
    Algebra ..> ConstantExpression : обрабатывает константы
    Algebra ..> ParameterExpression : вычисляет производную переменной
    Algebra ..> UnaryExpression : раскрывает преобразования
    Algebra ..> BinaryExpression : применяет правила операций
    Algebra ..> MethodCallExpression : использует правило цепочки
```
