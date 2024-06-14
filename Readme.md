# Рекомендательная система по формированию финансового плана
Проект направлен на создание LLM-помощника для рекомендаций по формированию финансового плана для достижения целей на основе анализа стратегий инвестиционных портфелей других клиентов. Система должна учитывать индивидуальные цели клиентов и предлагать оптимальные стратегии на основе данных о портфелях с похожими целями и профилем риска.

## Структура Проекта
1. Анализ показателей инвестиционных портфелей других клиентов с помощью LLM-бота.
2. Использование LLM-бота помощника для выбора оптимальной стратегии для клиента.
3. Трекинг перформанс-метрик портфеля клиента. 

### 1. Анализ показателей инвестиционных портфелей других клиентов. Визуальное отображение результатов.
С помощью использования запросов клиент сможет общаться с LLM-ботом и выгружать всю информацию, касающуюся анализа показателей инвестиционных портфелей: своих или других клиентов. LLM-бот, в свою очередь, будет обрабатывать информацию по базе данных инвестиционных портфелей, формировать файл JSON и отправлять на сервер, где расположен бот. На сервере файл json с нужными данными будет обрабатываться и представлять в подходящем виде клиенту. 

В качестве модели предлагаю использовать модель [saiga2](IlyaGusev/saiga2_70b_lora) построенную на базе Llama и обученную на корпусе текстов на русском языке. Существует несколько видов данной модели с разным количеством параметров. В зависимости от того, насколько быстро чат-бот сможет получать запрос от клиента и выдавать информацию мы подберем нужную модель.

Если есть определенные ограничения, с которыми банк может столкнуться, используя модели Meta, то можно выбрать другую модель. Однако мы можем взять саму модель Llama и обучить ее на наших данных, с помощью Fine-Tuning техник и получить уже собственную модель для своих задач и созданную на уникальном корпусе текстов по инвестиционной деятельности нашего банка.

### 2. Использование LLM-бота помощника для выбора оптимальной стратегии для клиента

С помощью использования prompt-запросов клиент сможет общаться с LLM-ботом и выгружать всю информацию, касающуюся выбора оптимальной стратегии для достижения финансовых целей. LLM-бот будет обрабатывать запросы, анализировать данные из базы данных финансовых стратегий и формировать JSON-файл с результатами анализа. Этот файл будет отправляться на сервер, где расположен бот. На сервере JSON-файл будет обрабатываться и представляться в удобном виде клиенту.

