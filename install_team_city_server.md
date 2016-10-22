# Как установить TeamCity Server 10.0.2
*OC: Debian 8 Jessie*

Хорошо написано [здесь](http://maxim.rubchinsky.com/install-teamcity-ubuntu/).

1. [Ставим Java](https://linux.nesterof.com/install_java_8_ppa.html)
2. Скачиваем TeamCity 
   ```bash
   # cd /opt
   # wget https://download.jetbrains.com/teamcity/TeamCity-10.0.2.tar.gz
   # tar -xvf TeamCity-10.0.2.tar.gz
   # chown -R $user-name TeamCity
   ```
3. Добавляем скрипт автозапуска
   ```bash
   # joe gedit /etc/init.d/teamcity
   
   #!/bin/sh
    # /etc/init.d/teamcity -  startup script for teamcity
    export TEAMCITY_DATA_PATH="/opt/JetBrains/TeamCity/.BuildServer"
         
    case $1 in
    start)
    start-stop-daemon --start  -c $user --exec /opt/JetBrains/TeamCity/bin/runAll.sh start
    ;;
         
    stop)
    start-stop-daemon --start -c $user  --exec  /opt/JetBrains/TeamCity/bin/runAll.sh stop
    ;;
         
    esac
         
    exit 0
   ```



