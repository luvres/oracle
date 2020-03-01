## Oracle 19c Enterprise Edition
### Oracle 11g Express Edition
-----
### Oracle 19c Enterprise Edition on Oraclelinux

##### Run image
```
docker run --name Oracle -p 1521:1521 -d izone/oracle:19.3.0-ee

docker logs -f Oracle
```
##### Run image always
```
docker run --name Oracle \
--restart=always \
--publish=1521:1521 \
-d izone/oracle:19.3.0-ee

docker logs -f Oracle
```
##### Set password
```
docker exec Oracle ./setPassword.sh oracle
```
#### Access database
```
docker exec -ti Oracle sqlplus sys/oracle@ORCLCDB as sysdba

docker exec -ti Oracle sqlplus system/oracle@ORCL
```
##### Check version
```
select banner from v$version;
```

-----
### Oracle Express Edition 11g Release 2 on Debian Wheezy

### Installation
```
    docker pull izone/oracle:11g
```
### Run with 8080 and 1521 ports opened:
```
docker run --rm -h oraclexe --name OracleXE \
	-p 8080:8080 \
	-p 1521:1521 \
	izone/oracle:11g
```
### Run with data on host and reuse it:
```
docker run --rm -h oraclexe --name OracleXE \
	-p 8080:8080 \
	-p 1521:1521 \
	-v /my/oracle/data:/u01/app/oracle \
	izone/oracle:11g
```
### Run with customization of processes, sessions, transactions
#### This customization is needed on the database initialization stage. If you are using mounted folder with DB files this is not used:
```
##Consider this formula before customizing:
#processes=x
#sessions=x*1.1+5
#transactions=sessions*1.1
docker run --name OracleXE -h oraclexe\
	-p 8080:8080 \
	-p 1521:1521 \
	-v /my/oracle/data:/u01/app/oracle\
	-e processes=1000 \
	-e sessions=1105 \
	-e transactions=1215 \
	-d izone/oracle:11g
```
### Connect database with following setting:
```
hostname: localhost
port: 1521
sid: xe
username: system
password: oracle
```
### Password for SYS & SYSTEM:
```
oracle
```
### Connect to Oracle Application Express web management console with following settings:
```
http://localhost:8080/apex
workspace: INTERNAL
user: ADMIN
password: oracle
```
### Apex upgrade up to v 5.*
```
docker run --rm --name OracleXE -h oraclexe \
	--volumes-from ${DB_CONTAINER_NAME} \
	--link ${DB_CONTAINER_NAME}:oracle-database \
	-e PASS=YourSYSPASS \
	-ti sath89/apex install
```
**CHANGELOG**
* Fixed issue with reusable mounted data
* Fixed issue with ownership of mounted data folders
* Fixed issue with Gracefull shutdown of service
* Reduse size of image from 3.8G to 825Mb
* Database initialization moved out of the image build phase. Now database initializes at the containeer startup with no database files mounted
* Added database media reuse support outside of container
* Added graceful shutdown on containeer stop
* Removed sshd
* Build with Debian Wheezy
