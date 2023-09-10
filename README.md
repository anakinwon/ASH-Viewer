<매뉴얼>
https://cafe.naver.com/sysdba/34



1. 다운로드

- 아래 사이트에 접속해서 ASH Viewer & OPEN JDK 를 다운 받습니다.

 
DBA 실무 노하우 실습환경
■ 실습 환경 가상서버 이미지 DBA실무 가상머신 이미지 ■ 기타 프로그램 D2Coding : 고정글꼴 폰트 putty : 터미널 접속 프로그램 sqldeveloper : 오라클 접속/관리 프로그램 tvnviewer : 리눅스 GUI 원격 접속 프로그램(VNC 클라이언트) ASH Viewer & OPEN JDK : 오라클 실시간 성능분석 프로그램

edu.dba.co.kr

- 다운 받은 파일(ashv_jdk.zip)의 압축을 풀면 세개의 파일로 구성되어 있습니다.

. ashv-4.4.0-bin.zip : ASH Viewer

. ojdbc8.jar : JDBC 드라이버

. openjdk-19.0.2_windows-x64_bin.zip : ASH Viewer가 다소 높은 JDK를 필요로 하기 때문에 새로 설치하는 것을 권장합니다. 별도의 설치과정은 필요없고 압축만 풀고 경로만 지정하면 됩니다.

​

​

2. 디렉토리 구성

﻿D:\Program\ashv-4.4.0 : ashv-4.4.0-bin.zip
D:\Program\jdk-19.0.2 : openjdk-19.0.2_windows-x64_bin.zip
D:\Program\jdk-19.0.2\lib : ojdbc8.jar 파일을 복사
- 위와 같이 디렉토리를 구성하고 압축을 해제합니다. 디렉토리 구성은 원하는 대로 하시면 됩니다.

- 각 디렉토리 파일 목록은 다음과 같습니다.



​

​

3. run.bat 파일 수정​

@REM ----------------------------------------------------------------------------
@REM Licensed to the GNU GENERAL PUBLIC LICENSE Version 3
@REM ----------------------------------------------------------------------------

@REM ----------------------------------------------------------------------------
@REM ASH Viewer start up batch script
@REM
@REM Required ENV vars:
@REM JAVA_HOME - location of a JDK home dir

SET JAVA_HOME=D:\Program\jdk-19.0.2

SET JAVA_EXE="%JAVA_HOME%\bin\java.exe"

%JAVA_EXE% -Xmx1024m -jar ASH-Viewer.jar
- D:\Program\ashv-4.4.0\run.bat 파일의  JAVA_HOME을 변경합니다.

​

​

4. 실행 및 접속정보 설정


- run.bat 파일을 더블클릭하여 실행합니다. 화면이 뜨지 않을 경우 CMD 창에서 run.bat를 실행하여 로그를 확인합니다.

​


- Edit - 접속정보 설정 - Save - Connect 순서로 진행합니다.

URL : jdbc:oracle:thin:@ip_address:port:SID
JDBC : D:\Program\jdk-19.0.2\lib\ojdbc8.jar
- 접속정보는 URL 부분에 위와 같은 양식으로 설정합니다.

- Open JAR 버튼을 눌러 위에서 다운받아 복사한 JDBC 드라이버의 위치를 지정합니다.
- 




--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
# ASH Viewer

ASH Viewer provides graphical view of active session history data within the database.

Supported databases: Oracle, PostgreSQL

## Table of contents

