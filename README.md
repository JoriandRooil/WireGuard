# SnakeGuard
Вы нашли самый простой способ установить <br>
WireGuard и управлять им на любом хосте Linux.

## Функционал

* WireGuard и Web UI
* Список, создание, редактирование, удаление отключение и включение клиентов
* Скачивание конфигурационных файлов для клиентов
* Статистика для каждого клиента
* Генерация QR кода

## Необходимо

* Linux сервер
* Установленный Docker

## Установка

### 1. Установка Docker

Для установки используйте данный код:
```bash
$ curl -sSL https://get.docker.com | sh
$ sudo usermod -aG docker $(whoami)
$ exit
```

### 2. Установка SnakeGuard

Скопируйте и вставьте в вашу консоль данную команду:
```bash
$ docker run -d \
  --name=snakeguard \
  -e WG_HOST=172.0.0.1 \
  -e PASSWORD=19q8xq19e7 \
  -e WG_DEFAULT_DNS=94.140.14.14,94.140.15.15 \
  -e WG_MTU=1384 \
  -v ~/.snakeguard:/etc/wireguard \
  -p 51820:51820/udp \
  -p 51821:51821/tcp \
  --cap-add=NET_ADMIN \
  --cap-add=SYS_MODULE \
  --sysctl="net.ipv4.conf.all.src_valid_mark=1" \
  --sysctl="net.ipv4.ip_forward=1" \
  --restart unless-stopped \
  joriandrooil/snakeguard
```

> Замените IP адрес в строчке `WG_HOST` на ваш IP адрес или домен <br>
> Рекомендуется заменить пароль в строчке `PASSWORD` для безопасности вашего сервера

## Дополнение
Для шифрования трафика вы можете 
использовать протокол DNSCrypt.

### 1. Установка
```bash
sudo add-apt-repository ppa:shevchuk/dnscrypt-proxy
sudo apt update
sudo apt install dnscrypt-proxy
```

### 2. Настройка конфига

По данному адресу переходим в редактор:
```bash
sudo nano /etc/dnscrypt-proxy/dnscrypt-proxy.toml
```

Изменяем данные параметры на свои:
> Укажите локальный IP адрес в строчке `listen_addresses` <br>
> В строчке `server_names` укажатие DNSCrypt сервер

После перезагружаем сервисы данными командами:
```bash
sudo systemctl restart NetworkManager
sudo systemctl restart dnscrypt-proxy
```

#### Для дополнительной безопасности можете использовать данный код для блокировки тунеля
```bash
iptables -A INPUT --proto icmp -j DROP
```
