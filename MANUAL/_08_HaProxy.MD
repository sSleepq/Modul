# HaProxy

### Порядок действий

Непосредственно перед установкой обновим данные репозитория, для этого введём в терминале:

```sh
apt update
```

Перейдем непосредственно к установке HaProxy на Ubuntu. Отправим в терминал команду:

```sh
sudo apt install haproxy -y
```

> Также нужно установить Nginx для проверки

После выполнения всех настроек и загрузки haproxy можно перейти к настройке

Сделайте копию конфигурационного файла haproxy:

```sh
sudo cp /etc/haproxy/haproxy.cfg /etc/haproxy/haproxy.backup
```

---
Откройте конфигурационный файл

```sh
sudo nano /etc/haproxy/haproxy.cfg
```

Все команды дописываем в конце файла

Для начала укажем, что HAProxy будет принимать все входящие подключения на 8080 порт, а так же сервер по умолчанию:

```sh
frontend website
       bind :8080
       default_backend main
```

В качестве бэкенд-серверов у нас будем мы сами, с установленным nginx

```sh
backend main
       server main1 <Ваш Айпи Адресс>:80
```

Сохраним конфиг и перезапустим HAProxy:

```sh
sudo systemctl restart haproxy
```