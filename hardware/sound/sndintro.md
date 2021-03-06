# Звук

* Цифровойзвук
* Применениязвука
* Типымикрофонов
* ЗаписьзвукавAudacity
* СведениедорожеквAbletonLive
* Пультыиконтроллеры
* ОбработказвукавMax/MSP
* Анализзвука

### Что такое звук

Звук, в широком смысле — упругие волны, продольно распространяющиеся в среде и создающие в ней механические колебания;

в узком смысле — субъективное восприятие этих колебаний специальными органами чувств животных или человека.

Как и любая волна, звук характеризуется амплитудой и частотой.         \(Википедия\)

![](/assets/snd01.png)

### [http://blog.modernmechanix.com/mags/qf/c/PopularScience/9-1950/med\_sound.jpg](http://blog.modernmechanix.com/mags/qf/c/PopularScience/9-1950/med_sound.jpg)

### Представление звука

### в цифровом виде

Реальный звук захватывается микрофоном, затем подвергается аналого-цифровому преобразованию.

Разрешение по времени - частота дискретизации,   \[процедура - дискретизация\]

Разрешение по амплитуде - разрядность.                   \[процедура - квантование\]

![](/assets/snd02.png)

#### Амплитуда

[http://upload.wikimedia.org/wikipedia/commons/thumb/9/9a/Digital.signal.svg/567px-Digital.signal.svg.png](http://upload.wikimedia.org/wikipedia/commons/thumb/9/9a/Digital.signal.svg/567px-Digital.signal.svg.png)

### 

### Применения звука

![](/assets/snd04.png)

Создание звукового фона: воспроизведение готовых звуков и аудиотреков![](/assets/snd05.png)

Обработка звука

Манипуляции со звуком, захватываемым прямо во время выступления \(музыка, вокал, стихи\)

![](/assets/snd06.png)

Анализ звука

Вычисление параметров звука \(громкость, содержимое по частотам, темп, тон\) и использования для визуализации

![](/assets/snd07.png)

Синтез звука

Превращение параметров, изображений, движений, запахов в звуки и музыку

\(сонификация, транскодинг\)

### Создание звукового фона

#### Подготовка звуков                                                 Сведение дорожек

* Готовые звуковые файлы: freesound.org                       Ableton Live
* Синтез: Cubase, Reason
* Запись в микрофон: Audacity

### Микрофоны

Динамический микрофон![](/assets/snd08.png)

Концертный

[http://novotroitsk-rap.my1.ru/img/a4e6156c10.gif](http://novotroitsk-rap.my1.ru/img/a4e6156c10.gif)

![](/assets/snd09.png)

Конденсаторный микрофон

Студийный

![](/assets/snd10.png)

[http://harmonica.ru/content/study/articles/amplification\_mics/condenser.gif](http://harmonica.ru/content/study/articles/amplification_mics/condenser.gif)

![](/assets/snd11.png)

### Запись звука в Audacity

![](/assets/snd12.png)

Запись, подрезка, нормализация, сохранение в файл \(AIFF, WAV\)

### Сведение дорожек в Ableton Live

### ![](/assets/snd13.png)![](/assets/snd14.png)

### Пульты и контроллеры

![](/assets/snd15.png)



### Пульты и контроллеры

![](/assets/snd17.png)

![](/assets/snd16.png)

### ![](/assets/snd18.png)![](/assets/snd19.png)

### Обработка звука в Ableton Live

![](/assets/snd20.png)

### Обработка звука в Ableton Live

![](/assets/snd21.png)

### Обработка звука в Max/MSP

![](/assets/snd23.png)

![](/assets/snd24.png)

Перформанс “Одной линией”, 

Екатерина Жаринова и Ольга Севостьянова, 2013г.

[http://ekazha.wordpress.com/projects/line/](/h  ttp://ekazha.wordpress.com/projects/line/)



### Обработка звука в Max/MSP

![](/assets/snd25.png)

### Анализ звука

Вычисление мгновенной громкости

Анализ спектра

Преобразования звука в изображение

![](/assets/snd26.png)

![](/assets/snd27.png)

![](/assets/snd28.png)

[https://masteringof.wordpress.com/examples/sounds/](https://masteringof.wordpress.com/examples/sounds/)



### Анализ звука

Звук обрабатывается блоками

\(типичное значение - по 256 отсчётов\)

44100 / 256 = 172,3 раза в секунду

0.0058 сек один блок

Чаще всего звуковые алгоритмы обрабатывают полученный блок без задержки блоков, и суммарная задержка между записью и воспроизведением равна сумме двух блоков \(запись + воспроизведение\).

В нашем примере это 0.012 сек.

В то же время, сложные алгоритмы анализа звука могут аккумулировать буферы для обработки; в этом случае, итоговая задержка увеличивается.



### Синтез звука

Источники входных данных:

* Пульты
* Датчики
* Движения человека
* Изображения
* Видео

Звук так же подаётся в звуковую карту блоками \(типичное значение - 256 отсчётов\)

Пример - маятник; пусть его положение задаёт параметры звука









