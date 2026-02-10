flowchart TD
    User[Пользователь<br>в корпоративном мессенджере]

    subgraph "Мультиагентная AI-система (единый интерфейс)"
        Bot[Чат-бот<br><i>маршрутизация, контекст</i>]
        Analyst[Агент-Аналитик<br><i>LLM: анализ, рекомендации</i>]
        Editor[Агент-Редактор<br><i>LLM + RAG: внесение правок</i>]
    end

    Confluence[Корпоративный портал<br>Confluence / Wiki]
    LLM_Analyst[Внешний LLM-провайдер<br><i>для анализа</i>]
    LLM_Editor[Внешний LLM-провайдер<br><i>для редактирования</i>]

    %% Основной поток
    User -- "запрос + ссылка на ТЗ" --> Bot
    Bot -- "анализ ТЗ" --> Analyst
    Analyst -- "чтение документа" --> Confluence
    Analyst -- "генерация рекомендаций" --> LLM_Analyst
    Analyst -- "рекомендации" --> Bot
    Bot -- "результат анализа" --> User

    %% Поток редактирования
    User -- "команда на правки" --> Bot
    Bot -- "правка по контексту" --> Editor
    Editor -- "чтение/поиск контекста" --> Confluence
    Editor -- "генерация изменений" --> LLM_Editor
    Editor -- "запись изменений" --> Confluence
    Editor -- "подтверждение" --> Bot
    Bot -- "результат редактирования" --> User

    %% Стилизация
    style User fill:#e1f5fe,stroke:#01579b
    style Bot fill:#f3e5f5,stroke:#4a148c
    style Analyst fill:#e8f5e8,stroke:#1b5e20
    style Editor fill:#fff3e0,stroke:#e65100
    style Confluence fill:#fce4ec,stroke:#880e4f
    style LLM_Analyst fill:#f1f8e9,stroke:#33691e
    style LLM_Editor fill:#f1f8e9,stroke:#33691e
