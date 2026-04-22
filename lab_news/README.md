# Лабораторная работа №2 — Прогноз волатильности DJIA по новостям

## Описание

Индивидуальная работа. Прогноз дневной волатильности DJIA по новостным заголовкам и ценовой истории. Под волатильностью понимается модуль дневной доходности: `abs_return = |daily_return|`. Решения сабмитятся в Kaggle InClass контест, финальная оценка — по Public LB RMSE.

| Параметр | Значение |
|----------|----------|
| Раздел программы | 1.6 — NLP в финансах |
| Датасет | Daily News for Stock Market Prediction (DJIA 2008-08 → 2016-07) |
| Kaggle контест | [predict-djia-daily-returns-from-news-headlines-pfur](https://www.kaggle.com/competitions/predict-djia-daily-returns-from-news-headlines-pfur) |
| Задача | Регрессия: прогноз `abs_return ≥ 0` |
| Метрика | **RMSE** (чем меньше, тем лучше) |
| Формат | Индивидуально |

## Данные

Файлы скачиваются со страницы контеста на Kaggle:

| Файл | Описание |
|---|---|
| `train.csv` | 1,610 дней: `Date + abs_return` (2008-08-11 → 2014-12-31) |
| `test.csv` | 378 дней: `Date` (2015-01-02 → 2016-07-01) — нужно предсказать |
| `Combined_News_DJIA.csv` | 25 заголовков новостей на каждый день, за весь период |
| `RedditNews.csv` | Сырые заголовки Reddit r/worldnews с датами |
| `djia_train.csv` | Цены DJIA (OHLCV) только до 2014-12-31 включительно |
| `sample_submission.csv` | Шаблон сабмишена (Date, abs_return=0) |

## Целевая переменная

```
daily_return_t = (close_t − close_{t-1}) / close_{t-1}
abs_return_t   = |daily_return_t|
```

## Что нужно предсказать

Неотрицательное значение `abs_return` для каждой даты из `test.csv`. Формат сабмишена:

```
Date,abs_return
2015-01-02,0.0045
2015-01-05,0.0112
...
```

Ровно 378 строк + header. Даты в формате `YYYY-MM-DD`.

## Метрика

**RMSE** (`sklearn.metrics.mean_squared_error(..., squared=False)`).

```python
from sklearn.metrics import mean_squared_error
import numpy as np
rmse = np.sqrt(mean_squared_error(y_true, y_pred))
```

## Оценивание

Оценка ставится по Kaggle Public LB RMSE (чем меньше, тем лучше):

| Оценка | RMSE |
|---|---|
| 5 | ≤ 0.0064 |
| 4 | 0.0064 – 0.0065 |
| 3 | 0.0065 – 0.0067 |
| 2 | 0.0067 – 0.00720 |
| 1 | 0.00720 – 0.00800 |
| 0 | > 0.00800 / нет сабмишена |

## Загрузка данных

```bash
kaggle competitions download -c predict-djia-daily-returns-from-news-headlines-pfur -p data/news/contest
unzip -qo data/news/contest/predict-djia-daily-returns-from-news-headlines-pfur.zip -d data/news/contest
rm -f data/news/contest/predict-djia-daily-returns-from-news-headlines-pfur.zip
```
