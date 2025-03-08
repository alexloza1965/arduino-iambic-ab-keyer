# arduino-iambic-ab-keyer
Arduino based Iambic B / Iambic A keyer - radio telegraph manipulator controller for ham CW with additional TX out and programmable delay between TX and Key OUT

Автоматический телеграфный ключ с ямбическим режимом работы (A и B), а также дополнительным выходным сигналом TX для переключения трансивера в режим передачи. При этом он опережает сигнал выхода ключа Key OUT  на 50 мс, что бы все механические коммутаторы (рэле) трансивера и усилителя мощности переключились в нужное состояние. По завершении манипуляции через 0.8 сек сигнал TX обнуляется.
Для реализации проекта я использовал Arduino Nano из-за её малых размеров, но подойдет любая другая аналогичная или подобная плата. Нужно будет только внимательно отследить соответствие номеров ножек (“распиновку”) с их программным обозначением.

<b>Описание входов и выходов:</b><br>
D3 - вход Точка.<br>
D2 - вход Тире.<br>
Примечание: Точки и тире замыкать на землю. Для инверсии поменять ножки местами.<br>
D4 - выход ключа Key OUT, D5 - выход TX.<br>
D11 - желтый светодиод Key OUT.<br>
D12 - красный светодиод TX.<br>
D8 - выход звукового сигнала самоконтроля.<br>
A0 - вход регулятора тона (потенциометр 20К).<br>
A1 - вход регулятора скорости (потенциометр 20К).<br>
