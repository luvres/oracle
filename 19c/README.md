## Oracle 19c Enterprise Edition
##### Download binaries
```
http://www.oracle.com/technetwork/database/enterprise-edition/downloads/index.html

git clone https://github.com/luvres/oracle.git

cd oracle/19c

cp $HOME/Downloads/LINUX.X64_193000_db_home* ./19.3.0
```
#### Build image
```
./buildDockerImage.sh -v 19.3.0 -e
```
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
```
##### Check version
```
select banner from v$version;
```
##### Save image
```
docker save izone/oracle:19.3.0-ee > img-oracle19c.tar
```
##### Load saved image
```
docker load < img-oracle19c.tar
```