- [Quick start](#quick-start)
- [How it works](#how-it-works)
- [Build](#build)
- [Security](#security)
- [Bugs and feature requests](#bugs-and-feature-requests)
- [Downloads](#downloads)
- [Based on](#based-on)
- [License](#license)
- [Contact](#contact)

![ASH-Viewer](media/main.png)

## Quick start
- [Download the latest binary file.](https://github.com/akardapolov/ASH-Viewer/releases)
- Download JDBC driver for your database ([Oracle](https://www.oracle.com/database/technologies/appdev/jdbc-downloads.html), [PostgreSQL](https://jdbc.postgresql.org/download.html))
- Unpack the binary archive and run ASH-Viewer.jar
- Open connection dialog and populate them with data (URL for Oracle database: **jdbc:oracle:thin:@host:port:SID**)

 ![ASH-Viewer connection dialog](media/connection.png)
- Press Connect button and start to monitor your system and highlight a range to show details.

 ![ASH-Viewer Top activity](media/top.png)
- Review Raw data interface to gain a deep insight into active session history

![ASH-Viewer raw data](media/raw.png)
- Double-click on Top sql & sessions interface to get window with ASH details by sql or session ID

 ![ASH-Viewer sql details](media/sql.png)

 ![ASH-Viewer session details](media/session.png)

[Return to Table of Contents](#table-of-contents)

## How it works
Active Session History (ASH) is a view in Oracle database that maps a circular buffer in the SGA.
  The name of the view is V$ACTIVE_SESSION_HISTORY. This view is populated every second
  and will only contain data for 'active' sessions, which are defined as sessions
  waiting on a non-idle event or on a CPU.
  
ASH Viewer provides graphical Top Activity, similar Top Activity analysis and Drilldown
    of Oracle Enterprise Manager performance page. ASH Viewer store ASH data locally using
    embedded database Oracle Berkeley DB Java Edition.
    
For Oracle standard edition and PostgreSQL, ASH Viewer emulate ASH, storing active session data on local storage.
  
Please note that v$active_session_history is a part of the Oracle Diagnostic Pack and requires a purchase of the ODP license.

[Return to Table of Contents](#table-of-contents)

## Build

To compile the application into an executable jar file, do the following:

1. Install JDK version 11 or higher, Maven and Git on your local computer.
    ```shell
    java -version  
    mvn -version
    git --version 
    ``` 
2. Download the source codes of the application to your local computer using Git

    ```shell
    git clone <url source code storage system>
    cd ASH-Viewer
    ```

3. Compile the project using Maven
    ```shell
    mvn clean compile
   ```

4. Execute the Maven command to build an executable jar file with tests running
    ```shell
     mvn clean package -DskipTests=true 
    ```

An executable jar file like `ashv-<VERSION>-SNAPSHOT-jar-with-dependencies.jar` will be located at the relative path ashv/target

[Return to Table of Contents](#table-of-contents)

## Security
Encryption and Container settings provide security for database passwords (go to Other tab -> Security block)

### Encryption
Encryption setting has AES and PBE options
- **AES** - Advanced Encryption Standard (AES) with 256-bit key encryption, from the [Bouncy Castle provider](https://www.bouncycastle.org/), [FIPS](https://www.nist.gov/standardsgov/compliance-faqs-federal-information-processing-standards-fips#:~:text=are%20FIPS%20developed%3F-,What%20are%20Federal%20Information%20Processing%20Standards%20(FIPS)%3F,by%20the%20Secretary%20of%20Commerce.) compliant algorithm;
- **PBE** - Password based encryption (PBE) in PBEWithMD5AndDES mode with secret key (computer name or hostname). This option is weak and deprecated, please use AES when possible;

### Container
It's the way to store your encrypted data
- **DBAPI** - You sensitive data stored in Windows Data Protection API (DPAPI), maximum protected, Windows only;
- **Registry** - OS System registry storage using java preferences API - weak, but better than **Configuration**;
- **Configuration** - All data in local configuration file - weak, not recommended;

### Recommendations 
- use **AES** encryption and Windows Data Protection API (**DBAPI**) whenever possible;
- do not use **PBE** copied configuration on another host, you need to change password with a new secret key (always do it for password leak prevention).

[Return to Table of Contents](#table-of-contents)

## Bugs and feature requests
If you found a bug in the code or have a suggestion for improvement, [Please open an issue](https://github.com/akardapolov/ASH-Viewer/issues)  

[Return to Table of Contents](#table-of-contents)
 
## Downloads
- [Current version](https://github.com/akardapolov/ASH-Viewer/releases)
- [Old release 3.5.1 on github.com](https://github.com/akardapolov/ASH-Viewer/releases/tag/v3.5.1)
- [Mirror on sourceforge.net](https://sourceforge.net/projects/ashv/files/)   

[Return to Table of Contents](#table-of-contents)

## Based on
- [JFreeChart by David Gilbert](http://www.jfree.org)
- [E-Gantt Library by Keith Long](https://github.com/akardapolov/ASH-Viewer/tree/master/egantt)
- [Berkeley DB Java Edition](http://www.oracle.com/database/berkeley-db)
- [SwingLabs GUI toolkit by alexfromsun, kleopatra, rbair and other](https://en.wikipedia.org/wiki/SwingLabs)
- [Dagger 2 by Google](https://dagger.dev/)
- [AES cipher by Bouncy Castle](https://www.bouncycastle.org/)
- [Windows DPAPI Wrapper by @peter-gergely-horvath](https://github.com/peter-gergely-horvath/windpapi4j)

[Return to Table of Contents](#table-of-contents)

## License
[![GPLv3 license](https://img.shields.io/badge/License-GPLv3-blue.svg)](http://perso.crans.org/besson/LICENSE.html)

  Code released under the GNU General Public License v3.0

[Return to Table of Contents](#table-of-contents)

## Contact
  Created by [@akardapolov](mailto:akardapolov@gmail.com) - feel free to contact me!

[Return to Table of Contents](#table-of-contents)
