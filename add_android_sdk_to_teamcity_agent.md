# Как установить Android SDK на TeamCity агент

> OC: Debian 8 Jessie

1. Скачать и распаковать свежий sdkmanager

 ```bash
 # cd /opt
 # wget https://dl.google.com /android/repository/tools_r25.2.3-linux.zip
 # unzip tools_r25.2.3-linux.zip
 # mv tools android-sdk-linux
 ```

1. Установить API, build-tools, platform-tools и прочее. 

 ```bash
 # cd android-sdk-linux/tools
 # ./android update sdk --no-ui
 
 ```
 Если со свободным местом на жестком диске проблема можно ставить выборочно под проект. Для этого нужно сначала вывести весь список доступных пакетов командой `./android list sdk -u -a` и затем установить нужные `android update sdk --no-ui --filter 3,5,8,14` (например 3, 5, 8, 14)
 
1. Прописать переменную ANDROD_HOME в скрипт автозапуска TeamCity (почему-то при указани в ~/.bashrc TeamCity ее не видит)

 ```bash
 # joe /etc/init.d/teamcity
 (add this)
 
 export ANDROID_HOME="/opt/android-sdk-linux"
 ```
 
1. `# service teamcity restart`





