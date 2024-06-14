# Рекомендательная Финансовая Система

## Предсказание покупок по банковским картам на данных транзакционного журнала
Проект направлен на предсказание покупок по банковским картам на основе данных транзакционного журнала. Система анализирует предыдущие покупки и рекомендует продукты, которые могут потребоваться клиенту в будущем.

## Структура Проекта
1. **Подготовка данных**: Создание системы для работы с панельными данными, обогащение их внешними источниками.
2. **Разработка модели**: Создание и тренировка модели или ансамбля моделей машинного обучения.
3. **Развертывание и мониторинг**: Организация инфраструктуры для развертывания модели и мониторинга её работы.

### Подготовка данных
Данные транзакционного журнала содержат информацию о транзакциях, налогах, сборах и чеках. Предварительная обработка включает:
- Очистка данных
- Удаление дубликатов
- Обработка пропущенных значений
- Нормализация и стандартизация
- Преобразование текста в векторные представления с помощью моделей Word2Vec или LLM ([BERT-base-uncased](https://huggingface.co/google/bert_uncased_L-12_H-768_A-12), [Meta-LLaMA-3-8B](https://huggingface.co/meta/llama-3b8))

### Разработка модели
Используемые подходы:
- **Коллаборативная фильтрация**: Использование SVD и ALS для анализа поведения пользователей.
- **Модели-трансформеры**: Применение моделей для обработки текстовых данных.

### Развертывание и мониторинг
- **Контейнеризация**: Для упаковки и развертывания моделей мы будем использовать Docker.
- **API для интеграции**: следует создать RESTful API для взаимодействия с другими системами и возможностью использования наших моделей другими отделами.
- **Мониторинг и обновление**: Настройка системы мониторинга для отслеживания производительности модели и обновление моделей с получением новых данных. Для этой задачи можно использовать фреймворк MLflow или Kubeflow.

## Рекомендательная система по формированию финансового плана
Проект направлен на создание LLM-помощника для рекомендаций по формированию финансового плана для достижения целей на основе анализа стратегий инвестиционных портфелей других клиентов. Система должна учитывать индивидуальные цели клиентов и предлагать оптимальные стратегии на основе данных о портфелях с похожими целями и профилем риска.

## Структура Проекта
1. Анализ показателей инвестиционных портфелей других клиентов с помощью LLM-бота.
2. Использование LLM-бота помощника для выбора оптимальной стратегии для клиента.
3. Трекинг перфоманс-метрик портфеля клиента. 

### 1. Анализ показателей инвестиционных портфелей других клиентов. Визуальное отображение результатов.
С помощью использования promt-запросов клиент сможет общаться с LLM-ботом и выгружать всю информацию, касающуюся анализа показателей инвестиционных портфелей: своих или других клиентов. LLM-бот, в свою очередь, будет обрабатывать информацию по базе данных инвестиционных портвелей, формировать файл json и отправлять на сервер, где расположен бот. На сервере файл json с нужными данными будет обрабатываться и представлять в подходящем виде клиенту. 

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

### 3. Трекинг перфоманс-метрик портфеля клиента. 
Чат-бот модель также будет помогать клиенту "трекать" перфоманс метрики портфеля, для этого ему важно будет решать следующие задачи:

#### 1. Автоматизация отчетности
LLM-модель должна автоматически генерировать отчеты о состоянии портфеля клиента. На основе данных о текущих инвестициях, исторических данных и текущих рыночных условиях чат-бот должен  предоставлять регулярные обновления перфоманс-метрик, таких как доходность, волатильность, альфа и бета. Это позволит клиентам получать актуальную информацию о своих инвестициях без необходимости вручную анализировать данные.

#### 2. Индивидуальные уведомления и оповещения
Чат-бот может попросить клиента согласиться настроить индивидуальные уведомления для каждого клиента. Например, если доходность портфеля отклоняется от целевых значений или если уровень риска превышает допустимые пределы, клиент будет мгновенно уведомлен об этом. Это позволяет оперативно реагировать на изменения в рыночной ситуации и принимать своевременные инвестиционные решения.
