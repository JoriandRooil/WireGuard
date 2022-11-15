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
<pre>
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
</pre>

> Замените IP адрес в строчке `WG_HOST` на ваш IP адрес или домен <br>
> Рекомендуется заменить пароль в строчке `PASSWORD` для безопасности вашего сервера


## Обновление

Для обновления следует выполнить данные команды:

```bash
docker stop snakeguard
docker rm snakeguard
docker pull joriandrooil/snakeguard
```

А после запустить команду из 2 пунка установки
