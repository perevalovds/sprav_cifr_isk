# Использование вебкамер

### План лекции

##### 1.Вебкамеры

Получение изображения

Пороговая обработка

Калибровка

Игра “Трамплин”

##### 2. Камеры глубины

Создание интерактивных поверхностей

Динамический мэппинг

### Растровое изображение

* это прямоугольный массив пикселей.

1-канальное      пиксель = яркость                              0..255

3-канальное      пиксель = Red, Green, Blue               0..255

4-канальное      пиксель = Red, Green, Blue, Alpha    0..255

Она называются:

“полутоновое”, “цветное”, “цветное с прозрачностью”

![](/assets/Screen Shot 2017-07-31 at 13.16.09.png)

### Получение изображения с камеры в openFrameworks

ofVideoGrabber camera;

void ofApp::setup\(\){

```
camera.setDeviceID\( 0 \);  //Выбор камеры \(если несколько\)

camera.initGrabber\( 640, 480 \);
```

}

void ofApp::update\(\){

```
camera.update\(\);
```

}

void ofApp::draw\(\){

```
ofSetColor\( 255 \);

camera.draw\( 0, 0 \);
```

}

![](/assets/web01.png)

### Конвертация в полутоновое изображение

объявить

ofPixels pix;

добавить в update\(\):

```
if \( camera.isFrameNew\(\) \) {

       pix = camera.getPixelsRef\(\);

       pix.setImageType\( OF\_IMAGE\_GRAYSCALE \);

    //Тут можно обрабатывать пиксели

}
```

добавить в draw\(\):

```
 ofImage img;

img.setFromPixels\( pix \);

img.draw\( 0, 0 \);
```

![](/assets/web02.png)

### Доступ к пикселям

pix.getColor\( x, y \).getBrightness\(\) - возвращает яркость пикселя \(x,y\)

pix.setColor\( x, y, ofColor\( value \) \) - ставит яркость пикселю

Уменьшение яркости изображения:

добавить после “Тут можно обрабатывать ...”

```
 for \(int y=0; y&lt;480; y++\) {

        for \(int x=0; x&lt;640; x++\) {

            int value = pix.getColor\( x, y \).getBrightness\(\);

            value = value / 2;

            pix.setColor\( x, y, value \);

        }

    }
```

![](/assets/web03.png)

### Пороговая обработка

объявить

```
int T = 100;            //порог
```

изменить value = …  на

```
value = \( value &gt; T \) ? 255 : 0;
```

добавить в draw\(\) вывод на экран строки со значением T:

```
ofSetColor\( 255, 0, 0 \);

ofDrawBitmapString\( "T = " + ofToString\( T \), 20, 20 \);
```

добавить в keyPressed\(\) изменение порога по нажатию z  и x:

```
if \( key == 'z' \) T -= 10;

if \( key == 'x' \) T += 10;
```

![](/assets/web04.png)

### Определение роста человека

В условных единицах. Для перевода в метры - нужно просто калибровать эти значения.

int Y = 0;  //координата самой верхней точки

в update\(\), после пороговой обработки

```
    //вычисление высоты - ищем самую верхнюю белую точку

    Y = -1;

    for \(int y=0; y&lt;480 && Y == -1; y++\) {

        for \(int x=0; x&lt;640; x++\) {

                int value = pix.getColor\( x, y \).getBrightness\(\);

                if \( value == 255 \) {

                    Y = y;

                     break;

                }

        }

    }
```

в draw\(\)

```
ofSetColor\( 255, 0, 0 \);

ofSetLineWidth\( 3 \);

ofLine\( 0, Y, 640, Y \);

ofSetLineWidth\( 1 \);

   ofDrawBitmapString\( "H = " + ofToString\( 480-Y \), 20, 40 \);
```

![](/assets/web05.png)

### Оцифровка графика функции

Используя этот же алгоритм, но для

X=0, 10, 20, ...

можно оцифровать график функции. Будем искать черные точки \(графика\) на белом фоне \(бумаге\)

int stepX = 10;

int Y\[200\];

вычисление трамплина, после пороговой обр.

for \(int x=0; x&lt;640/stepX; x++\) {

        int x1 = x \* stepX;

        for \(int y=0; y&lt;480; y++\) {

            Y\[ x \] = 480;

            int value = pix.getColor\( x1, y \).getBrightness\(\);

            if \( value == 0 \) {

                Y\[ x \] = y;

                break;

            }

        }

}

в draw\(\) рисуем трамплин:

ofPushMatrix\(\);

ofTranslate\( 10, 260 \);

ofSetColor\( 255 \);

ofNoFill\(\);

ofSetLineWidth\( 2 \);

ofRect\( 0, 0, 639, 479 \);

