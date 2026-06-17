# Практика: Генератор отчетов

## 1. Описание предметной области и сущностей


Система подготовки статистических сводок по погодным измерениям. Производит вычисление показателей (среднее значение, стандартное отклонение, медиана) для двух метрик (температура и влажность) и формирует вывод в HTML и Markdown.

IStatisticsEngine - задает метод Compute для расчета показателей

MeanStdEngine - обсчитывает среднее и отклонение, реализует IStatisticsEngine

MedianEngine - находит медиану ряда, реализует IStatisticsEngine

IOutputRenderer - определяет методы RenderHeader, RenderListStart, RenderEntry, RenderListEnd для построения вывода

HtmlOutputRenderer - формирует HTML-разметку, реализует IOutputRenderer

MarkdownOutputRenderer - формирует Markdown-разметку, реализует IOutputRenderer

ReportGenerator - связывает рендерер и вычислитель, метод Generate создает отчет

ReportHelper - предоставляет четыре готовых метода: MeanStdHtml, MedianHtml, MeanStdMarkdown, MedianMarkdown

Measurement - содержит Temperature и Humidity

MeanAndStd - содержит Mean и Std
## 2. Диаграмма классов (Mermaid)

```mermaid
classDiagram
    direction LR

    class Measurement {
        +double Temperature
        +double Humidity
    }

    class MeanAndStd {
        +double Mean
        +double Std
    }

    class IStatisticsEngine {
        <<interface>>
        +Compute(IEnumerable~double~) object
    }

    class IOutputRenderer {
        <<interface>>
        +RenderHeader(string) string
        +RenderListStart() string
        +RenderEntry(string, string) string
        +RenderListEnd() string
    }

    class HtmlOutputRenderer {
        +RenderHeader(string) string
        +RenderListStart() string
        +RenderEntry(string, string) string
        +RenderListEnd() string
    }

    class MarkdownOutputRenderer {
        +RenderHeader(string) string
        +RenderListStart() string
        +RenderEntry(string, string) string
        +RenderListEnd() string
    }

    class MeanStdEngine {
        +Compute(IEnumerable~double~) object
    }

    class MedianEngine {
        +Compute(IEnumerable~double~) object
    }

    class ReportGenerator {
        -IOutputRenderer _renderer
        -IStatisticsEngine _engine
        +ReportGenerator(IOutputRenderer, IStatisticsEngine)
        +Generate(IEnumerable~Measurement~, string) string
    }

    class ReportHelper {
        <<static>>
        +MeanStdHtml(IEnumerable~Measurement~) string
        +MedianHtml(IEnumerable~Measurement~) string
        +MeanStdMarkdown(IEnumerable~Measurement~) string
        +MedianMarkdown(IEnumerable~Measurement~) string
    }

    IOutputRenderer <|.. HtmlOutputRenderer
    IOutputRenderer <|.. MarkdownOutputRenderer
    IStatisticsEngine <|.. MeanStdEngine
    IStatisticsEngine <|.. MedianEngine
    
    ReportGenerator --> IOutputRenderer : использует
    ReportGenerator --> IStatisticsEngine : использует
    ReportGenerator ..> Measurement : обрабатывает
    
    MeanStdEngine ..> MeanAndStd : возвращает
    
    ReportHelper ..> ReportGenerator : создает
    ReportHelper ..> HtmlOutputRenderer : создает
    ReportHelper ..> MarkdownOutputRenderer : создает
    ReportHelper ..> MeanStdEngine : создает
    ReportHelper ..> MedianEngine : создает
```
