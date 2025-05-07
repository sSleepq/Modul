# DNS

### Порядок действий

Настройка прямой и обратной зоны DNS

- Cоздадим зону DNS

Добавьте информацию о зоне в конфигурацию
```sh
sudo nano /etc/bind/named.conf.local
```
- Затем добавьте следующие строки

Далее идут команды в которых нужно прописывать `СВОИ ДАННЫЕ`

```sh
zone "domain-name.com" {
        type master;
        file "/etc/bind/db.domain-name.com";
        allow-transfer { 10.1.1.10; };
        also-notify { 10.1.1.10; };
};
```
`domain-name.com` меняется на название вашей придуманной доменной зоны
10.1.1.10 меняется на ip адресс Ubuntu servera со стороны `Bridge`

### Выполните перезапуск службы

```sh
systemctl reload bind9
```

- Конфигурационный файл зоны

Создайте файл зоны из шаблона и откройте его.

```sh
sudo cp /etc/bind/db.local /etc/bind/db.domain-name.com
sudo nano /etc/bind/db.domain-name.com
```

Замените `localhost` в записи SOA полным доменным именем вашего сервера с указанием «.» в конце.

А `root` замените на любое другое имя.

<img src="src\img\_02_DNS\1.png"></img>

Перезапустите rndc

```sh
sudo rndc reload
```