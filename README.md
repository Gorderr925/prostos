# ProstOS

![версия](https://img.shields.io/badge/версия-2.0-22c55e)
![основа](https://img.shields.io/badge/основа-Debian%2013%20Trixie-a80030)
![рабочий стол](https://img.shields.io/badge/рабочий%20стол-XFCE%204.20-0ea5e9)
![лицензия](https://img.shields.io/badge/лицензия-MIT-64748b)

Лёгкий и дружелюбный дистрибутив Linux для тех, кто только знакомится с
открытым кодом. Собран на **Debian 13 (Trixie)** с рабочим столом **XFCE** и
графическим установщиком **Calamares**.

> Просто. Открыто. Твоё.

## Скачать

Готовые образы лежат в разделе [**Releases**](https://github.com/Gorderr925/prostos/releases) —
по одному релизу на каждую версию:

| Версия | Что это | Файл |
|---|---|---|
| [ProstOS 2.0](https://github.com/Gorderr925/prostos/releases/tag/v2.0) | актуальная: новые обои, окно знакомства, прошивки NVIDIA | `ProstOS-2.0.iso` |
| [ProstOS 1.0](https://github.com/Gorderr925/prostos/releases/tag/v1.0) | первый выпуск, оставлен как история проекта | `ProstOS-1.0.iso` |

Образ весит около 3 ГБ, поэтому в релизе он лежит частями `prostos.iso.part00`,
`part01`, … Склеить в Windows: `copy /b prostos.iso.part* prostos.iso`.

## Что внутри

- Debian 13 amd64, ядро с firmware для Wi-Fi и видео;
- XFCE 4.20, LightDM, тема Arc, иконки Papirus;
- русский язык, раскладка RU/EN, часовой пояс Europe/Moscow;
- Firefox ESR, LibreOffice (Writer/Calc/Impress), VLC, GParted, Synaptic;
- Calamares — установка в пару кликов;
- приветственная страница о системе и open source;
- поддержка persistence на флешке (сохранение изменений между загрузками).

## Сборка в облаке (GitHub Actions)

1. Создайте публичный репозиторий на GitHub (например, `prostos`).
2. Загрузите в него содержимое этой папки (ветка `main`).
3. Откройте **Actions → Build ProstOS ISO → Run workflow**.
4. Через 20–60 минут образ появится в разделе **Releases** (тег вида `v2.0`),
   а также в артефакте `prostos-iso`.

Номер версии задаётся файлом `VERSION` в корне репозитория, а описание релиза
берётся из раздела этой версии в `CHANGELOG.md`. Чтобы выпустить 3.0, достаточно
поменять `VERSION` на `3.0` и дописать раздел в `CHANGELOG.md`.

Сборка идёт в контейнере `debian:trixie`, на вашем ПК ничего устанавливать не нужно.
Если ISO получился больше 1,9 ГБ, он автоматически делится на части
`prostos.iso.part00`, `part01`, … — склеить их можно так (Windows):
`copy /b prostos.iso.part* prostos.iso`.

## Локальная сборка (Linux)

Нужны `live-build`, `debootstrap`, `xorriso`, `squashfs-tools`, `dosfstools`,
`mtools`, `grub-pc-bin`, `grub-efi-amd64-bin`, `isolinux`, `syslinux-common`.

```bash
sudo apt update
sudo apt install -y live-build debootstrap xorriso squashfs-tools dosfstools \
  mtools grub-pc-bin grub-efi-amd64-bin isolinux syslinux-common syslinux-utils \
  wget xz-utils rsync file bzip2 cryptsetup ca-certificates fakeroot bc

cd live
sudo lb build
```

Готовый образ: `live/prostos-amd64.hybrid.iso`.

## Запись на флешку

Рекомендую **Ventoy** — не нужно «прожигать» образ:

1. Установите Ventoy на флешку (8 ГБ и больше).
2. Скопируйте ISO в раздел Ventoy.
3. Загрузитесь с флешки и выберите ProstOS в меню Ventoy.

Альтернатива — **Rufus** (режим «ISO Image», GPT/UEFI или MBR/BIOS).

> Перед установкой на реальный диск проверьте образ в VirtualBox/VMware/QEMU.

## Persistence

В параметрах загрузки уже есть `persistence`. Создайте на флешке раздел с меткой
`persistence`, а в нём файл `persistence.conf`:

```
/home
```

Тогда изменения в `/home` будут сохраняться между загрузками. В Ventoy
используйте плагин Persistence и файл `prostos.dat`.

## Структура

```
live/auto/config                     параметры live-build
live/config/preseed/                 ответы debconf (локаль, клавиатура, grub)
live/config/package-lists/           список пакетов
live/config/hooks/normal/            хуки: локаль, время, автологин, чистка
live/config/includes.chroot/         файлы, которые попадут в ISO
  etc/                               брендинг, LightDM, GTK, XFCE
  usr/share/prostos/                 логотип, обои, приветственная страница
  usr/share/calamares/branding/      оформление установщика
VERSION                              номер текущей версии (2.0)
CHANGELOG.md                         описание изменений каждой версии
.github/workflows/build.yml          сборка ISO в GitHub Actions
```

## Переменные live-сессии

| Что | Значение |
|---|---|
| Пользователь | `prost` (автологин) |
| Пароль live-пользователя | `live` |
| Часовой пояс | Europe/Moscow |
| Язык | ru_RU.UTF-8 |
| Раскладки | RU/EN, переключение Alt+Shift |

## Лицензия

Конфигурация и брендинг — MIT. Пакеты внутри ISO распространяются под своими
лицензиями (в основном GPL/LGPL). Debian — свободная система, XFCE — GPL.