В качестве модели предлагаю использовать модель [saiga2](https://huggingface.co/IlyaGusev/saiga2_70b_lora), построенную на базе Llama и обученную на корпусе текстов на русском языке. Существует несколько видов данной модели с разным количеством параметров. В зависимости от того, насколько быстро чат-бот сможет получать запросы от клиента и выдавать информацию, мы подберем нужную модель.

Если есть определенные ограничения, с которыми банк может столкнуться, используя модели Meta, то можно выбрать другую модель. Однако мы можем взять саму модель Llama и обучить ее на наших данных, с помощью Fine-Tuning техник и получить уже собственную модель для своих задач, созданную на уникальном корпусе текстов по финансовой деятельности нашего банка.

#### Пример использования LLM-бота для выбора стратегии

##### Шаг 1: Взаимодействие с клиентом
Клиент отправляет запрос через чат-бота, например:
```arduino
"Какую стратегию вы посоветуете для достижения цели накопления на пенсию в течение 20 лет?"
```
##### Шаг 2: Обработка запроса
LLM-бот анализирует запрос, используя обученную модель, и подбирает подходящие стратегии из базы данных.

##### Шаг 3: Формирование и отправка ответа
Результаты анализа формируются в JSON-файл и отправляются на сервер:
```arduino
{
    "strategy": "Консервативное инвестирование",
    "description": "Рекомендуемая стратегия включает вложения в низкорисковые активы с регулярными пополнениями.",
    "expected_return": "5-7% годовых",
    "risk_level": "низкий",
    "recommended_assets": ["гособлигации", "высококачественные корпоративные облигации"]
}
```
##### Шаг 4: Представление результата клиенту
На сервере JSON-файл обрабатывается и представляется клиенту в удобном для восприятия виде, например, в виде дашборда с графиками и ключевыми показателями.

### 3. Трекинг перформанс-метрик портфеля клиента. 
Чат-бот также будет помогать клиенту отслеживать перформанс-метрики портфеля, для этого ему важно будет решать следующие задачи:

#### 1. Автоматизация отчетности
LLM-модель должна автоматически генерировать отчеты о состоянии портфеля клиента. На основе данных о текущих инвестициях, исторических данных и текущих рыночных условиях чат-бот должен  предоставлять регулярные обновления перформанс-метрик, таких как доходность, волатильность, альфа и бета. Это позволит клиентам получать актуальную информацию о своих инвестициях без необходимости вручную анализировать данные.

Пример отчета в формате JSON:
```arduino
{
    "report_date": "2024-06-14",
    "client_name": "Иван Иванов",
    "portfolio_value": "1,200,000 руб.",
    "monthly_return": "6%",
    "year_to_date_return": "8%",
    "asset_allocation": {
        "гособлигации": "50%",
        "высококачественные корпоративные облигации": "30%",
        "акции": "20%"
    },
    "risk_assessment": {
        "volatility": "Низкая",
        "beta": "0.8"
    },
    "recommendations": "Рекомендуем рассмотреть ребалансировку портфеля, увеличив долю акций до 25% для повышения потенциальной доходности."
}
```

#### 2. Индивидуальные уведомления и оповещения
Чат-бот может попросить клиента согласиться настроить индивидуальные уведомления для каждого клиента. Например, если доходность портфеля отклоняется от целевых значений или если уровень риска превышает допустимые пределы, клиент будет мгновенно уведомлен об этом. Это позволяет оперативно реагировать на изменения в рыночной ситуации и принимать своевременные инвестиционные решения.

Пример уведомления о достижения финансовых целей в формате JSON:
```arduino
{
    "notification_date": "2024-06-14",
    "client_name": "Мария Иванова",
    "event_type": "Целевая доходность достигнута",
    "message": "Поздравляем! Ваш портфель достиг целевой доходности в 7% за последний квартал. Рекомендуем рассмотреть возможность ребалансировки для сохранения достигнутых результатов.",
    "portfolio_value": "1,500,000 руб.",
    "current_return": "7%"
}
```

Пример уведомления о рыночном событии в формате JSON:
```arduino
{
    "notification_date": "2024-06-14",
    "client_name": "Мария Иванова",
    "event_type": "Значительное изменение стоимости актива",
    "message": "Акции компании XYZ упали на 6% за последний день. Рекомендуем проверить ваш портфель и оценить возможные действия.",
    "affected_asset": "Акции компании XYZ",
    "price_change": "-6%"
}
```

## Прогнозирование поведения инвест. портфеля клиента на основе различных событий на рынке 

Проект направлен на создание модели LLM, способной прогнозировать поведение инвестиционного портфеля клиента в ответ на различные рыночные события. Основная цель проекта – предоставить инвесторам инструмент, который поможет им принимать более обоснованные решения и минимизировать риски в условиях изменяющихся рыночных условий.

## Структура Проекта
1. Сбор и обработка данных
2. Обучение модели
3. Генерация прогнозов и рекомендаций

### 1. Сбор и обработка данных
Модель будет интегрирована с различными источниками данных банка, такими как финансовые рынки, макроэкономические показатели, новостные ленты и корпоративные отчеты, telegram-каналы на финансовую тематику. Это позволит собирать актуальную информацию, необходимую для точного прогнозирования. Необходимо будет реализовать система потоковой передачи данных с помощью фреймворков, типа Apache Kafka для передачи данных в режиме реального времени. Далее мы векторизируем данные  и, в зависимости от модели, применим токенайзер. 

### 2. Обучение модели
Для обучения модели мы будем использовать технологию RAG (Retrieval-Augmented Generation) для дообучения уже настроенных LLM моделей. Модель может быть использована также [saiga2](https://huggingface.co/IlyaGusev/saiga2_70b_lora) 

### 3. Генерация прогнозов и рекомендаций

После дообучения модели RAG можно использовать её для генерации прогнозов о поведении инвестиционного портфеля в ответ на различные рыночные события. Например, модель может предсказать, как повышение процентных ставок повлияет на стоимость активов в портфеле, или как отчет о прибыли компании отразится на её акциях.

Пример запроса для генерации прогноза:
```arduino
question = "Как повышение процентных ставок повлияет на мой инвестиционный портфель?"
forecast = generate_forecast(question)
print("Прогноз:", forecast)
```

Генерация рекомендаций
Помимо прогнозов, дообученная модель будет предоставлять рекомендации по управлению инвестиционным портфелем. Эти рекомендации могут включать советы по ребалансировке активов, изменению инвестиционной стратегии или принятию других мер для минимизации рисков и максимизации доходности.

Пример запроса для генерации рекомендаций:
```arduino
question = "Какие рекомендации вы можете дать для моего инвестиционного портфеля при повышении процентных ставок?"
recommendation = generate_recommendation(question)
print("Рекомендации:", recommendation)
```
