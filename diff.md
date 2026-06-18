# Практика: Дифференцирование

## 1. Описание предметной области и сущностей
Algebra - основной класс дифференцирования выражений. Разбирает Expression Tree и строит производную с использованием правил сложения, умножения и цепного правила.

ExpressionNode - абстрактное выражение дерева, базовый элемент всех типов выражений.

ConstantExpression - числовая константа, производная равна 0.

ParameterExpression - переменная функции (x), производная равна 1.

BinaryExpression - бинарное выражение (сложение, вычитание, умножение).

UnaryExpression - унарное выражение (например Convert), используется для раскрытия вложенных конструкций.

MethodCallExpression - вызов математической функции (Sin, Cos и др.).

MemberExpression - доступ к членам выражения (например ToString, Length), приводит к ошибке.

MathFunctions - содержит поддерживаемые математические функции Sin и Cos.

ExpressionKind — перечисление типов операций выражения.
## 2. Диаграмма классов (Mermaid)
```mermaid
classDiagram
    direction LR

    class Algebra {
        <<static>>
        +Differentiate(function Expression~Func~double,double~~) Expression~Func~double,double~~
        -DifferentiateExpr(expr Expression, x ParameterExpression) Expression
        -DifferentiateAdd(BinaryExpression, x ParameterExpression) Expression
        -DifferentiateSub(BinaryExpression, x ParameterExpression) Expression
        -DifferentiateMul(BinaryExpression, x ParameterExpression) Expression
        -DifferentiateMethodCall(MethodCallExpression, x ParameterExpression) Expression
        -DifferentiateUnary(UnaryExpression, x ParameterExpression) Expression
        -Throw(Expression) Expression
    }

    class ExpressionNode {
        <<abstract>>
    }

    class ConstantExpression
    class ParameterExpression
    class BinaryExpression
    class MethodCallExpression
    class UnaryExpression
    class MemberExpression

    class MathFunctions {
        <<static>>
        +Sin(double) double
        +Cos(double) double
    }

    class ExpressionKind {
        <<enumeration>>
        Add
        Subtract
        Multiply
        Call
        Convert
        Constant
        Parameter
    }

  
    ExpressionNode <|-- ConstantExpression : наследует
    ExpressionNode <|-- ParameterExpression : наследует
    ExpressionNode <|-- BinaryExpression : наследует
    ExpressionNode <|-- MethodCallExpression : наследует
    ExpressionNode <|-- UnaryExpression : наследует
    ExpressionNode <|-- MemberExpression : наследует


    BinaryExpression *-- ExpressionNode : left/right operands
    UnaryExpression *-- ExpressionNode : operand
    MethodCallExpression *-- ExpressionNode : argument
    MemberExpression *-- ExpressionNode : inner expression

    Algebra ..> ExpressionNode : анализирует выражение
    Algebra ..> ParameterExpression : определяет переменную x
    Algebra ..> BinaryExpression : применяет правило цепочки
    Algebra ..> MethodCallExpression : sin/cos обработка
    Algebra ..> UnaryExpression : Convert unwrap
    Algebra ..> MemberExpression : обработка ToString/Length

    Algebra ..> MathFunctions : использует для дифференцирования

    ExpressionKind ..> BinaryExpression : тип операции
    ExpressionKind ..> UnaryExpression : тип преобразования
    ExpressionKind ..> MethodCallExpression : вызов функции
```
