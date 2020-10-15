# Установка ПО для аппаратного ускорения

Перед уставновкой **КРАЙНЕ** рекомендуется вычистить ПК от ffmpeg и библиотек libva* .

## 1. Установка драйвера mfx

```
sudo apt-get install libmfx1 libmfx-tools
```
p.s. возможно также нужны следующие драйвера

```
sudo apt-get install libva-drm2 libva-x11-2 libva-wayland2 libva-glx2
```
## 2. Установка IntelMediaSDK

Для Ubuntu версией 19.04 и старше:

```
sudo apt-get install libva-dev libmfx-dev intel-media-va-driver-non-free
export LIBVA_DRIVER_NAME=iHD

```
Дополнительно для сборки ffplay:

```
sudo apt-get install libsdl2-dev
```

P.S. Для более старых версий системы:

GOOD LUCK!

Сборку можно осуществить отсюда
https://github.com/Intel-Media-SDK/MediaSDK/wiki/Build-Media-SDK-on-Ubuntu

Не забудьте настроить среду
```
export LIBVA_DRIVERS_PATH=/path/to/iHD_driver
export LIBVA_DRIVER_NAME=iHD
export LD_LIBRARY_PATH=/path/to/msdk/lib
export PKG_CONFIG_PATH=/path/to/msdk/lib/pkgconfig
```
## 3. Ручная сборка FFMPEG

Со страницы https://ffmpeg.org/ скачать одну из последних версий sourse-файлов ffmpeg.
Заходим в папку ffmpeg-version_name и конфигурируем сборку

```
./configure --arch=x86_64 --disable-yasm --enable-vaapi --enable-libmfx --enable-nonfree --enable-shared
```
Далее осуществляем саму сборку
```
sudo make
```
Занимает несколько минут. Далее установку

```
sudo make install
```
## 4. Проверка установки

Нужен файл с видео в фомате h264(далее input.mp4):

```
sudo ffmpeg -hwaccel qsv -c:v h264_qsv -i input.mp4 -c:v mjpeg_qsv output.mp4
```
При ошибках попробовать также команду:
```
sudo ffmpeg -init_hw_device qsv=hw -hwaccel qsv -c:v h264_qsv -i input.mp4 -c:v mjpeg_qsv output.mp4
```
При отсутсвии ошибок и номально воспроизведении output.mp4 можно переходить к запуску программ, использующих API.
