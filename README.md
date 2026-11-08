## Короткий опис задачі
Класифікація ірисів (датасет Iris, 3 класи, 4 ознаки). Експеримент `iris-classification` у MLflow, 5 ранів, розбиття 70% / 30% (test = 45 квіток). Метрики: accuracy та log loss.

## Використані дані
- `screenshots/mlflow_runs.png`: таблиця ранів у MLflow з параметрами і метриками
- `screenshots/mlflow_artifacts.png`: артефакти rf-depth3 (модель збережена) і rf-small (артефактів немає)
- `screenshots/grafana_metrics.png`: графіки та таблиці `mlflow_accuracy` і `mlflow_loss` у Prometheus
- Додатково: порівняльні графіки MLflow, сторінка run rf-depth3, текстовий опис вхідних даних

## Обраний run
`rf-depth3` (random forest, 100 дерев, max_depth=3): accuracy 0.956, loss 0.141, run ID `2ddc4f9d563c43e5a7c6c8523a580db3`. Найкращий за обома метриками й єдиний із збереженою моделлю. Baseline: `logreg-baseline` (0.933 / 0.191).

## PM-рішення
Go to staging. До production переходити поки що не варто.

## Виявлені ризики
- Дуже малий test (45 квіток): одна помилка = 2,2 п.п. accuracy
- Модель rf-depth3 не зареєстрована в Model Registry, не залоговано dataset і версію коду
- Не визначено rollback, owner, критерій переходу
- Немає моніторингу drift і якості після деплою

## Що потрібно зробити перед production
- Зареєструвати модель у Model Registry, залогувати dataset і git commit
- Визначити критерій переходу, owner рішення і rollback
- Налаштувати моніторинг якості та drift після деплою