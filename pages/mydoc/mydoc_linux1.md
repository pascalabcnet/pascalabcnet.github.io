---
title: Установка PascalABC.NET под Linux
keywords: Linux
last_updated: 28.07.2026
sidebar: mydoc_sidebar
permalink: mydoc_linux1.html
toc: false
folder: mydoc
---

## Установка под Linux - общий алгоритм

0. [Скачиваем версию PascalABC.NET под Linux отсюда](https://pascalabc.net/downloads/PascalABCNETLinux.zip). Это .zip-архив.

1. Файлы архива содержат папки PascalABCNETLinux и Samples. Для определенности будем считать, что мы распаковали их в /home/user

2. Перед запуском следует установить пакет Mono. Процесс установки описан здесь: [Установка mono под Linux](https://www.mono-project.com/download/stable/#download-lin). 

Можно не читать официальный документ, а проделать следующее:

Выполнить команду

```
sudo apt-get install mono-complete
```

и затем возможно команду

```
sudo apt-get install mono-locale-extras
```

для установки пакета с кодировкой 1251.

Возможно, в некоторых версиях Linux пакет `mono-complete` будет отсутствовать, тогда вместо него следует установить пакет mono-devel:

```
sudo apt-get install mono-devel
```

Еще рекомендуем выполнить команду

```
sudo apt-get install gnome-themes-standard
```

3. Попробовать из папки PascalABCNETLinux запустить PascalABCNETLinux.exe. В некоторых версиях Linux расширение exe не ассоциировано с запуском mono, поэтому в консоли в этой папке следует выполнить команду

```
mono PascalABCNETLinux.exe
```

4. При запуске проиcходит ошибка, связанная с отсутствием папок

```
/etc/mono/registry/LocalMachine
```

В этом случае рекомендуется либо создать папки registry и registry/LocalMachine в /etc/mono/ вручную, либо выполнить команды

```
mkdir -p /home/user/PascalABCLinux/registry
mkdir -p /home/user/PascalABCLinux/registry/LocalMachine
export MONO_REGISTRY_PATH=/home/user/PascalABCLinux/registry
```

## Установка под WINE

[Установка под WINE описана здесь](https://pascalabc.miraheze.org/wiki/%D0%A3%D1%81%D1%82%D0%B0%D0%BD%D0%BE%D0%B2%D0%BA%D0%B0_%D0%B2_Linux)

## Alt Linux Образование 10.1

PascalABC.NET уже находится в репозитории Alt Linux Образование - устанавливаем его из репозитория


## Alt Linux общий алгоритм

Под Alt Linux установка пакета Mono выполняется по-другому:

    Переходим в режим администратора:

```
su-
```

    Обновляем пакеты:

```
apt-get update
```

    Инсталлируем mono-devel, выполняя команду в консоли:

```
apt-get install mono-devel
```

    Инсталлируем пакет с кодировкой 1251:

```
apt-get install mono-locale-extras
```

Не забываем выполнить

```
apt-get install gnome-themes-standard
```

4. Если при запуске проиcходит ошибка, связанная с отсутствием папок

```
/etc/mono/registry/LocalMachine
```

В этом случае рекомендуется либо создать папки `registry` и `registry/LocalMachine` в `/etc/mono/` вручную, либо выполнить команды

```
mkdir -p /home/user/PascalABCLinux/registry
mkdir -p /home/user/PascalABCLinux/registry/LocalMachine
export MONO_REGISTRY_PATH=/home/user/PascalABCLinux/registry
```

## Astra Linux

Под Astra Linux установка пакета Mono выполняется по-другому:

Обновляем пакеты:

```
sudo apt-get update
```

Инсталлируем mono-devel, выполняя команду в консоли:

```
sudo apt-get install mono-complete
```

Не забываем выполнить

```
sudo apt-get install gnome-themes-standard
```

4. При запуске проиcходит ошибка, связанная с отсутствием папок

```
/etc/mono/registry/LocalMachine
```

В этом случае рекомендуется либо

создать папки `registry` и `registry/LocalMachine` в `/etc/mono/` вручную, 

либо

выполнить команды

```
mkdir -p /home/user/PascalABCLinux/registry
mkdir -p /home/user/PascalABCLinux/registry/LocalMachine
export MONO_REGISTRY_PATH=/home/user/PascalABCLinux/registry
```
