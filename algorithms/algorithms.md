Алгоритм - связан с технологией \(видео/звук\), а конкретная реализация - и со средством \(Processing/openFrameworks\). Писать алгоритмы на "абстрактном языке" - затея тупиковая. Поэтому, такой вариант: Средства - независимые Технологии - независимые Алгоритмы - уже зависят от Средств и Технологий.

#### Техника программирования:

\*Асинхронные таймеры для реализации нескольких щелкающих реле в ардуино, поведение нескольких персонажей игры. Идея - таймер без delay. T0=millis\(\); ... If \(time &gt; T0+1000\) { Щелчок - т.е. действие T0=millis\(\); }

\*Конечные автоматы \(см лекции ЕАСИ, 2 курс 1е занятие весна\) для разных состояний игры - начало, игра, конец. Также см асинхронный таймер.

#### Текстурирование и 3д треугольник, два, конус из кружков, треуг, текстурированный {#3}

#### Звуковой паттерн

#### Физика частиц

#### Маятники на резинках

#### Оптический поток

#### Перспективное преобразование для мэппинга и калибровки камер.

#### Программирование интерактивности

\(Описание для Микрокосмоса\) У инсталляции есть "внутреннее состояние", которое описывается несколькими числами: "спит" 10%, "возбуждена" 30%, "расслабленна" 60%. Мы описываем, как люди влияют на ее состояние - то есть меняют распределение этих чисел.

Внешние характеристики, которые система показывает вовне - яркость и тон цвета, и характер звука, определяются ее состоянием. Например, если она "расслабленна", то цвет мягко флуктуирует, а звук гудит слегка. Если она"возбуждена", то цвет ярко-красный, а звук более пронзительный, к примеру.

Люди - их близость к инсталляции и скорость движения - определяют изменения состояния системы. Например, если подойти близко, она возбуждается, если долго рядом никого нет - она засыпает. так выходит: внешние условия \(люди в комнате\) -&gt; СОСТОЯНИЕ -&gt; внешняя реакция \(изменение света и звука\) Это, что называется, "долгая интерактивность" - бывает, не сразу ясно, что инсталляция реагирует на конкретно тебя. Поэтому обычно следует добавлять "быструю интерактивность" - то есть мгновенную реакцию в виде легкого изменения света и звука при движениях людей. Короче, получается два типа интерактивности - долгая и мгновенная. Настя, "внутренние состояния" системы, как оно меняется от людей, и как это выливается из нее во внешний мир - как раз ожидаю, что ты придумаешь\) я лишь технологическую канву этого продумал, не наполнение\)\)\) Надо что-то простое придумать, чтоб мне успеть запрограммировать ☺ Тут две странички моего текста чуть подробнее про внешние условия - состояния - результат: http://bit.ly/2mCkb6s \(правда только в контексте видео, но философия везде та же

---







