
# Прогнозирование уровня загрязнения воздуха по городам
## Оглавление

- [Описание](#title1)
- [Данные](#title1)
- [Разведывательный анализ данных (EDA)](#title1)
    - [Обработка пропущенных значений](#title2)
    - [Анализ распределения данных](#title2)
    - [Корреляционный анализ](#title2)
    - [Графический анализ временных рядов](#title2)
- [Подготовка данных](#title1)
- [Моделирование](#title1)
- [Оценка качества модели](#title1)
- [Результаты](#title1)
- [Использованные библиотеки](#title1)
- [Авторы](#title1)
- [Лицензия](#title1)
- [Инструкция по использованию](#title1)

## <a id="title1">Описание</a>
**Загрязнение воздуха** — один из ключевых факторов, негативно влияющих на здоровье населения и экологическую ситуацию в городах. По данным Всемирной организации здравоохранения (ВОЗ), загрязнённый воздух ежегодно приводит к миллионам преждевременных смертей, увеличивает нагрузку на систему здравоохранения и снижает качество жизни. Однако традиционные методы мониторинга требуют значительных ресурсов, а их данные часто запаздывают. \
**Цель данного проекта** — создать модель для прогнозирования уровня загрязнения воздуха на основе исторических данных об измерениях ключевых загрязняющих веществ. Мы используем данные о качестве воздуха в разных городах Индии, включая концентрации различных загрязняющих веществ и индекс качества воздуха (AQI).

## <a id="title1">Данные</a>
Данные содержат информацию о качестве воздуха в разных городах Индии за период 2015–2020 годов. Основные признаки включают:
- **PM2.5, PM10:** Концентрация твердых частиц.
- **NO2, SO2, CO, O3:** Концентрация газообразных загрязнителей.
- **AQI:** Индекс качества воздуха, рассчитанный на основе вышеупомянутых параметров.
- **City:** Название города.
- **Date:** Дата измерения. \
**Источник данных:** [Ссылка на источник] (если есть).

## <a id="title1"> Разведывательный анализ данных (EDA )</a>
#### <a id="title2"> Обработка пропущенных значений</a>
- Пропуски в числовых столбцах были заполнены медианными значениями по городам.
- Пропущенные значения AQI были восстановлены с помощью модели **`RandomForestRegressor`**.
#### <a id="title2"> Анализ распределения данных</a>
- Построена гистограмма с кривой плотности (KDE) для оценки распределения AQI.
- Выявлены выбросы, которые не были удалены, так как они соответствуют реальным пикам загрязнения.
#### <a id="title2"> Корреляционный анализ</a>
- Построена тепловая карта корреляции между признаками.
- Выявлена сильная корреляция между NO и NOx, что объясняется их химическим составом.
#### <a id="title2"> Графический анализ временных рядов</a>
- Построены графики изменения AQI во времени для каждого города.
- Выявлены сезонные колебания: повышение уровня загрязнения наблюдается в зимние периоды.

## <a id="title1"> Подготовка данных</a>
1.	**Удаление нечисловых данных:**
- Убраны категориальные признаки (например, название города).
2.	**Масштабирование признаков:**
- Все числовые признаки были стандартизированы с помощью **`StandardScaler.`**
3.	**Разделение данных:**
- Данные разбиты на обучающую (80%) и тестовую (20%) выборки.

## <a id="title1"> Моделирование</a>
Для решения задачи использовалась модель **`RandomForestRegressor`**, которая хорошо работает с табличными данными и не требует масштабирования признаков. Однако масштабирование всё равно улучшило качество модели.
**Параметры модели:**
- **`n_estimators=100`**: Количество деревьев в ансамбле. Значение 100 является стандартным и обеспечивает хорошее качество без чрезмерных вычислительных затрат.
- **`random_state=42`**: Используется для инициализации генератора случайных чисел, что обеспечивает воспроизводимость результатов.
**Примечание:** Был опробован подбор параметров методом **`GridSearchCV`** из библиотеки **`scikit-learn`**, но он не дал результатов лучше указанных выше.

## <a id="title1"> Оценка качества модели</a>
