# На что можем влиять через adb (в поисках сервисов)

## В поисках сервисов (на что можем влиять через ADB)

## /system - раздел с системными приложениями (и бинарями)

Структура

```
/system
    /app - системные прилоежния (apk)
    /bin - бинари
    /data-app - еще какие-то apk
    /framework - какие-то jar-ники, связаны с /bin
    /lib* - либы
    /priv-app - системные приложения (apk)
    /rfs - что-то с firmware
    /vendor
        /app - приложения
        /bin - бинари
        ...
    /xbin - бинари
    /usr - ?
```

### /bin

ATFWD-daemon - форвард AT-команд\
DR\_AP\_Service - ?

#### app\_process

```
Usage: app_process [java-options] cmd-dir start-class-name [options]
ex: app_widget
base=/system
export CLASSPATH=$base/framework/appwidget.jar
exec app_process $base/bin com.android.commands.appwidget.AppWidget "$@"
```

#### applypatch

```
usage: %s [-b <bonus-file>] <src-file> <tgt-file> <tgt-sha1> <tgt-size> [<src-sha1>:<patch> ...]
   or  %s -c <file> [<sha1> ...]
   or  %s -s <bytes>
   or  %s -l
Filenames may be of the form
  MTD:<partition>:<len_1>:<sha1_1>:<len_2>:<sha1_2>:...
```

#### appops

Позволяет изменять поведение пакетов \
все равно что `cmd appops ...`

#### appwidget

```
base=/system
export CLASSPATH=$base/framework/appwidget.jar
exec app_process $base/bin com.android.commands.appwidget.AppWidget "$@"
```

#### athdiag

Связано что-то с чтением памяти процессы

#### audiod - ?

#### audioserver - ?

#### bcc

```
Это компилятор https://android.googlesource.com/platform/external/bcc/
bpf - на сколько понял, язык или фреймворк для написания чего-то под ядро. Есть какие-то способы через это мониторить траффик и еще что-то
Подробнее: https://source.android.com/devices/tech/datausage/ebpf-traffic-monitor
```

#### bdt

Позволяет включать/выключать bluetooth

Bluedroid Test Application

```
The test application provides a small console shell interface that allows
access to the Bluetooth HAL API library though ASCII commands. This is similar
to how the real JNI service would operate. The primary objective of this
application is to allow Bluetooth to be put in DUT Mode for RF/BB BQB test purposes.

This application is mutually exclusive with the Java based Bluetooth.apk. Hence
before launching the application, it should be ensured that the Settings->Bluetooth is OFF.

This application is built as 'bdt' and shall be available in '/system/bin/bdt'
```

