+++
title = 'Я скомпилировал ОС, в которой есть только тетрис'
date = 2024-04-16T16:58:48+03:00
draft = false
+++

![](/images/1.png)

Недавно на просторах ютуба я нашёл [видео](https://www.youtube.com/watch?v=FaILnmUYS_U), где разработчик создавал Tetris OS. Исходный код он [выложил](https://github.com/jdah/tetris-os) на гитхаб, но репозиторий с ним заблокировали. Сейчас его можно скачать только из [Wayback Machine](https://web.archive.org/web/20210423124508if_/https://codeload.github.com/jdah/tetris-os/zip/refs/heads/master). Для того, чтобы скомпилировать нужно установить некоторые пакеты (для Ubuntu/Debian):

```
$ sudo apt install gcc gcc-multilib qemu-system-x86
```

Дальше переходим в папку с кодом и там выполняем:

```
$ make iso
$ qemu-system-i386 -drive format=raw,file=boot.iso -d cpu_reset -monitor stdio -device sb16 -audiodev pulseaudio,id=pulseaudio,out.frequency=48000,out.channels=2,out.format=s32
```

В моём случае при выполнении команды возникли проблемы с аудио. Для того, чтобы это исправить нужно открыть файл src/main.c и убрать эту строчку:

```
#define ENABLE_MUSIC
```

Сохраняем, снова выполняем `make iso` и вместо последнией команды пишем:

```
$ qemu-system-i386 -drive format=raw,file=boot.iso 
```

Есть одно "но": звука в игре не будет.
Вот и всё!

![](/images/2.png)

Также эту систему можно запустить на реальном компьютере, а не в эмуляторе, как мы это сделали. Для этого можно записать файл boot.iso на флешку и запустить на старом компьютере без UEFI или в режиме Legacy BIOS.