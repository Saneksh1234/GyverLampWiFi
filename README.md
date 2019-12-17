![PROJECT_PHOTO](https://github.com/vvip-68/GyverLampWiFi/blob/master/proj_img.jpg)
# Крутая WiFi лампа на esp8266 своими руками
* [Описание проекта](#chapter-0)
* [Папки проекта](#chapter-1)
* [Схемы подключения](#chapter-2)
* [Материалы и компоненты](#chapter-3)
* [Как скачать и прошить](#chapter-4)
* [FAQ](#chapter-5)
* [Полезная информация](#chapter-6)

<a id="chapter-0"></a>
## Описание проекта
Этот проект основан на проекте AlexGyver ["Матрица на адресных светодиодах с управлением по Bluetooth"](https://github.com/AlexGyver/GyverMatrixBT)
с реализацией функционала проекта ["Крутая WiFi лампа на esp8266 своими руками"](https://github.com/AlexGyver/GyverLamp)
и его дальнейшем развитии.

### Железо
- Проект собран на базе микроконтроллера ESP8266 в лице платы NodeMCU или Wemos D1 mini (неважно, какую из этих плат использовать!).
- В версии, начиная с v1.01 добавлена поддержка микроконтроллера ESP32
- Вместо адресной ленты может использоваться гибкая адресная матрица 16×16, что выходит дешевле ленты (матрица 16×16 стоит 1500р, она состоит из 256 диодов с плотностью 100 штук на метр. 
  Лента такой же плотности стоит 1000р за метр (за 100 светодиодов). Для склейки матрицы размером 16×16 понадобится 2.5 метра ленты, то есть 2500р. А готовая матрица стоит на 1000р дешевле!).
- Система управляется со смартфона по Wi-Fi, а также “оффлайн” с кнопки на корпусе (сенсорная кнопка на TTP223 или любая физическая кнопка с нормально разомкнутыми контактами).

### Фишки
 - 26 крутых эффектов с поддержкой отображения часов поверх эффектов
 - Настройка скорости и вариаций отображения для каждого эффекта со смартфона
 - Работа системы как в локальной сети, так и в режиме “точки доступа”
 - Система получает точное время из Интернета
 - Управление кнопкой: смена режима, настройка яркости, вкл/выкл, отображение текущего IP адреса лампы
 - Режим будильник-рассвет: менеджер будильников на неделю в приложении

### Изменения функционала лампы по справнению с исходным проектом:
 - Адаптированная программа управления лампой на Andrioid
 - Отображение текущего времени на индикаторе TM1637
 - Отображение текущего времени на матрице поверх эффектов
 - Для ламп с матрицей, свернутой в трубу доступно отображение часов с плавным вращением вокруг матрицы.   
   Таким образом часы будут полностью видны при обороте по кругу вне зависимости от кривизны поверхности плафона лампы.
 - Настройка сервера синхронизации времени из программы на смартфоне 
 - Установка текущего времени со смартфона вручную, если не удалось подключиться к серверу времени NTP
 - Два режима работы индикатора времени TM1637 - светится постоянно или выключается вместе с лампой
 - Пока время не получено с сервера NTP - на индикаторе отображается --:-- вне зависимости от настройки
   "Выключать индикатор при выключении лампы"   
 - Поддержка звука будильника / звука рассвета звуковой платой MP3 DFPlayer
 - Настройки сетевого подключения (SSID и пароль, статический IP) задаются в программе и сохраняются в EEPROM
 - Если не удается подключиться к сети (неверный пароль или имя сети) - создается точка подключения
   с именем LampAP, пароль 12341234, IP 192.168.4.1. Подключившись к точке доступа из приложения
   можно настроить параметры сети. Если после задания параметиров сети WiFi соединение установлено - 
   в приложении на смартфоне виден IP адрес подключения к сети WiFi.  
 - Отображение текущего IP адреса лампы на индикаторе TM1637 или на матрице в режиме бегущей строки
 - Быстрое включение популярных режимов лампы из приложения
 - Два программируемых по времени режима, позволяющие, например, настроить автоматическое выключение лампы в ночное время
   и автоматическое включение лампы вечером в назначенное время

#### Эффекты:
 - Лампа белого или другого выбранного цвета
 - Снегопад
 - Блуждающий кубик
 - Пейнтбол
 - Радуга (горизонтальная, вертикальная, диагональная)
 - Огонь
 - The Matrix
 - Конфетти
 - Звездопад
 - Шумовые эффекты с разными цветовыми палитрами
 - Плавная смена цвета лампы
 - Светлячки

#### Возможности:
- Автоподключение к лампе при запуске
- Настройки яркости лампы из программы или кнопкой

### Кнопка управления режимами, последовательность переключения:
#### Будильник сработал, идет рассвет или мелодия пробуждения
- Любое нажатие кнопки отключает будильник
#### Долгое удержание кнопки 
- При включенной лампе - плавное изменение яркости
- При выключенной лампе - включение яркой белой лампы
#### Однократное нажатие кнопки
- Включение / выключение лампы. При включении возобновляется режим на котором лампа была выключена.
#### Двухкратное нажатие кнопки
- Ручной переход к следующему режиму
#### Трехкратное нажатие кнопки
- Включение демо-режима с автоматической сменой режимов по циклу
#### Четырехкратное нажатие кнопки
- Включение яркой белой лампы из любого режима, даже если лампа "выключена"
#### Пятикратное нажатие кнопки
- На индикаторе TM1637 отображается IP адрес лампы, если подключение к локальной WiFi сети установлено

<a id="chapter-1"></a>
## Папки
**ВНИМАНИЕ! Если это твой первый опыт работы с Arduino, читай [инструкцию](#chapter-4)**
- **libraries** - библиотеки проекта.
- **firmware** - прошивки
- **schemes** - схемы подключения компонентов
- **sounds** - звуковые файлы будильника для размещения на SD-карте
- **Android** - файлы с приложениями, примерами для Android и Thunkable
- **3D_print** - файлы для печати корпуса лампы на 3D принтере

<a id="chapter-2"></a>
## Схема
### С сенсорной кнопкой
![SCHEME](https://github.com/vvip-68/GyverLampWiFi/blob/master/schemes/scheme.jpg)
### С обычной кнопкой
![SCHEME](https://github.com/vvip-68/GyverLampWiFi/blob/master/schemes/scheme_b.jpg)

<a id="chapter-3"></a>
## Материалы и компоненты
### Ссылки оставлены на магазины
Полный список компонентов есть в статье https://alexgyver.ru/matrix_guide/
- NodeMCU http://ali.ski/RgD5P  http://ali.ski/_1FJZ
- Wemos D1 mini http://ali.ski/FuTgbO  http://ali.ski/Z9feWU
- Матрица 16x16 http://ali.ski/BCKQT  http://ali.ski/bRW14  http://ali.ski/X-tBrQ
- Адресная лента (для DIY матрицы) http://ali.ski/2dmOe_  http://ali.ski/rqgqdq  http://ali.ski/4Ma9iH
- Сенсорная кнопка http://ali.ski/aWQBAa  http://ali.ski/rsOrSB
- Резисторы http://ali.ski/UEez2
- БП 5V (брать 3A минимум) http://ali.ski/K-CThT  http://ali.ski/3UWXJ
- MP3 DFPlayer http://ali.onl/1gY1 http://ali.onl/1gY3
- Динамики http://ali.onl/1h3u http://ali.onl/1h3v
- Проводочки http://ali.ski/JQRler  http://ali.ski/_SuCF
- Плафон https://leroymerlin.ru/product/plafon-cilindr-18212968/

## Вам скорее всего пригодится
* [Всё для пайки (паяльники и примочки)](http://alexgyver.ru/all-for-soldering/)
* [Недорогие инструменты](http://alexgyver.ru/my_instruments/)
* [Все существующие модули и сенсоры Arduino](http://alexgyver.ru/arduino_shop/)
* [Электронные компоненты](http://alexgyver.ru/electronics/)
* [Аккумуляторы и зарядные модули](http://alexgyver.ru/18650/)

<a id="chapter-4"></a>
## Как скачать и прошить
* [Первые шаги с Arduino](http://alexgyver.ru/arduino-first/) - ультра подробная статья по началу работы с Ардуино, ознакомиться первым делом!
* Скачать архив с проектом
> На главной странице проекта (где ты читаешь этот текст) вверху справа зелёная кнопка **Clone or download**, вот её жми, там будет **Download ZIP**
* Установить библиотеки в  
`C:\Program Files (x86)\Arduino\libraries\` (Windows x64)  
`C:\Program Files\Arduino\libraries\` (Windows x86)
* **Подключить внешнее питание 5 Вольт**
* Подключить Ардуино к компьютеру
* Запустить файл прошивки (который имеет расширение .ino)
* Настроить IDE (COM порт, модель Arduino, как в статье выше)
* Настроить что нужно по проекту
* Нажать загрузить
* Скачать и установить на смартфон GyverLamp
* Пользоваться  

**Подробная инструкция [тут](https://github.com/vvip-68/GyverLampWiFi/wiki/%D0%9F%D0%BE%D0%B4%D0%B3%D0%BE%D1%82%D0%BE%D0%B2%D0%BA%D0%B0-%D1%81%D1%80%D0%B5%D0%B4%D1%8B-%D0%B4%D0%BB%D1%8F-%D1%81%D0%B1%D0%BE%D1%80%D0%BA%D0%B8-%D0%BF%D1%80%D0%BE%D0%B5%D0%BA%D1%82%D0%B0)**

## Важно
Если проект не собирается (ошибки компиляции) или собирается, но работает неправильно (например вся матрица светится белым и ничего не происходит) - проверьте версии библиотек. Данный проект рассчитан на работу с версииями библиотек поддержки плат ESP версии 2.5.2 и библиотеки FastLED версии 3.2.9 или более новую;

Если в качестве микроконтроллера вы используете Wemos D1 - в менеджере плат для компиляции все равно выбирайте **"NodeMCU v1.0 (ESP-12E)"**, в противном случае, если выберете плату Wemos D1 (xxxx), - будет работать нестабильно, настройки не будут сохраняться в EEPROM, параметры подключения к локальной сети будут сбрасываться каждый раз при перезагрузке, плата вместо подключения к локальной сети будет каждый раз создавать точку доступа.

<a id="chapter-5"></a>
## FAQ
### Основные вопросы
В: Как скачать с этого грёбаного сайта?  
О: На главной странице проекта (где ты читаешь этот текст) вверху справа зелёная кнопка **Clone or download**, вот её жми, там будет **Download ZIP**

В: Скачался какой то файл .zip, куда его теперь?  
О: Это архив. Можно открыть стандартными средствами Windows, но думаю у всех на компьютере установлен WinRAR, архив нужно правой кнопкой и извлечь.

В: Я совсем новичок! Что мне делать с Ардуиной, где взять все программы?  
О: Читай и смотри видос http://alexgyver.ru/arduino-first/

В: Вылетает ошибка загрузки / компиляции!   
О: Читай тут: https://alexgyver.ru/arduino-first/#step-5

### Вопросы по этому проекту

В: Эй чувак! У тебя проект не компилится. Ты файл FastLed.h в проект забыл включить. Выложи!   
О: Это стандартная библиотека для FastLed для управления адресными светодиодами. Идите в менеджер библиотек и установите ее. Или скачайте с [сайта производителя](https://github.com/FastLED/FastLED)

В: Эй чувак! У тебя проект не компилится. Ты файл DFRobotDFPlayerMini.h в проект забыл включить. Выложи!   
О: Это стандартная библиотека для MP3 DFPlayer. Идите в менеджер библиотек и установите ее. Или скачайте с [сайта производителя](https://github.com/DFRobot/DFRobotDFPlayerMini)

В: Собрал, использую NodeMCU/Wemos. Ничего не работает! Мигает один или несколько светодиодов в начале матрицы. И всё.   
О: Производители разных плат (NodeMCU, Wemos) могут использовать различные схемы соединения контактов микроконтроллера ESP8266 к выводам макетной платы. Обычно используемый в проекте пин вывода на ленту приходится или на пин D2 или на пин D4. Для проверки не подключайте сигнальный провод матрицы к микроконтроллеру, вместо этого через резистор коснитесь вывода D2 или D4 пина микроконтроллера. Большая вероятность что матрица заработает с тем или иным вариантом подключения.

В: Собрал, использую NodeMCU. Эффекты работают, но нестабильно. Случайные вспышки на матрице. Буквы бегущей строки прыгают.   
О: NodeMCU v3 чрезвычайно требователен к источнику питания. Ему на вход VIN нужно подавать напряжение в диапазоне 4.7-5 вольт. И не более. Описанные эффекты возникают даже при питании в 5.25 (а тем более - 5.45) вольт. Для проверки - не подключайте +5 вольт от блока питания к NodeMCU совсем, питание подавайте на матрицу непосредственно. Землю NodeMCU и ленты соедините. Подключите сигнальный пин NodeMCU ко входу DIN ленты. Подключите NodeMCU к компьютеру через USB (питание будет поступать отсюда). Должно заработать. Далее регулируйте выходное напряжение своего блока питания.  
В особо тяжелых случаях, когда понижение напряжения питания системы не приводит к желаемым результатам и артефакты, вспышки, дрожание текста бегущей строки продолжается, можно использовать дополнительно схему преобразования уровня от 3.3 вольта с выхода NodeMCU к 5 вольтам входа адресной ленты:  
![SCHEME](https://github.com/vvip-68/GyverLampWiFi/blob/master/schemes/convertor.png)

В: Не компилируется. Выбрана плата "голая ESP8266-12E". Сообщение об ошибке: "D4 was not declared in this scope."   
О: Очевидно производители библиотеки для "голой ESP8266-12E" не определили данную константу. Используйте всесто константы D4 числовое определение пина для вашей платы или выполните компиляцию проекта для плат NodeMCU или WeMos D1 R2.

В: Не компилируется. В сообщении об ошибке содержатся сведения о дублирующихся библиотеках.    
О: В вашей среде установлено две версии одной и той же библиотеки. Обычно это библиотека FastLED - одна версия находится в папке установки среды Ардуино (например в "C:\Program Files (x86)\Arduino\libraries\"), другая - в папке документов пользователя (например "C:\Users\vvip-68\Documents\Arduino\libraries\"). Удалите одну из версий библиотек и попробуйте скомпилировать снова.

В: Не компилируется. В сообщении об ошибке что-то про несоответствие типов.   
О: Обычно такая ситуация возникает в двух случаях:
- выбрана неверная плата. Используйте NodeMCU 1.0 (ESP-12E Module) или Wemos D1 R1. Под эти платы проект собирается, под другие, возможно, нужна модификация кода.
- установлена устаревшая версия библиотек поддержки плат - например для ESP8266 версия библиотеки 2.4.2. Данный проект использует библиотеки для плат ESP8266 версии 2.5. Обновите библиотеки поддержки плат.

В: Подскажите, что не так... C подключением через точку доступа всё исправно работает, а при попытке подключиться к локальной сети не могу законнектиться через приложение. В чем может быть проблема?  
О: Проблема может быть в неправильно указанном статическом адресе / параметрах сети в прошивке. В сектче по умолчанию используется адрес в сети 192.168.0.xxx. Ваш WiFi роутер в зависимости от настроек может создавать сеть в другом диапазоне. Чаще всего это 192.168.1.xxx или 192.168.100.xxx; Проверьте какую сеть создает ваш роутер и укажите в скетче и при подключении приложения к сети именно эту сеть.

В: Автор неверно указал IP адрес лампы 192.168.4.1 в режиме точки доступа. На самом деле он 192.168.4.2 - указываю его, приложение на смартфоне подключается. Правда управлять лампой не получается все равно - она не реагирует на изменения.     
О: Нет, все правильно. IP лампы - 192.168.4.1; Адрес 192.168.4.2 получает ваш смартфон при подключении к точке доступа. Когда в приложении для подключения вы указываете адрес 192.168.4.2 - вы подключаете ваш смартфон с самому себе, а не к лампе. Естественно никакое управление лампой работать не будет.   

В: Лампа создает точку доступа, телефон к ней подключается. В приложении ввожу IP адрес лампы 192.168.4.1, но соединение не происходит. Что я делаю не так?     
О: Некоторые телефоны не могут передавать данные через точку доступа, пока в них активен мобильный интернет. Все передаваемые данные отправляются в интернет, вместо передачи их в точку доступа. В настройках телефона выключите мобильный интернет ("Мобильные данные"). После этого телефон из приложения должен подключиться к лампе.  

В: В скетче есть настройки который задают имя и пароль к локальной сети. Указываю, но к сети даже не пытается подключиться В чем дело?    
#define NETWORK_SSID ""                      // Имя WiFi сети - пропишите здесь или задайте из программы на смартфоне   
#define NETWORK_PASS ""                      // Пароль для подключения к WiFi сети - пропишите здесь или задайте из программы на смартфоне       
О: Эти настройки определяют параметры доступа к сети по умолчанию, которые используются при ПЕРВОЙ загрузке прошивки в лампу. В этот момент они сохраняются в EEPROM и при последующих запусках имя сети и пароль извлекаются из энергонезависимой памяти и используются уже извлеченные значения, а не те, что прописаны в #define. Если вы уже запускали лампу и ПОСЛЕ этого изменили в скетче имя и пароль сети, вам нужно также изменить значение флага, указывающее было ли уже сохранение параметров в EEPROM или еще нет. Этот флаг находится в файле eeprom.ino в первой строке  
#define EEPROM_OK 0x5A                     // Флаг, показывающий, что EEPROM инициализирована корректными данными 
Измените его на любое другое значение, например 0xA5

### Всё собрал строго по инструкции, ничего не работает.

1. Разбираем всё обратно.
2. Берем платку, смотрим чтобы к ней НИЧЕГО не было подключено.
3. Плату в таком состоянии подключаем кабелем USB к компьютеру.
4. Берем последнюю версию [прошивки](https://github.com/vvip-68/GyverLampWiFi/) и загружаем ее в микроконтроллер.
5. Смотрим в сообщениях, что загрузка успешно выполнена и осуществлен перезапуск микроконтроллера, а в мониторе порта, что скетч старотовал, вывел версию прошивки, создал точку доступа LampAP
6. Устанавливаем на смартфон приложение из этого проекта
7. Подключаемся телефоном к точке доступа, приложение подключаем к матрице, пробуем тыкать на кнопки.
8. Отключаем микроконтроллер от компьютера
9. Подключаем блок питания +5 вольт и минусовой провод к матрице. Минусовой провод подключаем также к пину GND микроконтроллера.
Контроллер подключаем USB кабелем к компьютеру, блок питания включаем в сеть.
10. Проверяем, что напряжение питания с блока не превышает +5.25 вольт. Если больше - регулируем его до уровня 4.8 вольта.
11. Сигнальным проводом с матрицы с подключенным резистором 200 Ом тыкаемся поочередно в пины D4 и D2 микроконтроллера. В каком-то из этих двух вариантов на иматрице должно сформироваться осмысленное отображение эффекта или бегущего текста. Что из этого конкретно должно в данный момент отображаться написано в мониторе порта. Запоминаем подключение к какому пину дало результат.
12. Отключаем микроконтроллер и блок питания. Провод +5V с блока питания подключаем к микроконтроллеру на пин его входного напряжения питания (у разных плат обозначен по разному - Vin, Vcc или +5). Сигнальный провод матрицы припаиваем к пину, который определили шагом выше. Очень желательно между пинами Vin и GND микроконтроллера припаять электролитический конденсатор номиналом 1000-4700 мкф, и параллельно ему керамический конденсатор на 33-100 нф.
13. Включаем блок питания в розетку. Лампа должна заработать.
14. Собираем всю конструкцию в корпус. 

<a id="chapter-6"></a>
## Полезная информация
* [Cайт Alex Gyver](http://alexgyver.ru/)
* [Канал Alex Gyver на YouTube](https://www.youtube.com/channel/UCgtAOyEQdAyjvm9ATCi_Aig?sub_confirmation=1)
* [YouTube канал про Arduino](https://www.youtube.com/channel/UC4axiS76D784-ofoTdo5zOA?sub_confirmation=1)
* [Видеоуроки по пайке](https://www.youtube.com/playlist?list=PLOT_HeyBraBuMIwfSYu7kCKXxQGsUKcqR)
* [Видеоуроки по Arduino](http://alexgyver.ru/arduino_lessons/)
