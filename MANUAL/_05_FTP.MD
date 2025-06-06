# SMB

### Порядок действий

Непосредственно перед установкой обновим данные репозитория, для этого введём в терминале:

```sh
apt update
```

Перейдем непосредственно к установке FTP на Ubuntu. Отправим в терминал команду:

```sh
sudo apt install vsftpd -y
```

Лучше скопировать файл конфигурации по умолчанию, чтобы всегда оставлять себе возможность откатиться до дефолтных настроек FTP на Ubuntu. Копируем командой:

```sh
sudo cp /etc/vsftpd.conf /etc/vsftpd.conf.orig
```

---
Подготовка места для файлов

Создайте каталог для хранения загруженных файлов. Флаг -p создаст все несуществующие вышележащие каталоги. Структура каталогов FTP позволяет систематизировать файлы; позже вы сможете расширить её, добавив каталоги для зарегистрированных пользователей

```sh
sudo mkdir -p /var/ftp/pub
```

Установите на каталог права nobody:nogroup. Позже нужно будет передать все права на файлы пользователю и группе ftp

```sh
sudo chown nobody:nogroup /var/ftp/pub
```

Создайте в каталоге тестовый файл:

```sh
echo "vsftp test file" | sudo tee /var/ftp/pub/test.txt
```

---
Настройка анонимного доступа

В конфигурационном файле хранятся опции vsftp. Некоторые из них нужно отредактировать

```sh
sudo nano /etc/vsftpd.conf
```

Найдите в файле следующие строки и отредактируйте их, как показано ниже:

```sh
. . .
# Allow anonymous FTP? (Disabled by default).
anonymous_enable=YES
# We’ll set the local_enable setting to “NO” because we’re not going to allow users with local accounts to upload files via FTP. The comment in the configuration file can be a little confusing, too, because the line is uncommented by default.
# Uncomment this to allow local users to log in.
local_enable=NO
. . .
```

Добавьте в конфигурационный файл следующие параметры. Файл не имеет чёткой структуры, потому неважно, в какой части файла будут находиться новые опции

```sh
#
# Point users at the directory we created earlier.
anon_root=/var/ftp/
#
# Stop prompting for a password on the command line.
no_anon_password=YES
#
# Show the user and group as ftp:ftp, regardless of the owner.
hide_ids=YES
#
# Limit the range of ports that can be used for passive FTP
pasv_min_port=40000
pasv_max_port=50000
```

Сохраните и закройте файл.

Перезапустите демон:

```sh
sudo systemctl restart vsftpd
```

Команда systemctl не отображает вывод некоторых команд. Чтобы убедиться, что всё прошло успешно, введите:

```sh
sudo systemctl status vsftpd
```

Команда должна вернуть:

```sh
Aug 17 17:49:10 vsftp systemd[1]: Starting vsftpd FTP server...
Aug 17 17:49:10 vsftp systemd[1]: Started vsftpd FTP server.
```

Тестирование анонимного доступа

Откройте в проводнике такую ссылку:

```sh
ftp://<ВАШ_IP_Servera>
```

Если всё работает должным образом, вы увидите каталог pub:

<img src="src\img\_05_SMB\1.png" width="700px"></img>