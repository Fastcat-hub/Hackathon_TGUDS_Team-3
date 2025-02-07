
# Прогнозирование уровня загрязнения воздуха 
## Оглавление

- [Описание](#title1)
- [Данные](#title2)
- [Разведывательный анализ данных (EDA)](#title3)
    - [Обработка пропущенных значений](#title4)
    - [Анализ распределения данных](#title5)
    - [Корреляционный анализ](#title6)
    - [Графический анализ временных рядов](#title7)
- [Подготовка данных](#title8)
- [Моделирование](#title9)
- [Оценка качества модели](#title10)
- [Результаты](#title11)
- [Использованные библиотеки](#title12)
- [Авторы](#title13)
- [Лицензия](#title14)
- [Инструкция по использованию](#title15)

## <a id="title1">Описание</a>
**Загрязнение воздуха** — один из ключевых факторов, негативно влияющих на здоровье населения и экологическую ситуацию в городах. По данным Всемирной организации здравоохранения (ВОЗ), загрязнённый воздух ежегодно приводит к миллионам преждевременных смертей, увеличивает нагрузку на систему здравоохранения и снижает качество жизни. Однако традиционные методы мониторинга требуют значительных ресурсов, а их данные часто запаздывают. 

**Цель данного проекта** — создать модель для прогнозирования уровня загрязнения воздуха на основе исторических данных об измерениях ключевых загрязняющих веществ. Мы используем данные о качестве воздуха в разных городах Индии, включая концентрации различных загрязняющих веществ и индекс качества воздуха (AQI).

## <a id="title2">Данные</a>
Данные содержат информацию о качестве воздуха в разных городах Индии за период 2015–2020 годов. Основные признаки включают:
- **PM2.5, PM10:** Концентрация твердых частиц.
- **NO2, SO2, CO, O3:** Концентрация газообразных загрязнителей.
- **AQI:** Индекс качества воздуха, рассчитанный на основе вышеупомянутых параметров.
- **City:** Название города.
- **Date:** Дата измерения. \
**Источник данных:** \
  [Ссылка на модель](https://scikit-learn.org/0.16/modules/generated/sklearn.ensemble.RandomForestRegressor.html). \
  [Ссылка на dataset](https://www.kaggle.com/datasets/hirenvora/city-daycsv).
  
  

## <a id="title3"> Разведывательный анализ данных (EDA )</a>
#### <a id="title4"> Обработка пропущенных значений</a>
- Пропуски в числовых столбцах были заполнены медианными значениями по городам.
- Пропущенные значения AQI были восстановлены с помощью модели **`RandomForestRegressor`**.
#### <a id="title5"> Анализ распределения данных</a>
- Построена гистограмма с кривой плотности (KDE) для оценки распределения AQI.
- Выявлены выбросы, которые не были удалены, так как они соответствуют реальным пикам загрязнения.
#### <a id="title6"> Корреляционный анализ</a>
- Построена тепловая карта корреляции между признаками.
- Выявлена сильная корреляция между NO и NOx, что объясняется их химическим составом.
#### <a id="title7"> Графический анализ временных рядов</a>
- Построены графики изменения AQI во времени для каждого города.
- Выявлены сезонные колебания: повышение уровня загрязнения наблюдается в зимние периоды.

## <a id="title8"> Подготовка данных</a>
1.	**Удаление нечисловых данных:**
- Убраны категориальные признаки (например, название города).
2.	**Масштабирование признаков:**
- Все числовые признаки были стандартизированы с помощью **`StandardScaler.`**
3.	**Разделение данных:**
- Данные разбиты на обучающую (80%) и тестовую (20%) выборки.

## <a id="title9"> Моделирование</a>
Для решения задачи использовалась модель **`RandomForestRegressor`**, которая хорошо работает с табличными данными и не требует масштабирования признаков. Однако масштабирование всё равно улучшило качество модели.
**Параметры модели:**
- **`n_estimators=100`**: Количество деревьев в ансамбле. Значение 100 является стандартным и обеспечивает хорошее качество без чрезмерных вычислительных затрат.
- **`random_state=42`**: Используется для инициализации генератора случайных чисел, что обеспечивает воспроизводимость результатов. \
**Примечание:** Был опробован подбор параметров методом **`GridSearchCV`** из библиотеки **`scikit-learn`**, но он не дал результатов лучше указанных выше.

## <a id="title10"> Оценка качества модели</a>
Вычислены следующие метрики:
- **MSE (Mean Squared Error):** Средняя квадратичная ошибка.
- **MAE (Mean Absolute Error):** Средняя абсолютная ошибка.
- **MAPE (Mean Absolute Percentage Error):** Средняя абсолютная процентная ошибка.

Построены дополнительные графики:

**1.	Анализ остатков:**
- Центрирование остатков вокруг нуля говорит о том, что модель в среднем не смещена.
- Распределение остатков приближено к нормальному, что указывает на случайность ошибок.
- Длинные хвосты на графике говорят о наличии больших ошибок предсказания. 

**2.	Сравнение предсказаний:**
- Диаграмма рассеяния показывает, что большинство точек расположены вдоль красной линии (𝑦 = 𝑥), что говорит о высокой точности модели.
- При высоких значениях AQI модель иногда занижает предсказания.

## <a id="title11">Результаты</a>
Основные выводы:
- Модель показала хорошие результаты в прогнозировании AQI, что может быть полезно для принятия своевременных решений по борьбе с загрязнением воздуха.
- Подготовка данных сыграла ключевую роль: заполнение пропусков и масштабирование признаков существенно улучшили качество модели.
- Выявлены города с наиболее высоким уровнем загрязнения, что позволяет сосредоточить усилия на проблемных регионах. \
**Перспективы развития:**
- Оптимизация гиперпараметров модели для повышения точности.
- Расширение датасета за счет данных из других стран.
- Создание интерактивной платформы для реального времени мониторинга качества воздуха.

## <a id="title12">Использованные библиотеки</a>
1. `pandas`
2. `numpy`
3. `matplotlib`
4. `seaborn`
5. `scikit-learn`

Если вы хотите использовать этот проект, установите зависимости командой:
1. `pip install pandas numpy scikit-learn matplotlib seaborn`

## <a id="title13">Авторы</a>
Команда №3

## <a id="title14">Лицензия</a>
Этот проект распространяется под лицензией MIT — см. файл LICENSE для дополнительной информации.

## <a id="title15">Инструкция по использованию</a>
**Инструкция по установке:** \
Клонируйте репозиторий: git clone https://github.com/Fastcat-hub/Hackathon_TGUDS_Team-3 \
Перейдите в директорию проекта: Hackathon_TGUDS_Team-3 \
Открыть Hakaton_Kom_3.ipynb в Jupyter Notebook или Google Collab \
Зависимости установятся автоматически при запуске ячеек ноутбука.

### 1. Подготовка к работе
Убедитесь, что у вас установлен Python и все необходимые библиотеки. \
Если они отсутствуют, установите их командой:
`pip install pandas numpy scikit-learn matplotlib seaborn`

Файл данных (**`city_day.csv`**) должен находиться в той же папке, что и ноутбук. При необходимости замените файл на свой, изменив путь в коде загрузки данных.

### 2. Запуск модели
После подготовки данных выполните основные этапы работы с моделью: \
***1.	Подготовка данных:**
- Очистка данных (удаление пропусков, приведение типов).
- Выбор значимых признаков.
- Разделение на обучающую и тестовую выборки. \
**2.	Обучение модели:**
- В ноутбуке используется модель **`RandomForestRegressor`**.
- Можно изменить параметры модели (**`n_estimators, max_depth`** и др.). \
**3.	Оценка качества модели:**
- После обучения автоматически рассчитываются метрики: MSE, MAE, MAPE.
- Строятся графики: фактические vs предсказанные значения AQI, гистограмма остатков.

### 3. Настройка параметров
При необходимости можно:
- Изменить модель (например, **`XGBoost, GradientBoosting`**).
- Оптимизировать гиперпараметры с использованием **`GridSearchCV`**.
- Добавить новые признаки в данные.

### 4. Запуск и использование
Последовательно выполните все ячейки ноутбука. Ознакомьтесь с полученными метриками и графиками. При необходимости измените модель или параметры и повторите анализ.

