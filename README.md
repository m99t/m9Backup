#!/bin/bash
clear
##-----------------------------------------------------------------------------------------------------#
#|                                                                                                     |
#|               __________________                   .___             __  .__                         |
#|         _____/   __   \______   \_______  ____   __| _/_ __   _____/  |_|__| ____   ____            |
#|        /     \____    /|     ___/\_  __ \/  _ \ / __ |  |  \_/ ___\   __\  |/  _ \ /    \           |
#|	 |  Y Y  \ /    / |    |     |  | \(  <_> ) /_/ |  |  /\  \___|  | |  (  <_> )   |  \          |
#|	 |__|_|  //____/  |____|     |__|   \____/\____ |____/  \___  >__| |__|\____/|___|  /          |
#|             \/                                      \/           \/                    \/           |
#|                                                                                                     |
##-----------------------------------------------------------------------------------------------------#
#
#                [ A simple backup script that will tar archive directory(s) and move them to a dir ]
#
#                                Title   [ bacup.sh                   ]
#                                Version [ 1.0.2                      ]
#                                Date    [ 07 - May - 2015            ]
#                                Authors [ Sean Murphy,               ]

#------------------
#Variables
date=`date +%Y-%m-%d`
alldb="--all-databases"
backupfldr="/backups"
sqluser=""
sqlpass=""

#------------------
#Functions

function backupdir {
	echo -ne "Backing up: /$1\t\t\t[..]\r"
	backupfile=$backupfldr/$2/$date.tar.gz
	tar -zcf $backupfile -C / $1
	if [ -f $backupfile ]; then
		echo -ne "Backing up: /$1\t\t\t[OK]\r"
	else
		echo -ne "Backing up: /$1\t\t\t[FAIL]\r"
	fi
	echo -ne '\n'
}

function backupfile {
	echo -ne "Backing up: /$1\t\t[..]\r"
        backupfile=$backupfldr/$2/$date.tar.gz
        tar -zcf $backupfile -C / $1
        if [ -f $backupfile ]; then
                echo -ne "Backing up: /$1\t\t[OK]\r"
        else
                echo -ne "Backing up: /$1\t\t[FAIL]\r"
	fi
        echo -ne '\n'
}

function backupsql {
	echo -ne "Backing up SQL DB(s): $1\t\t\t[..]\r"
	backupfile=$backupfldr/$2/$date.sql
	mysqldump -u $sqluser -p$sqlpass --events --ignore-table=mysql.events $1 > $backupfile
	if [ -f $backupfile ]; then
		echo -ne "Backing up SQL DB(s): $1\t\t\t[OK]\r"
	else
		echo -ne "Backing up SQL DB(s): $1\t\t\t[FAIL]\r"
	fi
	echo -ne '\n'
}

#----------------
#Main Method

echo """
            ___________            _                
           |  _  | ___ \          | |               
 _ __ ___  | |_| | |_/ / __ _  ___| | ___   _ _ __  
| '_   _ \ \____ | ___ \/ _  |/ __| |/ / | | | '_ \ 
| | | | | |.___/ / |_/ / (_| | (__|   <| |_| | |_) |
|_| |_| |_|\____/\____/ \__,_|\___|_|\_\___,_| .__/ 
                                             | |    
                                             |_|    
"""
echo -e "\n"

#------------
#Syntax examples

: '
backupsql {Database Name} {Folder relitive to selected directory}
backupdir {Directory} {Folder}
backupfile {File} {Folder}
'
