# Stereolabs ZED

Камера глубины StereoLabs ZED [ https://www.stereolabs.com/zed/specs/](https://www.stereolabs.com/zed/specs/)

![](/assets/zed_product_main.jpg)

Это **пассивная стереокамера**, то есть, она состоит из двух RGB-камер, разнесённых друг от друга \(расстояние между камерами 12 см\). В отличие от этого, широкораспространённые 3D-камеры Kinect1,2, Xtion и PrimeSense являются активными, так как содержат ИК-лазер, с помощью которого измеряют расстояние. Активные камеры отлично работают в закрытых пространствах, но не работают на "открытом воздухе", то есть в ситуации, когда в поле зрения камеры попадают объекты, освещённые солнечным светом. Поэтому, в робототехнике и интерактивных публичных проектах имеется острая необходимость в пассивных 3D-камерах, которые бы работали днём, в присутствие солнечного света.

Это дешёвая стереокамера \(449$\). Аналог камеры ZED - семейство камер Bumblebee2 фирмы PointGrey

[https://www.ptgrey.com/bumblebee2-firewire-stereo-vision-camera-systems](https://www.ptgrey.com/bumblebee2-firewire-stereo-vision-camera-systems). Эти камеры хороши, но стоят порядка нескольких тысяч долларов США. \(Для заказа камеры в Россию требуется воспользоваться услугами одной из фирм, специализирующихся на покупке товаров за рубежом\).

Это небольшая и легкая камера. Её размеры и вес сопоставимы с размерами камеры Xtion. Очень удобно, что в камере есть отверстие 1/4'' для крепления к стандартным штативам. Камеру можно использовать на дроне

[https://developer.nvidia.com/embedded/learn/success-stories/stereolabs](https://developer.nvidia.com/embedded/learn/success-stories/stereolabs)

Камера поддерживает работу с ОС Windows, Linux, Jetson TK1, Jetson TX1.

## 1. Установка в Windows

Для работы камеры требуется Windows, USB 3.0 (c USB 2.0 также будет работать, но медленнее), современная видеокарта NVidia, а также установленная библиотека Cuda 7.5.

    1. Вместе с камерой прилагается USB-флэшка, на которой записано руководство и драйвера. Откройте флешку, и запустите инсталлятор ZED_SDK_WinSetup_v0.9.4e_CUDA75_beta.exe (или более новую версию) из папки Windows. (Если инсталлятор не запускается, вначале запустите vcredist_x64.exe - для установки нужных библиотек Visual Studio.)
    2. Также, можно скачать эти файлы на странице https://www.stereolabs.com/developers/#download_anchor
    Если у вас не установлена CUDA 7.5, во время установки инсталлятор предложит установить CUDA. Вы можете согласиться, но в моём случае инсталлятор CUDA сказал, что не может установить её на мою видеокарту. Поэтому, я рекомендую установить CUDA 7.5 прямо с сайта NVidia https://developer.nvidia.com/cuda-downloads.
    3. После установки, компьютер перезагрузится.
    4. Теперь, подключите ZED-камеру к компьютеру. (В моём случае, Windows написал, что устанавливаются драйвера нового устройства, и это сообщение висело долгое время, с прогресс-баром на отметке 20%. Оказалось, что драйвер был успешно поставлен.
    5. Проверим, что камера подключилась. Для этого, запустите программу ZED Explorer. Она покажет изображение с обеих RGB камер, имеющихся в  ZED.



## 2. Калибровка
Теперь стоит проверить качество работы камеры по вычислению карты глубины. Но, перед этим, я советую сделать калибровку камеры, запустив программу ZED Calibration, и нажать кнопку Start. На экране появится изображение сетки, в центре которой будет нарисован полупрозрачный, слабо видимый красный круг.



Вы должны поместить вашу камеру напротив экрана. Старайтесь располагать камеру так, чтобы ее ось была перпендикулярна плоскости экрана. В этом случае область экрана, на которую смотрит камера, будет показываться прозрачным синим кругом или эллипсом (если ось камеры направлена неперпендикулярно плоскости экрана).
Размер круга будет зависеть от расстояния от камеры до экрана.
Ваша задача - сопоставить синий круг с красным кругом. После этого, красный круг будет менять свой размер и положение. Вы должны выполнить все процедуры сопоставления.
В итоге, на экран выдастся сообщение, что данные для калибровки собраны, и некоторое время будут происходить вычисления.

В моем случае, на этапе калибровки, когда красный круг появился сверху, я никак не мог сопоставить с ним синий круг (программа это не засчитывала). Решением стало увеличение разрешения экрана так, чтобы вся сетка поместилась в экран.

##  3. Исследование качества карты глубины
Теперь, посмотрим, насколько хорошо камера вычисляет карту глубины. На демо-роликах показано, что она хорошо справляется с крупными объектами (деревья, люди) на открытом воздухе. Здесь мы проверим работу в помещении.

Запустите программу **ZED Depth Viewer**.

 В левом верхнем углу будет показан кадр с левой и правой камер в виде анаглифа (можно менять вид показа), внизу - карта глубины, справа - облако 3D-точек, которое можно вращать мышкой.

На карте глубин светлые пиксели соответствуют близким объектам, темные - дальним объектам. Черным цветом отмечены области, где камера отказалась вычислить глубину.

Для того, чтобы лучше рассмотреть качество карты глубины, нажмите кнопку Settings (она расположена в правом верхнем углу приложения), и уменьшите диапазон показа - параметр Depth Clamp, скажем, до 1663 mm (по умолчанию, стоит значение 15000 mm).


 Тогда, карта глубины будет более контрастной.
 

Видно, что камера правильно увидела кресло и силуэт человека, а также окружающие предметы. В то же время, карта глубины для контура человека (от носа до рук) вычислена неверно: в силуэт добавлена часть стены. Это связано с тем, что текстура стены - однородная, и пассивные камеры плохо работают с вычислением глубины однородных текстур.

### 3.1 Анализ близких объектов

Камера не видит (то есть, карта глубин неверна или черная), или видит с большими искажениями объекты, расположенные на расстоянии, ближе 80 см к камере.
На скриншоте показана обработка руки, близко поднесенной к камере. Видно, что карта глубин для руки не посчиталась.


### 3.2 Анализ маленьких объектов

Камера не различает пальцы рук, а также, карта глубин размазывается на визуально близкие объекты. На скриншоте ниже показана получившаяся карта глубин для кисти рук с растопыренными пальцами, а также "вырост" небольшой области на карте глубины из головы с сторону кисти.



### 3.3 Работа в затемненных областях и влияние солнечных бликов

Был проведен эксперимент по съемке затемненного пространства, напротив окна. Как оказалось, программа Depth Viewer осуществляет выравнивание средней яркости изображения, поэтому, у затемненных кадров автоматически повышается яркость. При этом, карта глубины восстанавливается верно:

Обратите внимание на солнечный блик в верхней части снимка. Видно, что в этом месте карта глубины не была вычислена (черное пятно).

## 4. Исследование скорости работы
ZED-камера передает в компьютер два видеопотока, а обработка и получение карты глубины делается в нем, с помощью GPU. Поэтому, скорость работы зависит от используемой видеокарты.
Я работал на ноутбуке, с видеокартой GeForce GT 650M, и скорость работы составила 11 кадров в секунду в разрешении HD720.
Вверху программы можно переключать разрешение и скорость кадров, и для разрешения HD720 можно поставить 60 кадров в секунду - то есть, для современных видеокарт, можно получить хорошую скорость работы.
Также, можно понизить разрешение - есть режим VGA / 100 FPS, но он у меня не включился (программа завершила работу с ошибкой).


## 5. Создание трехмерных моделей пространства и объектов с помощью ZedFu
Программа **ZedFu**, входящая в комплект поставки, позволяет осуществлять построение трехмерной модели пространства, сшивая несколько снимков.
Эта технология называется Data Fusion. Существует несколько подобных программ, работающих с камерами типа Kinect, и также с обычными снимками, полученными с фотоаппарата или мобильного телефона. Я экспериментировал со следующими программами:

    * **Kinect Fusion**  https://msdn.microsoft.com/en-us/library/dn188670.aspx - она не очень качественно работает, но зато с открытым кодом, и поддерживает Kinect1, 2
    * **Scanect** http://skanect.occipital.com/ - программа платная, но можно эксперементировать с триальной версией, она позволяет бесплатно публиковать модели онлайн, https://sketchfab.com/models/c04c2e056a0341389647f5ff743eff83
    * **Audodesk Remake** - эта программа работает не с камерой глубины, а с набором фото, и дает трехмерные модели наилучшего качества. Она бесплатная. Обработка данных идет на удаленном сервере, поэтому, построение моделей занимает достаточно много времени.

Проведем эксперимент с **ZedFu**. Запустите ее и нажмите кнопку Live в верхней части экрана, чтобы программа начала захватывать данные.
(Бывает, что после нажатия кнопки Live программа зависает. Для устранения этой проблемы можно переподключить камеру к компьютеру, и перезапустить программу)

После этого, окно программы программа будет показывать RGB-изображение (слева вверху), карту глубины (слева внизу) и 3D-модель данного кадра справа:


Теперь нажмите кнопку Start, расположенную в верхней части экрана, и медленно двигайте камеру. Вы увидите, что осуществляется сшивка фрагментов получаемых 3D поверхностей в одну поверхность. Затем, нажмите кнопку Stop. Программа начнет строить качественную модель (в процессе сшивки показывалась грубая модель).

Внимание: следует дождаться, пока программа завершит построение модели, до этого показывается модель низкого качества!

Результат сшивки запишется в виде OBJ-файла, и в нижней части экрана будет написан путь, куда сохранен этот файл (в папку Документы/ZED/Meshes). А в окне программы появится изображение построенной модели, которое можно вращать и перемещать мышью:

Открыв папку, кода сохранен файл, мы видим, что в нем не только OBJ-файл, но и файлы материалов и текстуры, а также файл в формате PLY:

Эти файлы являются стандартными, их понимает большинство 3D-редакторов и 3D-принтеров. Например, можно посмотреть OBJ-файл в программе Deep Exploration (она платная, но позволяет в триальном режиме просматривать файлы):

Zed-камера "видит" объекты на расстоянии до 20 метров, а потому, позволяет делать сшивку уличных сцен (съемка Игоря Содазота):

 

В данный момент получаемое качество модели не очень высокое, а именно, форма объектов типа мебели и людей не точная. В то же время, общая структура пространства восстанавливается достаточно хорошо.

Заключение
Камера вполне пригодна для использования в мобильной робототехнике и интерактивных инсталляциях на открытом воздухе и солнечном свете. Качество карта глубины сейчас имеет качество ниже, чем у активных 3D-камер типа Kinect, но это специфика пассивного стереозрения. Несомненно, что со временем алгоритмы стереосопоставления улучшатся, и камера будет работать качественнее.

Камера выдает облако точек, а потому потенциально возможно подключение ее к OpenNI/NITE для вычисления координат частей тела человека.

   ![](/assets/zed-1.png)

## 2. Calibration

Now it’s a good idea to explore thequality of the depth map computed by ZED camera. But before that, we suggest todo the calibration by running **ZED Calibration** program, and press the _Start_ button. You will a program’s window with a grid and translucent, slightly visible redcircle:

![](/assets/zed-2.png)

You have to put your camera in front of the screen.Try to position the camera so that it is perpendicular to the axis of the screen plane.In this case, the screen area on this pointing the camera, will appear transparent blue circle or ellipse \(if the camera axis is non-perpendicular to the screen plane\).  
The size of the circle will depend on the distance between the camera and the screen.Your task is to match the blue circle with a red circle.After that, a red circle will change its size and position.You must repeat this procedure several times.  
As a result, the screen displays a message calibraion data is collected, and some computations will started for a while.

## 3. Exploring depth map quality

Now, let’s see how well the camera calculates the depth map. On official demos you can see the camera detects well large objects \(trees, people\) outdoors. Here we examine the work in the room.

Start **ZED Depth Viewer** program.

![](https://kuflex.files.wordpress.com/2016/08/zed-3.png?w=960 "ZED-3")

In the upper left corner of the window it shows left and right cameras as anaglyph \(you can change this appearance style\). At the bottom it shows depth map, at the right – a cloud of 3D-points, which can be rotated with the mouse.

In the depths map bright pixels correspond to the close objects, dark pixels to the distant objects.Black color indicates the areas where the camera refused to calculate the depth.

In order to better explore the quality of the depth map, click the _Settings_ button \(located in the upper right corner of the window\), and decrease decrease display range by adjusting _Depth Clamp_ slider, say, down to 1663 mm \(the default value is 15,000 mm\).

![](https://kuflex.files.wordpress.com/2016/08/zed-4.png?w=904&h=804 "ZED-4")

Then, the depthmap will have more contrast.

![](https://kuflex.files.wordpress.com/2016/08/zed-5.png?w=960 "ZED-5")

We can see that the camera is correctly saw the silhouette of a man and a chair, as well as the surrounding objects. At the same time, the depth map for the human \(from the nose to the hand\) was calculated incorrectly: the part of the wall is erroneously added to the silhouette. This is due to the fact that the walltexture ishomogeneous and passive stereo cameras perform poorly with depthcalculation for areas with homogeneous texture.

### 3.1 Analysis of near objects

The camera does not see \(that is, the depth map is incorrect or black\), or gives the large distortions for the objects located at a distance equal or less than 80 cm to the camera. The screenshotshows ahand, badged close to the camera. It can be seen that the depth map are not computed for the hand.

![](https://kuflex.files.wordpress.com/2016/08/zed-6.png?w=960 "ZED-6")

### 3.2 Analysis of small objects

The camera does not distinguish small objects such as the fingers, as well as the depth map is smeared on visually near objects. The screenshot shows the depth map for the splayed fingers, as well as protrusion of a small area in a depth map from the head to the hand.

![](/assets/zed-3.png)

### 3.3 Processing the dark areas and the impact of a solar flare

In this experiment we capture a darkened space, opposite the window. As it turned out, **Depth Viewer** automatically adjust the average brightness of the images, so dark images became brighter. And, the depth map is restored correctly:

![](/assets/zed-12.png)

Note the solar flare at the top of the anaglyph image. It is evident that at this area the depth map was notcalculated \(black spot\).

## 4. Working speed

ZED-camera transmits two video streams \(left and right RGB cameras\) into the computer. The depth map computation is performed using GPU.Therefore, the performance heavily depends on the graphics card.  
We used a laptop with **GeForce GT 650M** video card, and the speed was 11 frames per second at resolution _HD720_.  
At the top of the program you can switch the resolution and frame rate, and for resolution for the _HD720_ where is option _60_ frames per second – that is, for modern graphics cards, you can get a quite good depth calculation speed.  
Also, you can reduce the resolution – there are _VGA_ / _100 FPS_ mode, but currently we can’t turned it on \(the program crashed\).

## 5. Creating a three-dimensional  space models using ZedFu

**ZedFu**programs included in the package allows for the construction of three-dimensional model of the space, by stitching multiple depth maps and images captured from diferent viewpoints.  
This technology is calledData Fusion.There are severalprograms for Data Fusion, working with depth cameras such as the Kinect, and also with conventional \(RGB\) cameras and mobile phones. We had experience with the following programs:

* **Kinect Fusion**  
  [https://msdn.microsoft.com/en-us/library/dn188670.aspx](https://msdn.microsoft.com/en-us/library/dn188670.aspx)  
   – it’s open source program, supporting Kinect1, 2, but works not very good. It’s great as starting point for Data Fusion algorithms study.

* **Scanect**  
  [http://skanect.occipital.com](http://skanect.occipital.com)  
   – it’s paid program, but you can experiment with the trial version, which allows you to publish your scanned models online, see our example  
  [https://sketchfab.com/models/c04c2e056a0341389647f5ff743eff83](https://sketchfab.com/models/c04c2e056a0341389647f5ff743eff83)

* **123D Catch**  
  [http://www.123dapp.com/catch](http://www.123dapp.com/catch)  
   – this free program does work with with a set of photos from an \(RGB\) camera or mobile, and create three-dimensional model of the fine quality. Data p  
  rocessing is performed on a remote server, so the construction of models can take a lot of time.

Let’s make an experiment with **ZedFu.** Run it and click the _Live_ button at the top of the screen to start the capturing video.

\(Sometimes the program hangs after pressing the _Live_ button. To resolve this problem, reconnect the camera and restart the program.\)

You will see the program window with the RGB-image \(top left\), a depth map \(bottom left\) and 3D-model of the current frame at the right:

![](/assets/zed-8.png)

Now click the _Start_ button at the top of the screen and move the camera slowly.You will how the 3D model at the right will grows.After a while, press the _Stop_ button.The program will start to build a finer 3D model.

Note: you must wait until the program will complete the construction of a model to get the 3D model of the good quality!

The resulted model is saved as the OBJ-file, and in the bottom of the screen you will see the path to the file \(**Documents/ZED/Meshes**\). Also the model will be shown, and you canrotate and move it using the mouse:

![](/assets/zed-9.png)

Open the folder with the OBJ file. You will find there not only OBJ file itself, but also the files of materials and textures, as well asa filein the PLY format:

![](/assets/zed-10.png)

OBJ and PLY are standard 3D file formats, and you can use them in most 3D-software. For example, you canopen theOBJ-file in the **Deep Exploration** program \(its paid program, but allows to view files in trial mode\):

![](/assets/zed-11.png)

Zed camera “sees” objects at a distance of 20 meters, and so allows capturing outdoor scenes:

![](/assets/zed-112.jpg)

![](/assets/zed-110.jpg)

It can be seen that the quality of the resulting model is not very high, namely, the form of furniture and people is restored inaccurately. At the same time, the overall structure and general location of the objects is restored well.

### Conclusion

The camera is quite suitable for using in mobile robotics and interactive installations in the open air and sunlight. The quality of the depth map is now lower than that of active depth cameras such as Kinect, but it is the specificity of the passive stereovision. There is no doubt that over time the stereocorrespondence algorithms will improve, and the passive cameras will work better.

The camera produces a cloud of points, and therefore potentially it is possible to connect it to the OpenNI / NITE to calculate the coordinates of human body parts for using in interactive installations.

