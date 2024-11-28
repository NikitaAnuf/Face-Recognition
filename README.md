**Модель распознавания лиц** <br />
	Распознавание лиц на изображении основано на предобученной нейронной сети DNN, её использование описано в статье https://waksoft.susu.ru/2021/04/02/raspoznavanie-licz-s-ispolzovaniem-opencv-v-python/.
	Раздел "Распознавание лиц с помощью SSD"

**Работа приложения** <br />
  Приложение берёт файл test.png из папки images и отображает окно с новым изображениями, на котором:
 - Красный квадрат: вероятность, что внутри квадрата расположено лицо выше 50%, точная вероятность указывается текстом на картинке
 - Зелёный квадрат: вероятность, что внутри квадрата расположено лицо ниже 50%, но модель всё равно отмечает, что это лицо

**Docker** <br />
	Файл Docker запускает bash-скрипт entrypoint.sh, который в свою очередь запускает приложение <br />
 Формирование образа имеет базовую структуру: ```docker build -t <название образа> .``` <br />
 С формированием контейнера всё немного сложнее, есть один важный параметр: ```docker run --name <название контейнера> -e DISPLAY=<IPv4 адрес хоста + адрес дисплея, обычно 0:0> <название образа>```
 Переменная среды ```DISPLAY``` отвчает за отображение результатов работы приложения в контейнере на экране хост-машины, для проброса использовался X Server VcXsrv,
 его установка и настройка для работы с Docker-контейнерами описана здесь: https://dev.to/darksmile92/run-gui-app-in-linux-docker-container-on-windows-host-4kde.
 Сама переменная среды должна быть в таком формате 192.168.1.1:0.0, до двоеточия IPv4 хоста, после - IP дисплея, обычно это :0.0. Я задал её как переменную среды и использовал в PowerShell как $env:DISPLAY <br />

 Результаты работы на хосте и в контейнере будут загружены в папку results в репозитории. <br />

 **Github Actions CI** <br />
 	Настроен пайплайн для автоматической  загрузки новой версии образа приложения на DockerHub, репозиторий называется nikitaanuf/face-recognition, теги версии - SHA GitHub. Ссылка на репозиторий - https://hub.docker.com/repository/docker/nikitaanuf/face-recognition/general?editDescription=true.
	В него автоматически загружено несколько версий образов при коммите в репозиторий GitHub.