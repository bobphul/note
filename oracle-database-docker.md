# Oracle Database container

# Test Environment
1. Windows 10
2. ~~WSL(Windows Subsystem for Linux) - **Ubuntu 18.04 LTS**  
Install : https://docs.microsoft.com/ko-kr/windows/wsl/install-win10~~
3. Docker Desktop for Windows (recommand : stable)  
Install : https://hub.docker.com/editions/community/docker-ce-desktop-windows

**※ ~~WSL,~~ Docker 설치는 상기 링크로 대체한다.**  
**※ Windows10 + Hyper-V에 Linux VM을 구성후 docker 환경을 사용해도 상관 없다.**  
**※ WSL는 굳이 사용하지 않는다.**

# ~~WSL~~
> ~~WSL1는 Windows와 Linux 커널 시스템 호출 간의 변환해줄 뿐, 아직 Docker에서 필요한 kernel 수준의 작업을 수행할수 없는 상태이다.  
> WSL2 beta가 오픈되었으나, 아직 안정화 단계는 아닌것으로 확인된다.~~  

~~Install 이후 초기화까지 마친 상태라고 가정한다.  
이전에 docker 관련 패키지가 설치되어 있다면 먼저 제거한다.~~
```
sudo apt-get remove docker docker-engine docker.io
```

~~패키지 속도 다운로드 및 원활한 설치를 위해 repo site를 변경한다.~~
```
sudo sed -i -re 's/([a-z]{2}.)?archive.ubuntu.com|security.ubuntu.com/mirror.kakao.com/g' /etc/apt/sources.list
```

~~apt 패키지 색인을 업데이트 한다.~~
```
sudo apt-get update
```

~~HTTP를 이용한 kakao mirror, docker 저장소를 사용할 수 있도록 패키지를 설치한다.~~
```
sudo apt-get install apt-transport-https ca-certificates curl software-properties-common
```

~~Docker 공식 GPG 키 추가~~
```
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -
```

~~Fingerprint 확인~~
```
sudo apt-key fingerprint 0EBFCD88
```

~~Docker repository 추가~~
```
sudo add-apt-repository "deb [arch=amd64] https://download.docker.com/linux/ubuntu $(lsb_releas
e -cs) stable"
```

~~apt 패키지 색인을 업데이트 한다.~~
```
sudo apt-get update
```

~~Docker-ce 패키지 설치~~
```
sudo apt-get install docker-ce
```

~~Docker 시작 확인~~
```
sudo service docker start
 * Starting Docker: docker                                 [ OK ]
sudo service docker status
 * Docker is not running
```
  
***
# Oralce Database Container 구성
> SingleInstance 기준이며, Github Guide 상에는 RAC도 포함되어져 있긴하다.