Подробнее: [https://android.googlesource.com/platform/external/bluetooth/bluedroid/+/5738f83aeb59361a0a2eda2460113f6dc9194271/test/bluedroidtest/README.txt](https://android.googlesource.com/platform/external/bluetooth/bluedroid/+/5738f83aeb59361a0a2eda2460113f6dc9194271/test/bluedroidtest/README.txt)

#### blkid

узнать id разделов диска:

```
rolex:/system/bin # blkid
/dev/block/zram0: UUID="835b9f4d-d854-4830-9aee-158d7a6180cb" TYPE="swap"
/dev/block/mmcblk0p1: SEC_TYPE="msdos" UUID="00BC-614E" TYPE="vfat"
...
```

#### cmd

Default Service Manager&#x20;

```
`cmd -l` - список сервисов в системе (системных?)
`cmd <service>` - взаимодействовать с сервисом (не все поддерживают взаимодействие через shell)
```

#### bmgr

Backup Manager

#### bu

exec app\_process $base/bin com.android.commands.bu.Backup "$@"

#### bugreport\[z]

создания zip-ника с багом

#### cameraserver

Ну как бы очевидно, но что внутри - хз

#### clatd

Буд-то для форвардинга ipv6 трафика [https://android.googlesource.com/platform/external/android-clat/+/refs/heads/jb-mr1-dev-plus-aosp/clatd.c](https://android.googlesource.com/platform/external/android-clat/+/refs/heads/jb-mr1-dev-plus-aosp/clatd.c)

или ipv4 в ipv6: [https://dan.drown.org/android/clat/](https://dan.drown.org/android/clat/)

#### cnss-daemon, cnss\_diag

Сети через WiFi? или настройка WLAN?

```
usage: cnss-daemon [options]
   -n, --nodaemon  do not run as a daemon
   -d              show more debug messages (-dd for more)
   -l              Log output to logcat
       --help      display this help and exit
```

#### content

```
base=/system
export CLASSPATH=$base/framework/content.jar
exec app_process $base/bin com.android.commands.content.Content "$@"
```

Чтение файлов

```
usage: adb shell content read --uri <URI> [--user <USER_ID>]
  Example:
  # cat default ringtone to a file, then pull to host
  adb shell 'content read --uri content://settings/system/ringtone > /mnt/sdcard/tmp.ogg' && adb pull /mnt/sdcard/tmp.ogg
```

#### dnsmasq

DNS, DNCP, TFTP - сервер

#### dpm, dpmd

Device Policy Manager

```
base=/system
export CLASSPATH=$base/framework/dpm.jar
exec app_process $base/bin com.android.commands.dpm.Dpm "$@"
```

#### drmserver

#### cplay

Воспроизведение музыки [https://android.googlesource.com/platform/external/tinycompress/+/b48936b8eb31518652013230e8eb1eca9b9af218/cplay.c](https://android.googlesource.com/platform/external/tinycompress/+/b48936b8eb31518652013230e8eb1eca9b9af218/cplay.c)

#### curl :)

#### diag\_\*

[https://osmocom.org/projects/quectel-modems/wiki/Diag\_mdlog](https://osmocom.org/projects/quectel-modems/wiki/Diag\_mdlog)

#### cnd

Возможно как-то связано с CnE, а это связано с сетями мобильными [https://www.qualcomm.com/news/onq/2013/07/02/qualcomms-cne-bringing-smarts-3g4g-wi-fi-seamless-interworking](https://www.qualcomm.com/news/onq/2013/07/02/qualcomms-cne-bringing-smarts-3g4g-wi-fi-seamless-interworking)

#### bootanimation

Включает анимацию загрузки

#### bt\_logger

Логирование трафика Bluetooth

#### btnvtool

Что то с bluetooth

#### atrace

```
usage: %s [options] [categories...]
options include:
  -a appname      enable app-level tracing for a comma separated list of cmdlines
  -b N            use a trace buffer size of N KB
  -c              trace into a circular buffer
  -f filename     use the categories written in a file as space-separated
                    values in a line
  -k fname,...    trace the listed kernel functions
  -n              ignore signals
  -s N            sleep for N seconds before tracing [default 0]
  -t N            trace for N seconds [defualt 5]
  -z              compress the trace dump
  --async_start   start circular trace and return immediatly
  --async_dump    dump the current contents of circular trace buffer
  --async_stop    stop tracing and dump the current contents of circular
                    trace buffer
  --stream        stream trace to stdout as it enters the trace buffer
                    Note: this can take significant CPU time, and is best
                    used for measuring things that are not affected by
                    CPU performance, like pagecache usage.
  --list_categories
                  list the available tracing categories
 -o filename      write the trace to the specified file instead
                    of stdout.
```

## TODO: Не рассмотренные сервисы

```
-rwxr-xr-x  1 root      shell       67096 2008-12-31 19:00 drmserver
-rwxr-xr-x  1 root      shell      211232 2008-12-31 19:00 dumpstate
-rwxr-xr-x  1 root      shell       18632 2008-12-31 19:00 dumpsys
-rwxr-xr-x  1 root      shell       35088 2008-12-31 19:00 dun-server
-rwxr-xr-x  1 root      shell      204888 2008-12-31 19:00 e2fsck
-rwxr-xr-x  1 root      shell       10352 2008-12-31 19:00 e_loop
-rwxr-xr-x  1 root      shell        6168 2008-12-31 19:00 ebtables
-rwxr-xr-x  1 root      shell       34984 2008-12-31 19:00 exfatfsck
-rwxr-xr-x  1 root      shell       39080 2008-12-31 19:00 fdpp
-rwxr-xr-x  1 root      shell       63656 2008-12-31 19:00 fingerprintd
-rwxr-xr-x  1 root      shell       35016 2008-12-31 19:00 fm_qsoc_patches
-rwxr-xr-x  1 root      shell       18608 2008-12-31 19:00 fmconfig
-rwxr-xr-x  1 root      shell       10336 2008-12-31 19:00 fmfactorytest
-rwxr-xr-x  1 root      shell       22624 2008-12-31 19:00 fmfactorytestserver
-rwxr-xr-x  1 root      shell       14504 2008-12-31 19:00 fmhal_service
-rwxr-xr-x  1 root      shell       59544 2008-12-31 19:00 fsck.f2fs
-rwxr-xr-x  1 root      shell       34912 2008-12-31 19:00 fsck_msdos
-rwxr-xr-x  1 root      shell      117056 2008-12-31 19:00 fstman
-rwxr-xr-x  1 root      shell       96632 2008-12-31 19:00 ftmdaemon
-rwxr-xr-x  1 root      shell       43576 2008-12-31 19:00 garden_app
-rwxr-xr-x  1 root      shell       63656 2008-12-31 19:00 gatekeeperd
-rwxr-xr-x  1 root      shell       55608 2008-12-31 19:00 gpsone_daemon
-rwxr-xr-x  1 root      shell       22284 2008-12-31 19:00 gptest
-rwxr-xr-x  1 root      shell       28336 2008-12-31 19:00 grep
-rwxr-xr-x  1 root      shell       10336 2008-12-31 19:00 gzip
-rwxr-xr-x  1 root      shell      171072 2008-12-31 19:00 hal_proxy_daemon
-rwxr-xr-x  1 root      shell      142576 2008-12-31 19:00 hci_qcomm_init
-rwxr-xr-x  1 root      shell         213 2008-12-31 19:00 hid
-rwxr-xr-x  1 root      shell      667440 2008-12-31 19:00 hostapd
-rwxr-xr-x  1 root      shell       47296 2008-12-31 19:00 hostapd_cli
-rwxr-xr-x  1 root      shell      839536 2008-12-31 19:00 hs20-osu-client
-rwxr-xr-x  1 root      shell       35008 2008-12-31 19:00 hvdcp_opti
-rwxr-xr-x  1 root      shell       34920 2008-12-31 19:00 idmap
-rwxr-xr-x  1 root      shell         194 2008-12-31 19:00 ime
-rwxr-xr-x  1 system    radio      137576 2008-12-31 19:00 ims_rtp_daemon
-rwxr-xr-x  1 root      shell        6272 2008-12-31 19:00 imscmservice
-rwxr-xr-x  1 system    system     174456 2008-12-31 19:00 imsdatadaemon
-rwxr-xr-x  1 root      shell      129416 2008-12-31 19:00 imsqmidaemon
-rwxr-xr-x  1 root      shell         203 2008-12-31 19:00 input
-rwxr-x---  1 root      root          622 2008-12-31 19:00 install-recovery.sh
-rwxr-xr-x  1 root      shell      158512 2008-12-31 19:00 installd
-rwxr-xr-x  1 root      shell        6096 2008-12-31 19:00 iop
-rwxr-xr-x  1 root      shell      256888 2008-12-31 19:00 ip
-rwxr-xr-x  1 root      shell      382440 2008-12-31 19:00 ip6tables
-rwxr-xr-x  1 root      shell      330384 2008-12-31 19:00 ipacm
-rwxr-xr-x  1 root      shell       10408 2008-12-31 19:00 ipacm-diag
-rwxr-xr-x  1 root      shell      373936 2008-12-31 19:00 iptables
-rwxr-xr-x  1 root      shell       10264 2008-12-31 19:00 irsc_util
-rwxr-xr-x  1 root      shell       30144 2008-12-31 19:00 iwlist
-rwxr-xr-x  1 root      shell      125280 2008-12-31 19:00 keystore
-rwxr-xr-x  1 root      shell      826024 2008-12-31 19:00 ld.mc
-rwxr-xr-x  1 root      shell      630808 2008-12-31 19:00 linker
-rwxr-xr-x  1 root      shell      931096 2008-12-31 19:00 linker64
-rwxr-xr-x  1 root      shell       18536 2008-12-31 19:00 lmkd
-rwxr-xr-x  1 root      shell       84208 2008-12-31 19:00 loc_launcher
-rwxr-xr-x  1 root      shell       30912 2008-12-31 19:00 logcat
-rwxr-xr-x  1 root      shell      100536 2008-12-31 19:00 logd
-rwxr-xr-x  1 root      shell       31216 2008-12-31 19:00 logwrapper
-rwxr-xr-x  1 root      shell      573736 2008-12-31 19:00 lowi-server
-rwxr-xr-x  1 root      shell       10344 2008-12-31 19:00 make_ext4fs
-rwxr-xr-x  1 root      shell       22840 2008-12-31 19:00 make_f2fs
-rwxr-xr-x  1 root      shell       43120 2008-12-31 19:00 mcd
-rwxr-xr-x  1 root      shell      774552 2008-12-31 19:00 mdnsd
-rwxr-xr-x  1 root      shell         210 2008-12-31 19:00 media
-rwxr-xr-x  1 root      shell       17944 2008-12-31 19:00 mediacodec
-rwxr-xr-x  1 root      shell       26136 2008-12-31 19:00 mediadrmserver
-rwxr-xr-x  1 root      shell       17944 2008-12-31 19:00 mediaextractor
-rwxr-xr-x  1 root      shell       17944 2008-12-31 19:00 mediaserver
-rwxr-xr-x  1 root      shell       14432 2008-12-31 19:00 mitop
-rwxr-xr-x  1 root      shell       28592 2008-12-31 19:00 mkexfatfs
-rwxr-xr-x  1 root      shell      109980 2008-12-31 19:00 mm-audio-ftm
-rwxr-xr-x  1 root      shell       87816 2008-12-31 19:00 mm-qcamera-app
-rwxr-xr-x  1 root      shell       30372 2008-12-31 19:00 mm-qcamera-daemon
-rwxr-xr-x  1 root      shell       22656 2008-12-31 19:00 mm-swvdec-test
-rwxr-xr-x  1 root      shell       34944 2008-12-31 19:00 mm-swvenc-test
-rwxr-xr-x  1 root      shell      339440 2008-12-31 19:00 mm-vidc-omx-test
-rwxr-xr-x  1 root      shell      224528 2008-12-31 19:00 mmi
-rwxr-xr-x  1 root      shell       26184 2008-12-31 19:00 mmi_agent32
-rwxr-xr-x  1 root      shell       26664 2008-12-31 19:00 mmi_agent64
-rwxr-xr-x  1 root      shell       10336 2008-12-31 19:00 mmi_debug
-rwxr-xr-x  1 root      shell       47280 2008-12-31 19:00 mmi_diag
-rwxr-xr-x  1 root      shell         217 2008-12-31 19:00 monkey
-rwxr-xr-x  1 root      shell       26824 2008-12-31 19:00 msm_irqbalance
-rwxr-xr-x  1 root      shell       10264 2008-12-31 19:00 mtd
-rwxr-xr-x  1 root      shell       22816 2008-12-31 19:00 mtpd
-rwxr-xr-x  1 root      shell       10264 2008-12-31 19:00 ndc
-rwxr-xr-x  1 root      shell      375400 2008-12-31 19:00 netd
-rwxr-xr-x  1 root      shell     1430952 2008-12-31 19:00 netmgrd
-rwxr-xr-x  1 root      shell        6240 2008-12-31 19:00 nl_listener
-rwxr-xr-x  1 root      shell      198888 2008-12-31 19:00 oatdump
-rwxr-xr-x  1 root      shell       71188 2008-12-31 19:00 patchoat
-rwxr-xr-x  1 root      shell       39264 2008-12-31 19:00 ping
-rwxr-xr-x  1 root      shell       43776 2008-12-31 19:00 ping6
-rwxr-xr-x  1 root      shell       14360 2008-12-31 19:00 pktlogconf
-rwxr-xr-x  1 root      shell         191 2008-12-31 19:00 pm
-rwxr-xr-x  1 root      shell        6336 2008-12-31 19:00 pm-proxy
-rwxr-xr-x  1 system    system      52272 2008-12-31 19:00 pm-service
-rwxr-xr-x  1 root      shell       30832 2008-12-31 19:00 port-bridge
-rwxr-xr-x  1 root      shell      259600 2008-12-31 19:00 pppd
-rwxr-xr-x  1 root      shell       34384 2008-12-31 19:00 profman
-rwxr-xr-x  1 root      shell       30896 2008-12-31 19:00 ptt_socket_app
drwxr-xr-x  2 root      shell        4096 2008-12-31 19:00 qmi-framework-tests
-rwxr-xr-x  1 root      shell      133880 2008-12-31 19:00 qmi_simple_ril_test
-rwxr-xr-x  1 root      shell       47760 2008-12-31 19:00 qseecom_sample_client
-rwxr-xr-x  1 root      shell       10768 2008-12-31 19:00 qseecomd
-rwxr-xr-x  1 root      shell      271224 2008-12-31 19:00 racoon
-rwxr-xr-x  1 root      shell       47304 2008-12-31 19:00 radish
-rwxr-xr-x  1 root      shell        6168 2008-12-31 19:00 reboot
-rwxr-xr-x  1 root      shell         188 2008-12-31 19:00 requestsync
-rwxr-xr-x  1 root      shell       43176 2008-12-31 19:00 resize2fs
-rwxr-xr-x  1 root      shell       10264 2008-12-31 19:00 resize_ext4
-rwxr-xr-x  1 root      shell       14464 2008-12-31 19:00 rild
-rwxr-xr-x  1 root      shell       17160 2008-12-31 19:00 rmnetcli
-rwxr-xr-x  1 root      shell       27696 2008-12-31 19:00 rmt_storage
-rwxr-x---  1 root      shell       14360 2008-12-31 19:00 run-as
-rwxr-xr-x  1 root      shell        6168 2008-12-31 19:00 schedtest
-rwxr-xr-x  1 root      shell       14440 2008-12-31 19:00 screencap
-rwxr-xr-x  1 root      shell      112880 2008-12-31 19:00 screenrecord
-rwxr-xr-x  1 root      shell       30744 2008-12-31 19:00 sdcard
-rwxr-xr-x  1 root      shell       18456 2008-12-31 19:00 secdiscard
-rwxr-xr-x  1 root      shell       22632 2008-12-31 19:00 secure_ui_sample_client
-rwxr-xr-x  1 root      shell      158520 2008-12-31 19:00 sensors.qcom
-rwxr-xr-x  1 root      shell       10264 2008-12-31 19:00 sensorservice
-rwxr-xr-x  1 root      shell       18536 2008-12-31 19:00 service
-rwxr-xr-x  1 root      shell       18576 2008-12-31 19:00 servicemanager
-rwxr-xr-x  1 root      shell         178 2008-12-31 19:00 settings
-rwxr-xr-x  1 root      shell       10272 2008-12-31 19:00 setup_fs
-rwxr-xr-x  1 root      shell      166072 2008-12-31 19:00 sgdisk
-rwxr-xr-x  1 root      shell      285488 2008-12-31 19:00 sh
-rwxr-xr-x  1 root      shell      360240 2008-12-31 19:00 sigma_dut
-rwxr-xr-x  1 root      shell         190 2008-12-31 19:00 sm
-rwxr-xr-x  1 root      shell       10264 2008-12-31 19:00 soter_client
-rwxr-xr-x  1 root      shell       10408 2008-12-31 19:00 ssr_diag
-rwxr-xr-x  1 root      shell       10336 2008-12-31 19:00 ssr_setup
-rwxr-xr-x  1 root      shell       26736 2008-12-31 19:00 subsystem_ramdump
-rwxr-xr-x  1 system    graphics    14432 2008-12-31 19:00 surfaceflinger
-rwxr-xr-x  1 root      shell         192 2008-12-31 19:00 svc
-rwxr-xr-x  1 root      shell        6168 2008-12-31 19:00 tbaseLoader
-rwxr-xr-x  1 root      shell      101160 2008-12-31 19:00 tc
-rwxr-xr-x  1 root      shell         172 2008-12-31 19:00 telecom
-rwxr-xr-x  1 root      shell       18600 2008-12-31 19:00 test_diag
-rwxr-xr-x  1 root      shell       26136 2008-12-31 19:00 test_module_pproc
-rwxr-xr-x  1 root      shell       84568 2008-12-31 19:00 tftp_server
-rwxr-xr-x  1 root      shell       26808 2008-12-31 19:00 time_daemon
-rwxr-xr-x  1 root      shell       10272 2008-12-31 19:00 tinycap
-rwxr-xr-x  1 root      shell       14360 2008-12-31 19:00 tinymix
-rwxr-xr-x  1 root      shell       10344 2008-12-31 19:00 tinypcminfo
-rwxr-xr-x  1 root      shell       10336 2008-12-31 19:00 tinyplay
-rwxr-xr-x  1 root      shell       43136 2008-12-31 19:00 tune2fs
-rwxr-xr-x  1 root      shell       18456 2008-12-31 19:00 tzdatacheck
-rwxr-xr-x  1 root      shell        4156 2008-12-31 19:00 uiautomator
-rwxr-x---  1 root      root        72888 2008-12-31 19:00 uncrypt
-rwxr-xr-x  1 root      shell       10264 2008-12-31 19:00 vdc
-rwxr-xr-x  1 root      shell       26824 2008-12-31 19:00 vendor_cmd_tool
-rwxr-xr-x  1 root      shell      588760 2008-12-31 19:00 vold
-rwxr-xr-x  1 root      shell       13848 2008-12-31 19:00 vsimd
-rwxr-xr-x  1 bluetooth bluetooth  104864 2008-12-31 19:00 wcnss_filter
-rwxr-xr-x  1 root      shell       18528 2008-12-31 19:00 wcnss_service
-rwxr-xr-x  1 root      shell       18528 2008-12-31 19:00 wdsdaemon
-rwxr-xr-x  1 root      shell       13848 2008-12-31 19:00 wfdservice
-rwxr-xr-x  1 root      shell         190 2008-12-31 19:00 wm
-rwxr-xr-x  1 root      shell     1939512 2008-12-31 19:00 wpa_supplicant
-rwxr-xr-x  1 root      shell       10392 2008-12-31 19:00 wt_tee_check
-rwxr-xr-x  1 root      shell      981600 2008-12-31 19:00 xtwifi-client
-rwxr-xr-x  1 root      shell      100688 2008-12-31 19:00 xtwifi-inet-agent
-rwxr-xr-x  1 root      shell       59664 2008-12-31 19:00 zip_utils
rolex:/system/bin #
```

#### am ActivityManager

`am stack list` - список запущенных активити. Выводятся в порядке наложения друг на друга (первым выводится та, которая сверху). Есть метки свернута или нет активити. `am stack info <stack id>` - инфа об отдельной активити `am stack positiontask <TASK_ID> <STACK_ID> <POSITION>` - поменять местами активити `am dumpheap [PID] [file name]` - только для debuggable приложений `am startservice` `am stopservice` `am broadcast` `am instrument` - старт Instrumentation - отслежитвание взаимодействия с системой. Скорее всего в приложении должно быть разрешение какое `am start` `am trace-ipc start / stop --dump-file` - опять же для debug-приложений
