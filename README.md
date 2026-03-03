# Product Analytics for a Digital Bank

Продуктовая аналитика цифрового банка: транзакции, воронки, удержание клиентов и ML-предсказание оттока.

**Датасет**: [Synthetic Bank Transactions](https://www.kaggle.com/datasets/radistaleks/synthetic-bank-transactions) (Kaggle)

---

## Данные

| Файл | Строк | Описание |
|------|-------|----------|
| `clients.csv` | ~1 000 | Клиенты: возраст, пол, доход, кредит/депозит |
| `transactions.csv` | ~929 000 | Транзакции за 2020 год |
| `subscriptions.csv` | 1 308 | Подписки с 2012 по 2020 |
| `categories.csv` | 29 | Справочник категорий MCC |

---

## Структура проекта

```
├── notebooks/
│   ├── EDA.ipynb                    # Разведочный анализ данных
│   ├── 02_Core_Metrics.ipynb        # MAU, ARPU, выручка, категории, мерчанты
│   ├── 03_Funnels.ipynb             # Продуктовая воронка, выживаемость подписок
│   ├── 04_Retention_Cohorts.ipynb   # Когорты, retention, LTV подписок
│   ├── 05_Segmentation_RFM.ipynb    # RFM-сегментация, K-Means кластеры
│   ├── 06_ML_Churn_Prediction.ipynb # Logistic Regression, Random Forest, XGBoost, SHAP
│   └── 07_Export_for_Dashboard.ipynb# Экспорт метрик в CSV
├── data/
│   ├── datasets/                    # Исходные CSV (не в git)
│   └── changed/                     # Экспортированные метрики для дашборда
├── requirements.txt
├── .gitignore
└── README.md
```

---

## Ноутбуки

### EDA.ipynb
Первичное знакомство с данными: распределения, временные паттерны, топ-категории и мерчанты.

### 02 — Core Metrics
- MAU и выручка по месяцам
- ARPU, средний чек, медиана
- Топ-15 категорий и мерчантов
- Тепловые карты по часам и дням недели

### 03 — Funnels
- Продуктовая воронка: транзакции → кредит → депозит → подписка
- Проникновение категорий (% клиентов)
- Survival curve подписок
- Churn rate по музыкальным сервисам

### 04 — Retention & Cohorts
- Когорты по году регистрации: ARPU, транзакций на клиента
- Retention matrix подписок (год старта × длительность)
- LTV подписок по сервисам
- Retention curve по транзакциям

### 05 — Segmentation & RFM
- RFM-скоринг (квинтили): Champions, Loyal, At Risk, Lost и др.
- Анализ по полу и возрастным группам
- Портфель продуктов
- K-Means кластеризация по категориям трат

### 06 — ML Churn Prediction
- Целевая переменная: отмена подписки в 2020
- Признаки: клиентские + транзакционные + подписочные
- Модели: Logistic Regression → Random Forest → XGBoost
- SHAP: важность признаков, waterfall для самого рискованного клиента
- Скоринг всех активных подписок

### 07 — Export for Dashboard
Пересчитывает все ключевые метрики и сохраняет CSV в `data/changed/`:

| Файл | Содержимое |
|------|------------|
| `monthly_kpis.csv` | MAU, выручка, ARPU по месяцам |
| `category_stats.csv` | Статистика по категориям |
| `merchant_stats.csv` | Топ-мерчанты |
| `product_funnel.csv` | Шаги воронки |
| `subscription_survival.csv` | Кривая выживаемости |
| `churn_by_month.csv` | Отмены подписок по месяцам |
| `music_ltv.csv` | LTV по музыкальным сервисам |
| `cohort_by_reg_year.csv` | Когорты по году регистрации |
| `rfm_clients.csv` | RFM-скоры всех клиентов |
| `rfm_segments.csv` | Средние метрики по сегментам |
| `at_risk_clients.csv` | Клиенты с высоким риском оттока |
| `hourly_patterns.csv` | Паттерны по часам |
| `daily_patterns.csv` | Паттерны по дням недели |

---

## Запуск

```bash
pip install -r requirements.txt
jupyter notebook
```

Ноутбуки загружают датасет автоматически через `kagglehub`. Для этого нужен Kaggle API-ключ (`~/.kaggle/kaggle.json`).

---

## Ключевые инсайты

- **MAU = 1 000** каждый месяц — все клиенты активны
- **Топ-категория** по объёму: Транспорт (37.7% транзакций)
- **Churn rate** подписок: ~21.7%
- **Медиана до отмены**: ~400 дней
- **Champions** (лучший RFM-сегмент): высокая частота + большие суммы + недавние покупки
- **XGBoost ROC-AUC** на предсказании оттока: >0.80
