# 本仓库解决在中国大陆使用teslaandroid 项目遇到的问题

注意:使用了adb remount后将无法进行 ota 升级 teslaandroid 程序
使用 teslaandroid 项目的时候会遇到以下问题：

1.在使用最新版 gps 功能的时候，会出现 gps 定位不准的情况，实际是由于坐标系的转换导致

2.在车机升级某个版本之后，出现了 tesla 车机无法搜索到 wifi 的情况，经过排查，判断为 wifi 信道导致

解决办法：

GPS：

下载三个文件：

android.html

gcoord.global.prod.js

require.js

将树莓派连接到电脑

    adb root
    adb remount vendor
    adb push android.html /sdcard
    adb push gcoord.global.prod.js /sdcard
    adb push require.js /sdcard
    adb shell 
    cd /vendor/tesla-android/lighttpd/www-default
    mv android.html android.html.bak
    cp /sdcard/android.html ./
    cp /sdcard/gcoord.global.prod.js ./
    cp /sdcard/require.js ./
    reboot

wifi:

    adb root
    adb remount vendor
    adb shell
    cp /vendor/build.prod /vendor/build.prod.bak
    echo "persist.tesla-android.softap.channel= 149" >> /vendor/build.prod
    echo  "ro.boot.wificountrycode=CN" >> /vendor/build.prod
    reboot

