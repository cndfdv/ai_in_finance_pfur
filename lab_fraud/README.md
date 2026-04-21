# Лабораторная работа №1 — Детекция мошенничества

## Описание

Индивидуальная работа. Решения сабмитятся в Kaggle InClass контест, финальная оценка — по Public LB ROC-AUC.

| Параметр | Значение |
|----------|----------|
| Раздел программы | 1.5 — Детекция мошенничества, борьба с аномалиями, AML-системы |
| Датасет | Financial Transactions Dataset: Analytics |
| Kaggle контест | [detect-fraud-in-card-transactions-pfur](https://www.kaggle.com/competitions/detect-fraud-in-card-transactions-pfur) |
| Задача | Бинарная классификация транзакций: мошенничество / норма |
| Метрика (Kaggle LB) | **ROC-AUC** |
| Формат | Индивидуально |

## Данные

Файлы скачиваются со страницы контеста на Kaggle:

| Файл | Описание |
|---|---|
| `train.csv` | 7,131,970 транзакций + метки `is_fraud` |
| `test.csv` | 1,782,993 транзакций (без меток — нужно предсказать) |
| `cards_data.csv` | Карты: тип, бренд, лимит, наличие чипа, утечка на даркнет |
| `users_data.csv` | Профили пользователей: возраст, доход, долги, кредитный скоринг |
| `mcc_codes.json` | Справочник Merchant Category Codes |
| `sample_submission.csv` | Шаблон сабмишена (все нули) |

## Что нужно предсказать

Вероятность того, что транзакция мошенническая (`is_fraud ∈ [0, 1]`), для каждого `id` из `test.csv`. Сабмишен:

```
id,is_fraud
7475333,0.0012
7475338,0.0004
7475342,0.9821
...
```

## Метрика

**Kaggle LB: ROC-AUC** (`sklearn.metrics.roc_auc_score`). Диапазон 0.5 (random) — 1.0 (идеальный ранкинг).

```python
from sklearn.metrics import roc_auc_score
roc = roc_auc_score(y_true, y_prob)
```

## Оценивание

Оценка ставится по Kaggle Public LB ROC-AUC:

| Оценка | ROC-AUC |
|---|---|
| 5 | ≥ 0.985 |
| 4 | 0.950 – 0.985 |
| 3 | 0.900 – 0.950 |
| 2 | 0.800 – 0.900 |
| 1 | 0.600 – 0.800 |
| 0 | < 0.600 / нет сабмишена |

## Загрузка данных

```bash
kaggle competitions download -c detect-fraud-in-card-transactions-pfur -p data/fraud/contest
unzip -qo data/fraud/contest/detect-fraud-in-card-transactions-pfur.zip -d data/fraud/contest
rm -f data/fraud/contest/detect-fraud-in-card-transactions-pfur.zip
```

