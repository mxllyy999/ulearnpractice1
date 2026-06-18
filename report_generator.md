# Практика: Генератор отчетов

## 1. Описание предметной области и сущностей

Система подготовки статистических сводок по погодным измерениям.
Производит вычисление статистических показателей (среднее значение и стандартное отклонение, медиана) для двух метрик (температура и влажность) и формирует отчёты в HTML и Markdown форматах.

Measurement - содержит данные измерений температуры и влажности.

IStatisticsCalculator - интерфейс вычисления статистики, определяет метод Calculate, возвращающий результат расчёта.

MeanStdCalculator - вычисляет среднее значение и стандартное отклонение, реализует IStatisticsCalculator.

MedianCalculator - вычисляет медиану набора данных, реализует IStatisticsCalculator.

IReportRenderer - интерфейс формирования отчёта, определяет методы для создания заголовка, списка и элементов отчёта.

HtmlRenderer - формирует отчёт в формате HTML, реализует IReportRenderer.

MarkdownRenderer - формирует отчёт в формате Markdown, реализует IReportRenderer.

ReportContext - содержит выбранные стратегии: рендерер отчёта и вычислитель статистики, обеспечивает их совместное использование.

ReportGenerator - формирует итоговый отчёт, используя ReportContext, обрабатывает список измерений и вызывает рендерер и вычислитель.

ReportFactory - предоставляет методы создания готовых конфигураций ReportGenerator для HTML и Markdown отчётов с разными типами статистики.
## 2. Диаграмма классов (Mermaid)

```mermaid
classDiagram
    direction LR

    class ReportContext {
        -renderer : IReportRenderer
        -calculator : IStatisticsCalculator
        +ReportContext(IReportRenderer, IStatisticsCalculator)
        +Renderer()
        +Calculator()
    }

    class ReportGenerator {
        -context : ReportContext
        +ReportGenerator(ReportContext)
        +Generate(IEnumerable~Measurement~, string) string
    }

    class IReportRenderer {
        <<interface>>
        +MakeCaption(string) string
        +BeginList() string
        +MakeItem(string, string) string
        +EndList() string
    }

    class IStatisticsCalculator {
        <<interface>>
        +Calculate(IEnumerable~double~) object
    }

    class HtmlRenderer
    class MarkdownRenderer

    class MeanAndStdCalculator
    class MedianCalculator

    class Measurement {
        +Temperature : double
        +Humidity : double
    }

    class ReportFactory {
        <<static>>
        +CreateHtmlMeanStd() ReportGenerator
        +CreateHtmlMedian() ReportGenerator
        +CreateMarkdownMeanStd() ReportGenerator
        +CreateMarkdownMedian() ReportGenerator
    }

    IReportRenderer <|.. HtmlRenderer : реализует интерфейс рендеринга HTML
    IReportRenderer <|.. MarkdownRenderer : реализует интерфейс рендеринга Markdown

    IStatisticsCalculator <|.. MeanAndStdCalculator : считает среднее и стандартное отклонение
    IStatisticsCalculator <|.. MedianCalculator : считает медиану

    ReportContext o-- IReportRenderer : содержит рендерер отчёта
    ReportContext o-- IStatisticsCalculator : содержит калькулятор статистики

    ReportGenerator --> ReportContext : использует конфигурацию отчёта
    ReportGenerator ..> Measurement : обрабатывает данные измерений

    ReportFactory ..> ReportGenerator : создаёт генератор отчётов
    ReportFactory ..> HtmlRenderer : создаёт HTML рендерер
    ReportFactory ..> MarkdownRenderer : создаёт Markdown рендерер
    ReportFactory ..> MeanAndStdCalculator : создаёт калькулятор среднего и отклонения
    ReportFactory ..> MedianCalculator : создаёт калькулятор медианы
```
