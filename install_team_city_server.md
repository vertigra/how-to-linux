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
   
3. Добавляем скрипт автозапуска (меняем $user-name на существующий в системе)
   ```
   # joe gedit /etc/init.d/teamcity
   ```
   ```bash
   #!/bin/sh
   ### BEGIN INIT INFO
   # Provides:          teamcity
   # Required-Start:
   # Required-Stop:
   # Should-Start:
   # Should-Stop:
   # Default-Start:     2 3 4 5
   # Default-Stop:      0 1 6
   # Short-Description: team-city startup script
   ### END INIT INFO

    export TEAMCITY_DATA_PATH="/opt/TeamCity/bin/.BuildServer"
         
    case $1 in
    start)
    start-stop-daemon --start  -c $user-name --exec /opt/TeamCity/bin/runAll.sh start
    ;;
         
    stop)
    start-stop-daemon --start -c $user-name  --exec  /opt/TeamCity/bin/runAll.sh stop
    ;;
         
    esac
         
    exit 0
   ```
   
4. Меняем права и ставим в автозапуск
   ```bash
   # chmod +x /etc/init.d/teamcity
   # update-rc.d /etc/init.d/teamcity defaults
   ```
   
5. Запускаем демона
   ```bash
   # /etc/init.d/teamcity start
   ```
   
6. Удаляем архив
   ```bash
    rm /opt/TeamCity-10.0.2.tar.gz
   ```


