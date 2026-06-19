# Практика: Контрольный разряд

## 1. Описание предметной области и сущностей
ControlDigitAlgo - основной класс, который считает контрольные цифры (UPC, ISBN-10, Luhn) и объединяет логику алгоритмов.

Extensions - общие методы для работы с числами: получение цифр и вычисление по модулю.

DigitStream - превращает число в последовательность цифр.

ModuloRules - считает контрольную цифру по модулю 10 и 11.

UpcTransform - правила вычисления для UPC.

LuhnTransform - правила вычисления для Luhn.

IsbnTransform - правила вычисления для ISBN-10.

IsbnFormatter - преобразует результат ISBN-10 в символ (цифра или X).
## 2. Диаграмма классов (Mermaid)
```mermaid
classDiagram
    direction TB


    class ControlDigitAlgo {
        <<static>>

        +Upc(long number) int
        +Isbn10(long number) char
        +Luhn(long number) int

        -ProcessUpc(IEnumerable~int~ digits) int
        -ProcessIsbn10(IEnumerable~int~ digits) int
        -ProcessLuhn(IEnumerable~int~ digits) int
    }


    class DigitStream {
        <<static>>

        +Reverse(long number) IEnumerable~int~
        +Enumerate(long number) IEnumerable~int~
    }


    class ModuloRules {
        <<static>>

        +Mod10(int value) int
        +Mod11(int value) int
    }

    class UpcTransform {
        <<static>>
        +Apply(int digit, int position) int
    }

    class LuhnTransform {
        <<static>>
        +Apply(int digit, int position) int
    }

    class IsbnTransform {
        <<static>>
        +Apply(int digit, int position) int
    }


    class IsbnFormatter {
        <<static>>
        +ToCheckSymbol(int value) char
    }


    ControlDigitAlgo ..> DigitStream : получает поток цифр
    ControlDigitAlgo ..> ModuloRules : применяет модульные правила

    ControlDigitAlgo ..> UpcTransform : UPC логика
    ControlDigitAlgo ..> LuhnTransform : Luhn логика
    ControlDigitAlgo ..> IsbnTransform : ISBN логика
    ControlDigitAlgo ..> IsbnFormatter : форматирование ISBN

    UpcTransform ..> DigitStream : использует позицию цифры
    LuhnTransform ..> DigitStream : использует позицию цифры
    IsbnTransform ..> DigitStream : использует позицию цифры
```
