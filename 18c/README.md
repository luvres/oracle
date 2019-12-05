## Oracle 18c Enterprise Edition
##### Download binaries
```
http://www.oracle.com/technetwork/database/enterprise-edition/downloads/index.html
cp $HOME/Downloads/LINUX.X64_180000_db_home* .
```
#### Build image
```
./buildDockerImage.sh -v 18.3.0 -e
```
##### Run image
```
docker run --name Oracle -p 1521:1521 -d izone/oracle:18.3.0-ee

docker logs -f Oracle
```
##### Run image always
```
docker run --name Oracle \
--restart=always \
--publish=1521:1521 \
-d izone/oracle:18.3.0-ee

docker logs -f Oracle
```
##### Set password
```
docker exec Oracle ./setPassword.sh oracle
```
#### Access database
```
docker exec -ti Oracle sqlplus sys/oracle@ORCLCDB as sysdba
```
##### Check version
```
select banner from v$version;
```
##### Save image
```
docker save izone/oracle:18.3.0-ee > img-oracle18c.tar
```
##### Load saved image
```
docker load < img-oracle18c.tar
```
