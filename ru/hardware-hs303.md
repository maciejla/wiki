# OpenIPC Wiki
[Оглавление](../README.md)

Switcam HS-303
--------------

Компания-производитель выпускала IP камеры Switcam HS-303 в трёх версиях,
значительно отличающихся аппаратно между собой. На данный момент проект
OpenIPC поддерживает прошивку без разборки всех трёх версий камер, однако
есть некоторые ограничения по настройке HS303 v3. Сейчас мы работаем над
созданием единой универсальной прошивки для всех трёх версий видеокамеры
Switcam HS-303.

Обсуждение проекта и возможностей прошивок (на русском языке) возможно в
открытой [Telegram](https://t.me/openipc_modding) группе нашего проекта.

Визуальное отличие устройств по цвету:

* HS303 v1 - имеет чёрно-белый цвет
* HS303 v2 - имеет черный цвет
* HS303 v3 - имеет белый цвет и более вытянутую вертикально форму

**Выбирайте правильный тип прошивки для своей камеры !**



## Подготовка перед прошивкой и эксплуатацией

Для комфортной работы в камерами OpenIPC в Windows необходимо установить программы:

* [Putty](https://www.chiark.greenend.org.uk/~sgtatham/putty/latest.html) для доступа по протоколу ssh (лучшая замена telnet)
* [WinSCP](https://winscp.net/eng/docs/lang:ru) для доступа к файловой системе по протоколу scp
* [Notepad++](https://notepad-plus-plus.org/) в качестве редактора файлов на Linux камере

Для входа по SSH необходимо использовать имя пользователя root, без пароля



## HS303 v1/v2/v3

### Краткая инструкция установки OpenIPC v2.2 (на 2022.10.02)

- подключите SD/MMC карту к ПК и разбейте её на два раздела
- карта может быть любого размера, но первый раздел должен быть не более 2GB
- отформатируйте SD/MMC карту как FAT (ограничение 2GB)
- распакуйте архив и поместите содержимое папки OpenIPC_PayWall в корень SD/MMC карты
- внесите правки в файл autoconfig/etc/network/interfaces (см.ниже)
- вставьте карту памяти в слот и подайте питание на камеру
- через 1 минуту (обновится загрузчик), сделайте сброс питания на пару секунд
- примерно через 2-3 минуты камера будет прошита и станет щелкать ИК фильтром
- сделайте сброс питания на пару секунд
- дождитесь двойного щелчка ИК фильтра и извлеките SD карту
- камера готова к эксплуатации

По-умолчанию камера пытается соединиться с точкой доступа с именем OpenIPC_NFS
и паролем сети project2021. Для изменения имени сети и пароля перед прошивкой
устройства, откройте файл autoconfig/etc/network/interfaces на SD/MMC карте в
редакторе Notepad++ (для Windows), выбрав при этом кодировку UTF-8 и впишите
свои значения.

После прошивки устройства, каталог autoconfig можно удалить или перенести в
место для хранения резервных копий на ПК.

Для восстановления настроек, достаточно записать сохраненные каталог и фаил в
корень SD карты, вставить её в выключенную камеру и подать питание.

Если вы прошиваете серию камер, обратите внимание на то, что после полной инициализации
прошитое устройство самостоятельно удаляет файлы обновлений, чтобы не происходило
зацикливания процесса прошивки, если пользователь забудет SD карту в камере.
Таким образом, удаленные файлы нужно восстанавливать перед прошивкой каждого нового
экземпляра камеры.

При использовании SD карты для записи видео (после прошивки), логический раздел можно
увеличить до 32GB.




#### Особенности релиза

- Что-бы устройство работало максимально стабильно, по-умолчанию отключены 
  дополнительные сервисы и службы: HLS, OSD, Motion и RTSP суб-поток;
- Для тех кто будет перепрошивать камеры с OpenWrt понадобится дополнительное
  действие в виде разового выполнения команды в старой прошивке - зайдите на
  камеру по SSH и выполните команду `flash_eraseall -j /dev/mtd4; reboot -f`



### Наиболее актуальные вопросы и ответы

#### Где можно взять прошивку для устройств Ростелеком Switcam HS303 v1/v2 ?

Готовые файлы для прошивки и базовая техподдержка доступны в телеграм группе
через PayWall сервис [здесь](https://paywall.pw/openipc).

Пошаговая инструкция по прошивке устройств и несколько ответов на часто задаваемые
вопросы находятся [здесь](https://github.com/OpenIPC/wiki/blob/master/ru/hardware-hs303.md).

Исходные коды проекта, для тех кому претит PayWall или кто хочет собрать прошивку
полностью [самостоятельно](https://github.com/OpenIPC).

Ваше право выбора в действиях полностью соблюдено. Успехов в техническом творчестве !

#### Как узнать какой IP адрес у прошитой камеры и как зайти на неё ?

Если внесены корректные данные по настройке WiFi интерфейса в конфигурационный
файл (SSID и ключ), то вы можете найти IP адрес камеры на своём роутере в списке
подключенных устройств с пометкой "OpenIPC".
Интерфейс управления камерой доступен в браузере на порту 85, а доступ по SSH
возможен на стандартном порту 22 с использованием логина root, без пароля при
первом подключении.


#### Что делать если камера прошилась но нет связи по WiFi ?

Проверте настройки на WiFi роутере, выставьте жестко любой канал, например 5
(не Auto), смените ширину полосы на 20MHz и установите режим работы B/G only.


#### Как можно обновить прошивку до последней актуальной версии через SSH ?

Зайдите на камеру по протоколу SSH через программу Putty (логин root, без пароля,
при первом подключении или с паролем, который установили в WEB, 22 порт) и
выполните команду:

```
sysupgrade -k -r
```

При наличии интернета, камера автоматически подключится к GitHub, скачает
и установит самые последние обновления.


#### Как можно обновить прошивку до последней актуальной версии через WEB ?

Зайдите на камеру через браузер указав IP адрес камеры и порт подключения 85,
например вот так:

```
http://192.168.1.10:85
```

Введите дефолтные логин и пароль (логин admin, пароль 12345).
После захода на камеру, установите новый пароль (не забудьте его записать!).
В разделе Firmware/Прошивка выполните действия, которые указаны в подсказках.

#### Как зарегистрировать данный тип камеры на сервисе ipeye.ru ?

В личном кабинете IPeye (зайти обязательно через браузер) добавить камеру по ID,
в качестве которого использовать MAC написанный в нижнем регистре (маленькими 
буквами). MAC написан на титульной странице WEB-интерфейса на 85 порту.

Перед этим необходимо включить поддержку протокола IPeye во вкладке Majestic в
WEB-интерфейсе или присвоить параметру `ipeye.enabled` статус `true` в конфигурационном
файле `/etc/majestic.yaml`, а так-же перезагрузить камеру.


