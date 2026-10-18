# Диаграммы

## Диаграмма последовательности

Диаграмма показывает взаимодействие посетителя, системы и ML-модели при выполнении Use Case «Анализ тональности отзыва» (UC-01).

```mermaid
sequenceDiagram
    participant User as Посетитель
    participant System as Система
    participant Model as ML-модель тональности
    participant DB as История анализов

    User->>System: Ввести текст отзыва и нажать «Анализировать»
    System->>System: Проверить длину и корректность текста
    alt Текст короче 10 символов
        System-->>User: Сообщение «Введите более развёрнутый отзыв»
    else Текст корректен
        System->>Model: Передать текст отзыва
        Model-->>System: Вернуть тональность и уверенность
        System->>DB: Сохранить результат в историю
        System-->>User: Показать тональность и уровень уверенности
    end
```

## Диаграмма классов

Диаграмма описывает основные сущности сервиса: пользователь, отзыв, результат анализа и запись истории.

```mermaid
classDiagram
    class User {
        +id: int
        +name: string
        +role: string
    }

    class Review {
        +id: int
        +text: string
        +createdAt: datetime
    }

    class SentimentResult {
        +sentiment: string
        +confidence: float
        +needsManualCheck: bool
    }

    class AnalysisHistory {
        +id: int
        +analyzedAt: datetime
        +textFragment: string
    }

    User "1" --> "*" Review : отправляет
    Review "1" --> "1" SentimentResult : анализируется как
    SentimentResult "1" --> "0..1" AnalysisHistory : сохраняется в
```
