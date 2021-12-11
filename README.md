# Соревнование Expresso Churn Prediction. Предсказание оттока клиентов, бинарная классификация.
## Ссылка: https://zindi.africa/competitions/expresso-churn-prediction
Описание исходных признаков лежит в файле VariableDefinitions.csv. 
Предварительная разведка данных train/test проводилась через pandas-profiling, в репозитории не представлена.
Prediction файл должен содержать вероятность оттока (churn) каждого из клиентов. Метрика AUC-ROC.

+ Папка **Preparation examples** содержит несколько файлов .ipynb для разных трансформаций исходного датасета.
+ Папка **Prediction examples** содержит несколько файлов (они все приблизительно в одном духе) .ipynb для загрузки модифицированных датасетов и применение алгоритмов на них.
+ Использованные библиотеки: *xgboost, lightgbm, catboost* для модифицированных датасетов и некоторые алгоритмы из *scikit-learn* для подвыборки исходного датасета, содержащего почти во всех признаках NaN значения.
+ Схема тюнинга (*Optuna*): 5SKF, внутри каждого train блока еще одно деление на train_inner и valid через обычный train_test_split (stratified=True). Обучение бустингов проводилось на train, overfit-детектор контролировал valid, ошибка по обобщению высчитывалась по train_outer.
### Структура проекта выглядела следющим образом:
#### Папка datasets
Трансформация данных.
```
+---datasets
|   +---dataset_0
|   |   |   dataset_0.ipynb
|   |   |   файлы csv
|   |   |   dataset_0_without_minimum_data_clean.ipynb (для разделения на информативные строки и "NAN строки" - идея не выстрелила)
|   +---dataset_1
|   +---dataset_2
|   +---...
|   +---dataset_N
|   |
|   +---initial
```
#### Папка preds
Загрузка модифицированных данных, настройка и тюнинг алгоритмов МО. Сохранение результатов.
```
|---preds
|   +---dataset_0
|   |   +---CB
|   |   |   |   CB_dataset_0_test.csv (валидация на public LB)
|   |   |   |   CB_dataset_0_train_OOF.csv (данные для обучения метамодели)
|   |   |   |   dataset_0_preds.ipynb
|   |   +---LGB
|   |   +---XGB
|   +---dataset_1
|   +---...
|   +---dataset_N
|   +---initial
|   +---level_2 (Модель для стакинга)
```
