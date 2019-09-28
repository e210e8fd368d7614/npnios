# Мои действия по установке и настройке сервера приватных подключений
***(ЧЕРНОВИК)***

Открываем файл /etc/sysctl.conf и меняем значение net.ipv4.ip_forward c 0 на 1, тем самым разрешая форвардинг пакетов и дописываем две строчки запрещающие ICMP send redirects и ICMP accept redirects:
```
net.ipv4.ip_forward = 1                     # Разрешаем форвардинг пакетов
net.ipv4.conf.default.send_redirects = 0    # Отключаем ICMP send redirects
net.ipv4.conf.default.accept_redirects = 0  # Отключаем ICMP accept redirects
Применяем сделанные изменения
```
`sysctl -p`
