Предисловие:
Наше любимое государство делает все, чтобы закрутить гайки, этот метод не надежен, но это одно из немногих, что у нас есть. Не унываем и верим в светлое будущее.

Как обойти белые списки?
Используем схему из двух хопов, логика такая, что мы арендуем сервер у Yandex Cloud или VK Cloud, во время действия вайтлистов подключаемся к нему, а трафик перенаправляем на иностранный сервер.
Таким образом, нужно арендовать недорогой иностранный сервер и сервер у клаудов, который не такой уж и дешевый, но что поделать

Схема будет выглядеть так:
Мы --> VK Cloud/Yandex Cloud -- > Иностранный сервер

Инструкция по настройке иностранного сервера:

1. Арендуйте любой иностранный VPS сервер за копейки, берем самую дешевую конфигурацию.

2. Получите данные от ssh, это юзернейм (в 99% случает это будет root) и пароль

3. Подключитесь консолью по ssh к серверу. А именно вбейте WIN + R, введите "cmd", откройте консоль, подключитесь к серверу командой ssh username@IPсервера
Если пишет "Are you sure you want to continue connecting (yes/no/[fingerprint])?", просто введите yes

4. Далее вводим
```
 apt update -y

 bash -c "$(curl -L https://github.com/XTLS/Xray-install/raw/main/install-release.sh)" @ install
```
5. Создаем необходимые для конфига данные
```
 xray uuid
 xray x25519
```

uuid - это условный аккаунт
xray x25519 - генерирует 2 ключа, приватный и публичный, просто запомним это

6. Пишем
```
nano /usr/local/etc/xray/config.json
```
Вставляем туда
```
{
"log": {
"loglevel": "debug",

"access": "/var/log/xray/access.log",
 "error": "/var/log/xray/error.log"
 },
 "inbounds": [
 {
 "tag": "proxy",
 "port": 443,
 "protocol": "vless",
 "settings": {
 "clients": [
 {
 "id": "ВАШ UUID",
 "flow": "xtls-rprx-vision"
 }
 ],
 "decryption": "none"
 },
 "streamSettings": {
 "network": "tcp",
 "security": "reality",
 "realitySettings": {
 "dest": "www.bing.com:443",
 "serverNames": [
 "www.bing.com"
 ],

"privateKey": "ВАШ ПРИВАТНЫЙ КЛЮЧ",
"shortIds": [
 "aabbccdd"
 ]
 }
 },
"sniffing": {
 "enabled": true,
 "destOverride": [
 "http",
 "tls",
 "quic"
 ],
 "routeOnly": true
 }
 }
 ],
 "outbounds": [
 {
 "protocol": "freedom",

"tag": "direct"
 }
 ]
 }
```
7. ctrl + O, - сохраняем документ,
Enter - подтверждаем,
ctrl + X - выходим из документа

8. Запускаем процесс
```
 systemctl start xray
```

9. Смотрим статус программы
```
 systemctl status xray
```

Если Active: `active (running)`, значит все ок

Устанавливаем nekobox https://github.com/MatsuriDayo/nekoray/releases

Открываем nekobox, ПКМ по свободному полю и выбираем создать новый профиль

<img width="332" height="335" alt="image" src="https://github.com/user-attachments/assets/0e960bb2-827d-4468-a5b5-b17c6f097220" />


12. Заполняем по форме (только вместо google www.bing.com)

<img width="816" height="559" alt="image" src="https://github.com/user-attachments/assets/306c8f04-5c47-4663-aa53-6b172e0c1ede" />

Помните, что 1 профиль = одно подключение. Т.е. одновременно подключиться к нему может только 1 устройство, каждый профиль имеет уникальный UUID

13. Чтобы включить VPN устанавливаем 2 галочки
<img width="282" height="83" alt="image" src="https://github.com/user-attachments/assets/dcadf973-1302-4395-aa37-effc11bb3867" />


ПКМ по строке подключения и запускаем

<img width="447" height="339" alt="image" src="https://github.com/user-attachments/assets/e225cda6-2035-469b-ac8f-46f5a53bcb3f" />


14. Проверяем на 2ip.ru получили ли мы иностранный IP адрес, если видим иностранный, значит VPN работает

15. Nekoray позволяет скопировать конфиг, можно либо скопировать ссылку и вставить в другой клиент, либо отсканировать QR код с телефона во VLESS клиенте, например, V2BOX
    <img width="872" height="413" alt="image" src="https://github.com/user-attachments/assets/1aeb2d62-a2a9-4325-a758-267fb7293e66" />

Инструкция по настройке VK Cloud/ Yandex Cloud:

Все то же самое, но конфиг будет следующим.
Обращаю внимание, что мы указываем данные от подключения к иностранному сервера потому, что по факту vk/yandex будут выступать клиентом для него.

```
{
"log": {
"loglevel": "debug",

"access": "/var/log/xray/access.log",
 "error": "/var/log/xray/error.log"
 },
 "inbounds": [
 {
 "tag": "proxy",
 "port": 443,
 "protocol": "vless",
 "settings": {
 "clients": [
 {
 "id": "ВАШ UUID",
 "flow": "xtls-rprx-vision"
 }
 ],
 "decryption": "none"
 },
 "streamSettings": {
 "network": "tcp",
 "security": "reality",
 "realitySettings": {
 "dest": "www.bing.com:443",
 "serverNames": [
 "www.bing.com"
 ],

"privateKey": "ВАШ ПРИВАТНЫЙ КЛЮЧ",
"shortIds": [
 "aabbccdd"
 ]
 }
 },
"sniffing": {
 "enabled": true,
 "destOverride": [
 "http",
 "tls",
 "quic"
 ],
 "routeOnly": true
 }
 }
 ],
 "outbounds": [
  {
      "protocol": "vless",
      "settings": {
        "vnext": [
          {
            "address": "IP ИНОСТРАННОГО СЕРВЕРА",
            "port": 443,
            "users": [
              {
                "id": "UUID ИНОСТРАННОГО СЕРВЕРА",
                "encryption": "none",
                "flow": "xtls-rprx-vision"
              }
            ]
          }
        ]
      },
      "streamSettings": {
        "network": "tcp",
        "security": "reality",
        "realitySettings": {
          "serverName": "www.bing.com - ДОМЕН ИНОСТРАННОГО СЕРВЕРА",
          "fingerprint": "chrome",
          "publicKey": "ПУБЛИЧНЫЙ КЛЮЧ ИНОСТРАННОГО СЕРВЕРА",
          "shortId": "aabbccdd",
          "spiderX": ""
        }
      },
      "tag": "vless-out"
    }
 ]
 }

```
Как добавить несколько точек подключений?

Помним, что один uuid - это одна точка подключения, вы не можете сидеть с одного идентификатора с пк и с телефона, ровно так же, как и вы и ваш друг не сможете сидеть с двух пк по одному идентификатору одновременно

Допустим, нам нужно 3 точки подключения, генерируем 3 раза uuid командой xray uuid
Меняем конфиг в `nano /usr/local/etc/xray/config.json`
Меняем следующие значения:
```
"clients": [

{

"id": "ВАШ ID",

"flow": "xtls-rprx-vision"

}, // здесь запятая стоит
 {

"id": "ВАШ ID",

"flow": "xtls-rprx-vision"

}, // и тут стоит
 {

"id": "ВАШ ID",

"flow": "xtls-rprx-vision"

} // тут запятой нет, т.к. это последний элемент
```
После внесения изменений в конфиг обязательно перезагружаем xray командой 
```
systemctl restart xray
```
Смотрим 
```
systemctl status xray
```
Если Active: `active (running)`, значит все ок