## 구성 가능한 Database Version & Editions
[Oracle Github](https://github.com/oracle/docker-images/tree/master/OracleDatabase/SingleInstance**)에 oracle database를 구성할수 있는 Dockerfile과 방법을 가이드하고 있다.  
참고로 제공되는 Dockerfile sample은 다음과 같다.
* Oracle Database 19c (19.3.0) Enterprise Edition and Standard Edition 2
* Oracle Database 18c (18.4.0) Express Edition (XE)
* Oracle Database 18c (18.3.0) Enterprise Edition and Standard Edition 2
* Oracle Database 12c Release 2 (12.2.0.2) Enterprise Edition and Standard Edition 2
* Oracle Database 12c Release 1 (12.1.0.2) Enterprise Edition and Standard Edition 2
* Oracle Database 11g Release 2 (11.2.0.2) Express Edition (XE)

## 필요사항
생성시 버전과 edition별 software zip파일을 받아서 dockerfile과 같은 경로에 저장해야 build가 가능하며, 다음 경로를 통해서 다운 받을수 있다.  
https://www.oracle.com/database/technologies/oracle-database-software-downloads.html
```
/oracle/docker-images/OracleDatabase/SingleInstance/dockerfiles/18.3.0$ ls -l
total 4457729
-rwxrwxrwx 1 root root         64 Mar 20 10:21 Checksum.ee
-rwxrwxrwx 1 root root         64 Mar 20 10:21 Checksum.se2
-rwxrwxrwx 1 root root       3532 Mar 20 10:21 Dockerfile
-rwxrwxrwx 1 root root 4564649047 Mar 20 13:48 LINUX.X64_180000_db_home.zip
-rwxrwxrwx 1 root root       1050 Mar 20 10:21 checkDBStatus.sh
-rwxrwxrwx 1 root root        905 Mar 20 10:21 checkSpace.sh
-rwxrwxrwx 1 root root       3102 Mar 20 10:21 createDB.sh
-rwxrwxrwx 1 root root       7002 Mar 20 10:21 db_inst.rsp
-rwxrwxrwx 1 root root       9407 Mar 20 10:21 dbca.rsp.tmpl
-rwxrwxrwx 1 root root       2526 Mar 20 10:21 installDBBinaries.sh
-rwxrwxrwx 1 root root       6526 Mar 20 10:21 runOracle.sh
-rwxrwxrwx 1 root root       1015 Mar 20 10:21 runUserScripts.sh
-rwxrwxrwx 1 root root        758 Mar 20 10:21 setPassword.sh
-rwxrwxrwx 1 root root        932 Mar 20 10:21 setupLinuxEnv.sh
-rwxrwxrwx 1 root root        678 Mar 20 10:21 startDB.sh
```

## Docker Build
docker build하는 파일이 shell script로 되어 있으나, windows에서는 다음과 같은 명령어로 build가 가능하다.
```c
docker build --force-rm=true --no-cache=true --
build-arg DB_EDITION=se2 -t oracle/database:18.3.0-se2 -f DOCKERFILE  .
```

## Docker Build 결과
oracle database 18c 18.3 standard edition 2를 이용해서 생성된 docker image이다.  
Software ZIP파일 사이즈가 4.3GB인데, 압축해제 및 Oraclelinux + package 추가 설치등 해서 최종 이미지 사이즈는 8.4GB가 되었다.  
소요시간은 PC환경마다 다르겠지만, 약 25분 소요되었다.
```
REPOSITORY          TAG                 IMAGE ID            CREATED             SIZE
oracle/database     18.3.0-se2          e493651381d5        About an hour ago   8.4GB
<none>              <none>              1cac0701f076        About an hour ago   13.2GB
oraclelinux         7-slim              fd84774952b5        10 days ago         118MB
```

13.2GB의 이미지는 생성과정에서 발생되는 임시 이미지로 보이며, 삭제해도 무방한것으로 확인되었다.
```
REPOSITORY          TAG                 IMAGE ID            CREATED             SIZE
oracle/database     18.3.0-se2          e493651381d5        About an hour ago   8.4GB
oraclelinux         7-slim              fd84774952b5        10 days ago         118MB
```
깔끔 ^_______________________^

## docker volume
oracle database container를 위한 volume을 하나 생성해본다.
이 volume은 컨테이너 기동시 데이터 용도로 사용할 예정이고, 기동시 경로를 지정할수 있다.  
default로는 /opt/oracle/oradata로 설정하게 된다.
```
docker volume create oracle18c

docker volume ls
DRIVER              VOLUME NAME
local               oracle18c
```

## docker run
Oracle관련 parameters를 이용해서 docker container를 시작한다.  
`-e`로 시작되는 parameters들은 입력하지 않으면 default로 생성되거나, ORACLE_PWD를 지정하지 않으면 임의 생성후 아래와 같이 알려주게 되며, 이는 추후에 변경할수도 있다.
```
ORACLE PASSWORD FOR SYS, SYSTEM AND PDBADMIN: <blablabla>
```
default값을 최대한 사용하면 다음과 같은 명령어가 된다.
```cmd
docker run --name oracle18c -p 1521:1521 -p 5500:5500 -e ORACLE_SID=ORCLCDB -e ORACLE_PDB=ORCLPDB1 -e ORACLE_PWD=qwer123$ -e ORACLE_CHARACTERSET=AL32UTF8 -v oracle18c:/opt/oracle/oradata oracle/database:18.3.0-se2
```
실제 기동이 끝나고 다음과 같은 메시지가 나오기까지 약15분 소요되었다.
```
...생략...
SQL>
PL/SQL procedure successfully completed.

SQL> Disconnected from Oracle Database 18c Standard Edition 2 Release 18.0.0.0.0 - Production
Version 18.3.0.0.0
The Oracle base remains unchanged with value /opt/oracle
#########################
DATABASE IS READY TO USE!
#########################
...생략...
```

## sqlplus connection test
### install sqlplus
sql connection 테스트를 위해서 client basic package와 sqlplus package(option)를 설치해보자.  
https://www.oracle.com/database/technologies/instant-client/winx64-64-downloads.html  
최신 버전이 좋을테니(?) 19.5버전의 SQL*Plus Package를 다운받아 설치한다.

sqlplus를 통해서 접속하여 Database 버전과 에디션을 확인한다.
```
sqlplus.exe pdbadmin/qwer123$@//localhost:1521/ORCLPDB1
or
sqlplus.exe system/qwer123$@//localhost:1521/ORCLCDB
or
sqlplus.exe sys/qwer123$@//localhost:1521/ORCLCDB as sysdba

SQL*Plus: Release 19.0.0.0.0 - Production on Fri Mar 20 17:08:29 2020
Version 19.5.0.0.0

Copyright (c) 1982, 2019, Oracle.  All rights reserved.


Connected to:
Oracle Database 18c Standard Edition 2 Release 18.0.0.0.0 - Production
Version 18.3.0.0.0

SQL> exit
Disconnected from Oracle Database 18c Standard Edition 2 Release 18.0.0.0.0 - Production
Version 18.3.0.0.0
```
### non-install sqlplus
만약 SQL*Plus 설치가 안되어져 있다면 docker 명령어를 사용해 컨테이너내 sqlplus를 통해 접근할수도 있다.
```
docker exec -ti oracle18c sqlplus pdbadmin@ORCLPDB1

SQL*Plus: Release 18.0.0.0.0 - Production on Sun Mar 22 23:33:13 2020
Version 18.3.0.0.0

Copyright (c) 1982, 2018, Oracle.  All rights reserved.

Enter password: 
Last Successful login time: Sun Mar 22 2020 23:24:34 +00:00

Connected to:
Oracle Database 18c Standard Edition 2 Release 18.0.0.0.0 - Production
Version 18.3.0.0.0

SQL> exit
Disconnected from Oracle Database 18c Standard Edition 2 Release 18.0.0.0.0 - Production
Version 18.3.0.0.0
```


## 패스워드 변경
패스워드 변경은 다음과 같이 한다.
```
docker exec oracle18c ./setPassword.sh q1w2e3R$
```

```
sqlplus.exe sys/qwer123$@//localhost:1521/ORCLCDB as sysdba

SQL*Plus: Release 19.0.0.0.0 - Production on Mon Mar 23 08:29:42 2020
Version 19.5.0.0.0

Copyright (c) 1982, 2019, Oracle.  All rights reserved.

ERROR:
ORA-01017: invalid username/password; logon denied


Enter user-name: ^C

-------------------------------------------------

sqlplus.exe sys/q1w2e3R$@//localhost:1521/ORCLCDB as sysdba

SQL*Plus: Release 19.0.0.0.0 - Production on Mon Mar 23 08:29:58 2020
Version 19.5.0.0.0

Copyright (c) 1982, 2019, Oracle.  All rights reserved.


Connected to:
Oracle Database 18c Standard Edition 2 Release 18.0.0.0.0 - Production
Version 18.3.0.0.0

SQL> exit
Disconnected from Oracle Database 18c Standard Edition 2 Release 18.0.0.0.0 - Production
Version 18.3.0.0.0
```
