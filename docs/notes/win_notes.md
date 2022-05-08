---
tags:
  - windows
  - python
  - git
---

# Windows Notes

## Add Python to PATH

- Press `Win + r`.
- Type `sysdm.cpl`.
- Go to `Advanced - Environment variables` (`Дополнительно - Переменные среды`).
- Python application path should look loke this:
```
C:\Users\usr_name\AppData\Local\Programs\Python\PythonXX
```

## Switch GIT user 

To switch git user on Windows go to credentials management tab:

- `Control panel - User accounts - Credential manager`
- `Панель управления - Учетные записи пользователей - Диспетчер учетных данных`

## Delete Windows.old

Чтобы удалить предыдущую версию Windows с экрана загурзки необходимо выполнить следующие действия:

- запустить консоль от имени администратора
- `bcdedit`
- `bcdedit /delete {win_id} /f`

## X-box 360 wireless reciver driver

- [Official guide for win 10](https://support.xbox.com/en-US/help/xbox-360/xbox-on-windows/accessories/xbox-controller-for-windows-setup?tag=makemoney0821-20)
- [Интсрукция на яндекс.дзен](https://zen.yandex.ru/media/origamer/ne-rabotaet-priemnik-besprovodnogo-geimpada-xbox-360-v-2004-obnovlenii-windows-10-poshagovaia-instrukciia-podkliucheniia-5f722c38315c38725c05aab8)
- [Старая инструкция](http://cityacee.blogspot.com/2012/08/xbox-360.html)