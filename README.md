# arduino-iambic-keyer-controller
Arduino based Iambic B / Iambic A keyer - radio telegraph manipulator controller for ham CW with additional TX out and programmable delay between TX and Key OUT

Автоматический телеграфный ключ с ямбическим режимом работы (A и B), а также дополнительным выходным сигналом TX для переключения трансивера в режим передачи. При этом он опережает сигнал выхода ключа Key OUT  на 50 мс, что бы все механические коммутаторы (рэле) трансивера и усилителя мощности переключились в нужное состояние. По завершении манипуляции через 0.8 сек сигнал TX обнуляется.

Для реализации проекта я использовал Arduino Nano из-за её малых размеров, но подойдет любая другая аналогичная или подобная плата. Нужно только внимательно отследить соответствие номеров ножек (“распиновку”) с их программным обозначением.

![Принципальная схема манипулятора](KeyerYambic10.svg "Принципальная схема манипулятора")

<b>Описание входов и выходов:</b><br>
D3 - вход Точка<br>
D2 - вход Тире<br>
<i>Примечание: Точки и тире замыкать на землю. Для инверсии поменять ножки местами.</i><br>
D4 - выход ключа Key OUT.<br>
D5 - выход TX<br>
D11 - желтый светодиод Key OUT<br>
D12 - красный светодиод TX<br>
D8 - выход звукового сигнала самоконтроля<br>
A0 - вход регулятора тона (потенциометр 20К)<br>
A1 - вход регулятора скорости (потенциометр 20К)<br>

<b>Iambic A и Iambic B:</b><br>
В интернете есть несколько статей, описывающих отличие режима A и B. Если говорить в двух словах, то режим B – это режим с памятью противоположного знака. Т.е. если во время формирования текущего знака коснуться противоположного и отпустить ключ, то по его завершении последует противоположный. В режиме A не последует. Лично мне комфортен режим Iambic B. Переключиться очень легко, поменяв в программе начальное значение переменной.<br>
yaType = 1; // Iambic B<br>
yaType = 0; // Iambic A<br>

<b>Алгоритм работы:</b><br>
Работа программы построена на счетчике микросекунд micros(). Действия манипулятора задают значения таймингов t0, t1, t2, t3, t4, на основе которых формируется текущий знак. Предыдущий алгоритм задавал тайминги на абсолютной шкале времени, что было вполне логично. Но из-за переполнения счетчика микросекунд каждые 71.58 мин (32 бита) возникали неприятные эффекты. Хотя и не критические. Для их полного устранения я перешел к относительным таймингам и теперь переполнение ни на что не влияет, даже на саму манипуляцию.<br>
Для формирования звукового сигнала самоконтроля применяется стандартная функция tone(). Никакие внешние библиотеки не используются. Частота тона и скорость передачи задаются с помощью потенциометров и входов АЦП A0, A1. Поскольку регулятор скорости – это фактически регулятор длительности точки, для более плавной её регулировки на больших скоростях я разбил диапазон на 4 сегмента.<br>

<b>Начальные установки:</b><br>
rat = 3;  // Соотношение тире/точка = 3<br>
pause = 1E6 * 0.8;  // Длительность сигнала TX после завершения манипуляции, 0.8 сек<br>
dtConst = 1E3 * 50;  // Задержка первого знака. Cигнал TX опережает Key OUT на 50 мс<br>

<b>Прилагаемые файлы:</b><br>
KeyerYambic10.svg – принципальная схема манипулятора<br>
Keyer_Yambic_009.ino – программа для Arduino<br>
