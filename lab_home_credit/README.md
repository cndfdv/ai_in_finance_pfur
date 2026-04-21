# Контест — Home Credit Default Risk

## Описание

Индивидуальная работа. Студент самостоятельно исследует данные, конструирует признаки и разрабатывает модель для максимизации метрики. Решения сабмитятся в оригинальный Kaggle-контест, финальная оценка — по Kaggle Public LB.

| Параметр | Значение |
|----------|----------|
| Раздел программы | 1.4 — AI для анализа и управления кредитными рисками |
| Датасет | Home Credit Default Risk |
| Kaggle контест | [home-credit-default-risk](https://www.kaggle.com/competitions/home-credit-default-risk) |
| Задача | Бинарная классификация: предсказать вероятность дефолта по кредиту |
| Метрика | **ROC-AUC** (Kaggle Public LB) |
| Формат | Индивидуально |

## Данные

7 связанных таблиц:

| Таблица | Описание |
|---------|----------|
| `application_train.csv` | Анкета заёмщика (основная таблица с TARGET) |
| `application_test.csv` | Анкета заёмщика (без TARGET, нужно предсказать) |
| `bureau.csv` | Кредитная история в других банках |
| `bureau_balance.csv` | Помесячные балансы кредитов из бюро |
| `previous_application.csv` | Прошлые заявки в этом банке |
| `POS_CASH_balance.csv` | История POS-кредитов |
| `credit_card_balance.csv` | Баланс кредитных карт |
| `installments_payments.csv` | История платежей по рассрочкам |

## Целевая переменная

```python
import pandas as pd

df = pd.read_csv('data/home_credit/application_train.csv')

# TARGET уже в данных:
# 1 — клиент допустил дефолт (не вернул кредит)
# 0 — клиент вернул кредит
y = df['TARGET']
X = df.drop(columns=['TARGET', 'SK_ID_CURR'])
```

## Что нужно предсказать

Вероятность дефолта `TARGET ∈ [0, 1]` для каждого `SK_ID_CURR` из `application_test.csv`. Формат сабмишена:

```
SK_ID_CURR,TARGET
100001,0.0312
100005,0.1789
...
```

## Метрика

**Kaggle LB: ROC-AUC** (`sklearn.metrics.roc_auc_score`). Диапазон 0.5 (random) — 1.0 (идеальный ранкинг).

```python
from sklearn.metrics import roc_auc_score
auc = roc_auc_score(y_true, y_prob)
```

## Оценивание

Оценка ставится по Kaggle Public LB ROC-AUC:

| Оценка | ROC-AUC |
|--------|---------|
| 5 | ≥ 0.790 |
| 4 | 0.770 – 0.790 |
| 3 | 0.750 – 0.770 |
| 2 | 0.700 – 0.750 |
| 1 | 0.600 – 0.700 |
| 0 | < 0.600 / нет сабмишена |

## Загрузка данных

```bash
kaggle competitions download -c home-credit-default-risk -p data/home_credit
unzip -qo data/home_credit/home-credit-default-risk.zip -d data/home_credit
rm -f data/home_credit/home-credit-default-risk.zip
```