ofSetLineWidth\( 1 \);

ofSetColor\( 255, 0, 0 \);

for \(int x=0; x&lt;640 / stepX - 1; x++\) {

    ofLine\( x \* stepX, Y\[x\], 

                 \(x+1\) \* stepX, Y\[x+1\] \);

    ofLine\( x \* stepX, 480, 

                 x \* stepX, Y\[x\] \);

}  

ofPopMatrix\(\);

### Оцифровка графика функции

Используя этот же алгоритм, но для

X=0, 10, 20, ...

можно оцифровать график функции. Будем искать черные точки \(графика\) на белом фоне \(бумаге\)

### ![](/assets/web06.png)

### Калибровка камеры и проектора

1\)Если пороговую обработку делать не для яркости, а для зелёной компоненты цвета \(или, красной или синей\),

то можно выводить картинку проектора красными и синими цветами, и камера не будет их видеть.    

     int value = pix.getColor\( x, y \).getBrightness\(\);

меняем на 

     int value = pix.getColor\( x, y \).g;

зелёная компонента цвета \(а r, b - синие\) 

### Калибровка камеры и проектора

1\)Если пороговую обработку делать не для яркости, а для зелёной компоненты цвета \(или, красной или синей\),

то можно выводить картинку проектора красными и синими цветами, и камера не будет их видеть.    

     int value = pix.getColor\( x, y \).getBrightness\(\);

меняем на 

     int value = pix.getColor\( x, y \).g;

зелёная компонента цвета \(а r, b - синие\) 

2\) После этого, можно на изображении с камеры задать 4 точки - углы соответствующей картинки проектора;

и осуществить трансформацию изображения камеры.

\(Используя перспективную трансформацию, или, для простоты, билинейную интерполяцией\)

После этого, камера и проектор будут видеть и освещать часть стены в одинаковых координатах

### Калибровка камеры и проектора

Билинейная трансформация:

    	for \(int y=0; y&lt;480; y++\) {

        	for \(int x=0; x&lt;640; x++\) {    

                  float xu = x / 640.0;

 	           float yu = y / 480.0;

            	    ofPoint P =   \(1-xu\) \* \(1-yu\) \* p\[0\]   +   \(xu\) \* \(1-yu\) \* p\[1\]

            	                      +       \(xu\) \* \(yu\) \* p\[2\]   +   \(1-xu\) \* \(yu\) \* p\[3\];

            	    int X = P.x;

            	    int Y = P.y;

            	    int value = 255;

            	    if \( X &gt;= 0 && X &lt; 640 && Y &gt;= 0 && Y &lt; 480 \) {

                	value = pixCam.getColor\( X, Y \).g; 

                	value = \(value &gt; T \) ? 255 : 0;  

            	}

            	pix.setColor\( x, y, value \);

            }

        }

![](/assets/web07.png)

### Игра трамплин

Добавим к проекту мячик, прыгающий по графику.

    Получим игру “Трамплин”

### ![](/assets/web08.png)Реализация мяча

В update\(\), после вычисления точек графика

         //мячик

    	ofPoint force;

    	force.y += 9.8 \* m \* scale;   	 

    	//вычисление реакции опоры \(трамплина\)

    	int x = int\(ball.x / stepX\);

    	if \( x &gt;= 0 && x &lt; 640 / stepX - 1 \) {

        	    float y1 = Y\[x\];

        	    float y2 = Y\[x+1\];

        	    //высота трамплина в этой точке

        	    float y = ofMap\( ball.x / stepX, x, x+1, y1, y2 \);      	 

        	    //направление

        	    ofPoint norm\( y1-y2, stepX \);

        	    norm.normalize\(\);   //делаем единичной длины

        	    if \( ball.y + rad &gt; y \) {

            	ball.y = y - rad;

            	float proj = norm.x \* vel.x + norm.y \* vel.y;

            	vel = vel - norm\*2\*proj;

        	    }

    	}

    	ofPoint a = force / m; //ускорение

    	vel += a \* dt;

         ball += vel \* dt;v

ofPoint ball;    //координаты центра мяча

ofPoint vel;     //вектор скорости мяча

float dt = 1.0 / 30.0;

float m = 0.5;

float rad = 20;

float scale = 50;

новая функция:

void start\(\) {

	ball.x = 40;

	ball.y = 0;

	vel.x = vel.y = 0;

}

в setup\(\):

      start\(\);

в draw\(\) рисуем мяч:

	ofSetColor\( 255, 0, 255 \);

	ofFill\(\);

	ofCircle\( ball, rad \);

### ![](/assets/web09.png)

### Примеры того, что еще можно делать с камерой

1\)Оптический поток

2\) Петля камера -&gt; экран -&gt; камера ...



### 















