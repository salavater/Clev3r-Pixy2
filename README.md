# Eng:

## Main information:
Pixy 2 is a camera with a built-in processor that can process images and provide ready-made information about the image.
Pixy official website: https://pixycam.com/pixy2/ 
Pixycam 2 documentation: https://docs.pixycam.com/wiki/doku.php?id=wiki:v2:start 

For the camera to work correctly with ev3, you need to install pixyMon v2 (https://pixycam.com/downloads-pixy2/ ), install “general frameware”, and in the settings in the “interface” menu select I2C mode (not lego I2C!!!)

This repository has a module for running ev3 with pixie 2 camera on clover. As well as test programs to check the functionality of the camera.

Repository contents:
“Pixy2.bpm” - main module
“getBlocks_test.bp” - a program that tracks specified colored objects
“specifics_test.bp” - a program for testing basic camera functions
“getMainFeature_test.bp” - a program that tracks straight lines (for example, a line)
“getRGB_test.bp” - a program that gets the rgb of “each” pixel (takes a photo)
“_internal/ and rgbToPNG.exe” - a program for saving an image in png format based on the results of “getRGB_test.bp”

You can see how test programs work here: [*link to video*](https://www.youtube.com/watch?v=GoYYRl5KZRw&ab_channel=SalavatYakupov)
If you move the “_internal” folder, the “rgbToPNG.exe” application, the text file “rgb.txt” into one folder (this file must be downloaded from the ev3 unit after running the “getRGB_test.bp” program, an image in RGB format is written to this file) and run “rgbToPNG.exe”, the application will create a PNG image in this folder

**I divided the commands for working with pixies (documentation) into 4 categories:**

**- Specifics**

**- Color Connected Components**

**- Line Tracking**

**- Video**

green commands - already implemented and in the module

red commands are commands that are described in the documentation (https://docs.pixycam.com/wiki/doku.php?id=wiki:v2:porting_guide ), but have not yet been added to the module


![main](https://github.com/salavater/Clev3r-Pixy2/blob/main/1.png)


## Specifics:

**Specific** these are general commands, such as turning on the backlight, getting fps, etc.

**getVersion()** - this command returns information about the current camera firmware
The command has 2 input parameters: camera port, I2C camera address.
Returns 5 output parameters about the current firmware version.

**printVersion()** - this command prints information about the camera firmware in a convenient form on the block screen.
The command has 2 input parameters: camera port, I2C camera address.

**getResolution()** - This command returns the camera resolution.
The command has 2 input parameters: camera port, I2C camera address.
Returns 2 parameters: the number of pixels in width and the number of pixels in height.

**setCameraBrightness()** - this command is not implemented(

**setServos()** - this command is not implemented(

**setLED()** - this command lights the RGB LED based on the specified RGB components.
The command has 5 input parameters: camera port, camera I2C address, R component, G component, B component.

**setLamp()** - This command turns the top and bottom LEDs on and off.
The command has 4 input parameters: camera port, I2C camera address, status.
Upper LEDs (0-1), status of the lower LED (0-1).
0 - disabled
1 - enabled

**getFPS()** - This command returns the current frames per second refresh rate of the camera. Attention!!! ev3 can accept significantly fewer frames per second due to the low information transfer rate of the I2C bus
The command has 2 input parameters: camera port, I2C camera address.
Returns 1 parameter: the current number of frames per second.

## Color Connected Components:

**Color Connected Components** are commands that allow you to track specified objects. Objects are set through the PixyMon2 application (https://pixycam.com/downloads-pixy2/ )

**getMainBlock()** - This command returns information about the largest object being tracked.
The command has 2 input parameters: camera port, I2C camera address.
Returns 8 parameters: object number, object center along the X axis, object center along the Y axis, object width, object height, angle, identification number, number of frames during which the object is in the field of view (0-255).

For each new object in the frame, the camera gives its own unique identification number. If you remove an object from the frame and move it back into the frame, the ID number will be different.

For normal objects, the angle will always be 0. For color codes, the angle is the degree of inclination of the code (-180/180).
If the tracked object is a color code, then instead of the object number there will be a color code (does not work correctly yet).

**getBlocks()** - This command returns information about all tracked objects. Please note that this command returns information about objects in the form of arrays, where counting starts from index 0. Objects in the array are sorted in descending order, that is, the 0th element of the array will contain information about the largest object, etc.
The command has 2 input parameters: camera port, I2C camera address.
Returns 9 parameters (1 numeric value and 8 numeric arrays): the number of objects in the frame (numeric value, subsequent arrays), object number, object center along the X axis, object center along the Y axis, object width, object height, angle, identification number, the number of frames during which the object is in the field of view (0-255).

## Line tracking:

**Line Tracking** are commands that allow you to track straight lines (for example, to follow a line).
**getMainFeature()** - This command returns information about a vector, a vector is a line between two points. A vector is the main (largest) straight line in the frame.
The command has 2 input parameters: camera port, I2C camera address.
Returns 6 parameters: the x-coordinate of the starting point,
y-coordinate for start point, x-coordinate for end point, y-coordinate for end point, ID number, flag

For each new object in the frame, the camera gives its own unique identification number. If you remove an object from the frame and move it back into the frame, the ID number will be different.
A flag is unique information about a vector, for example, if the vector is adjacent to an intersection, then the flag will be equal to 4, otherwise it will be equal to 0.

**getAllFeatures()** - this command is not implemented(

**setMode()** - this command is not implemented(

**setNextTurn()** - this command is not implemented(

**setDefaultTurn()** - this command is not implemented(

**setVector()** - this command is not implemented(

**reverseVector()** - this command “reverses” the direction of the vector (switches the starting and ending points).
The command has 2 input parameters: camera port, I2C camera address.

## Video:

**Video** are commands that allow you to obtain information about a picture in the RGB format of each pixel
**getRGB()** - This command returns the RGB values of a specified area in a frame.
The command has 5 input parameters: camera port, camera I2C address, x coordinate of the area center, y coordinate of the area center, saturation (0-1).
Returns 3 parameters: RGB of the specified area

An “area” is a collection of 25 pixels.
If the saturation input parameter is 0, then the RGB values will be “raw”. If the input parameter “saturation” is equal to 1, then the largest value among RGB will be equal to 255, the rest will be stretched in proportion to the maximum

# Рус:
## Общая информация:

Пикси2 это камера с встроенным процессором умеющим обрабатывать изображение и выдавать готовую информацию о картинке. 
Официальный сайт пикси: https://pixycam.com/pixy2/
Документация пикси 2: https://docs.pixycam.com/wiki/doku.php?id=wiki:v2:start

Для корректной работы камеры с ev3 нужно установить pixyMon v2 (https://pixycam.com/downloads-pixy2/ ), установить “general frameware”, и в настройках в меню “интерфейс” выбрать режим I2C(не lego I2C!!!)

В этом репозитории есть модуль для работы ev3 с камерой пикси 2 на клевере. А также тестовые программы для проверки функционала камеры.

Содержание репозитория:
“Pixy2.bpm” - основной модуль
“getBlocks_test.bp” - программа отслеживающая заданные цветные объекты
“specifics_test.bp” - программа для проверки базовых функций камеры
“getMainFeature_test.bp” - программа отслеживающая прямые (например линию)
“getRGB_test.bp” - программа получающая rgb “каждого” пикселя (делает фотографию)
“_internal/ and rgbToPNG.exe” - программа для сохранения картинки в формате png по результатам работы “getRGB_test.bp”

Посмотреть как работают тестовые программы можно здесь: [*ссылка на видео*](https://www.youtube.com/watch?v=GoYYRl5KZRw&ab_channel=SalavatYakupov)
Если в одну папку переместить папку “_internal”, приложение “rgbToPNG.exe”, текстовый файл “rgb.txt”(этот файл нужно скачать с блока ev3 после выполнения программы “getRGB_test.bp” , в этот файл записывается картинка в формате RGB) и запустить “rgbToPNG.exe”, приложение создаст в этой папке картинку в формате PNG 

Команды для работы с пикси я(документация) разделил на 4 категории:
- Specifics 
- Color Connected Components
- Line Tracking
- Video

зеленые команды - уже реализованы и есть в модуле

красные команды - это команды которые описаны в документации( https://docs.pixycam.com/wiki/doku.php?id=wiki:v2:porting_guide ), но еще не добавлены в модуль


![main](https://github.com/salavater/Clev3r-Pixy2/blob/main/1.png)

## Specific:

Specific - это общие команды, по типу включения выключения подсветки, получения fps и т д

getVersion() - эта команда возвращает информацию о текущей прошивке камеры
Команда имеет 2 входных параметра: порт камеры, I2C адрес камеры.
Возвращает 5 выходных параметра о текущей версии прошивки.

printVersion() - эта команда печатает информацию о прошивке камеры в удобном виде на экране блока.
Команда имеет 2 входных параметра: порт камеры, I2C адрес камеры.

getResolution() - эта команда возвращает разрешение камеры.
Команда имеет 2 входных параметра: порт камеры, I2C адрес камеры.
Возвращает 2 параметра: количество пикселей по ширине и количество пикселей по высоте.

setCameraBrightness() - эта команда не реализована(

setServos() - эта команда не реализована(

setLED() - эта команда зажигает RGB светодиод по заданным компонентам RGB компонентам.
Команда имеет 5 входных параметров: порт камеры, I2C адрес камеры, компонент R, компонент G, компонент B.

setLamp() - эта команда включает и выключает верхние и нижний светодиод.
Команда имеет 4 входных параметра: порт камеры, I2C адрес камеры, состояние.
Верхних светодиодов(0-1), состояние нижнего светодиода(0-1).
0 - выключено
1 - включено

getFPS() - эта команда возвращает текущую частоту обновления кадров в секунду камеры. Внимание!!! ev3 может принять значительно меньшее количество кадров в секунду, из-за маленькой скорости передачи информации шины I2C
Команда имеет 2 входных параметра: порт камеры, I2C адрес камеры.
Возвращает 1 параметр: текущее количество кадров в секунду.

## Color Connected Components:
Color Connected Components - это команды позволяющие отслеживать заданные объекты. Объекты задаются через приложение PixyMon2(https://pixycam.com/downloads-pixy2/)

getMainBlock() - это команда возвращает информацию о самом большом отслеживаемом объекте.
Команда имеет 2 входных параметра: порт камеры, I2C адрес камеры.
Возвращает 8 параметров: номер объекта, центр объекта по оси X, центр объекта по оси Y, ширина объекта, высота объекта,  угол, идентификационный номер, количество кадров в течении которых объект находится в поле зрения(0-255).

Для каждого нового объекта в кадре камера даёт свой уникальный идентификационный номер. Если убрать объект из кадра и заново переместить в кадр, идентификационный номер будет отличаться.

Для обычных объектов угол будет всегда равен 0. Для цветовых кодов, угол это степень наклонения кода(-180/180).
Если отслеживаемый объект это цветовой код, тогда вместо номера объекта будет цветовой код(пока работает не корректно).

getBlocks() - эта команда возвращает информацию о всех отслеживаемых объектах. Обратите внимание, что эта команда возвращает информацию об объектах в виде массивов, где отсчёт начинается с 0 индекса. Объекты в массиве отсортированы в порядке убывания, то есть в 0 элементе массива будет информация о самом большом объекте и т.д.
Команда имеет 2 входных параметра: порт камеры, I2C адрес камеры.
Возвращает 9 параметров(1 числовое значение и 8 числовых массива): количество объектов в кадре(числовое значение, последующие - массивы), номер объекта, центр объекта по оси X, центр объекта по оси Y, ширина объекта, высота объекта,  угол, идентификационный номер, количество кадров в течении которых объект находится в поле зрения(0-255).

## Line Tracking:

Line Tracking - это команды позволяющие отслеживать прямые(например для следования по линии).
getMainFeature() - эта команда возвращает информацию о векторе, вектор представляет собой линию, между двумя точками. Вектор это главная(самая большая) прямая линия в кадре.
Команда имеет 2 входных параметра: порт камеры, I2C адрес камеры.
Возвращает 6 параметров: координату по оси x для начальной точки,
координату по оси y для начальной точки, координату по оси x для конечной точки, координату по оси y для конечной точки, идентификационный номер, флаг

Для каждого нового объекта в кадре камера даёт свой уникальный идентификационный номер. Если убрать объект из кадра и заново переместить в кадр, идентификационный номер будет отличаться.
Флаг - это уникальная информация о векторе, например если вектор прилегает к перекрестку, тогда флаг будет равен 4, иначе равен 0.

getAllFeatures() - эта команда не реализована(

setMode() - эта команда не реализована(

setNextTurn() - эта команда не реализована(

setDefaultTurn() - эта команда не реализована(

setVector() - эта команда не реализована(

reverseVector() - эта команда “разворачивает” направление вектора(меняет начальную и конечную точку местами).
Команда имеет 2 входных параметра: порт камеры, I2C адрес камеры.


## Video:

Video - это команды позволяющие получать информацию о картинке, в формате RGB каждого пикселя
getRGB() - эта команда возвращает значения RGB указанной области в кадре.
Команда имеет 5 входных параметров: порт камеры, I2C адрес камеры, координату x центра области, координату y центра области, насыщенность(0-1).
Возвращает 3 параметра: RGB указанной области

“Область” - это совокупность 25 пикселей.
Если входной параметр “насыщенность” будет равен 0, тогда значения RGB будут “сырыми”. Если входной параметр “насыщенность” будет равен 1, тогда наибольшее значение среди RGB будет равен 255, остальные растянутся пропорционально максимальному 
