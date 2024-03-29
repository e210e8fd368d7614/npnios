# Мои действия по установке и настройке сервера приватных подключений
***(ЧЕРНОВИК)***

Поскольку я использую несколько своеобразную систему управления процессами настройки серверов, ввиду их многочисленности, то сначала надо провести подготовку самого сервера, точнее, его операционки.
Линкую нужные файлы с таким рассчетом, что бы можно было редактировать от имени пользователя, который отвечает за администрирование конкретного сервиса или службы.
Для этого просто переношу требуемые файлы в директорию, где будет вестись работа с ними, в частности в директорию этого репозитория, и линкую их в места, где привыкла видеть их операционка.

Чтобы выключить поддержку IPv6 на всех сетевых интерфейсах сразу, открываем на редактирование файл sysctl.conf

`sudo nano /etc/sysctl.conf`
В конец файла добавляем строки:

sysctl.conf

# Turn off IPv6

`net.ipv6.conf.all.disable_ipv6 = 1`
`net.ipv6.conf.default.disable_ipv6 = 1`
`net.ipv6.conf.lo.disable_ipv6 = 1`
`net.ipv6.conf.eth0.disable_ipv6 = 1`

При этом в последней строке, если необходимо, изменяем имя сетевого интерфейса eth0 на тот, который используется у нас. Если в системе несколько интерфейсов, то добавляем по аналогии строку для каждого дополнительного интерфейса.

Для вступления изменений в силу, заставим sysctl перечитать файл /etc/sysctl.conf:

`sysctl -p`

Перезагружаем сервер и проверяем список интерфейсов, где IPv6 интерфейсов уже не должно остаться

`ip addr | grep inet6`

Открываем файл /etc/sysctl.conf и меняем значение net.ipv4.ip_forward c 0 на 1, тем самым разрешая форвардинг пакетов и дописываем две строчки запрещающие ICMP send redirects и ICMP accept redirects:
```
net.ipv4.ip_forward = 1                     # Разрешаем форвардинг пакетов
net.ipv4.conf.default.send_redirects = 0    # Отключаем ICMP send redirects
net.ipv4.conf.default.accept_redirects = 0  # Отключаем ICMP accept redirects
Применяем сделанные изменения
```
`sysctl -p`
