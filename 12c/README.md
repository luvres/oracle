## Oracle 12c Enterprise Edition
##### Download binaries
```
http://www.oracle.com/technetwork/database/enterprise-edition/downloads/index.html
cp $HOME/Downloads/linuxamd64_12201_database* .
```
#### Build image
```
./buildDockerImage.sh -v 12.2.0.1 -e
```
##### Run image
```
docker run --name Oracle -p 1521:1521 -d izone/oracle:12.2.0.1-ee
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
##### Link to Oracle 12c in Desktop
```
tee $HOME/Desktop/oracle.desktop <<EOF
[Desktop Entry]
Encoding=UTF-8
Type=Application
Name=Oracle EE
Icon=$HOME/1uvr3z/oracle/12c/oracle12c.png
Exec=/usr/bin/gnome-terminal -e "docker exec -ti Oracle sqlplus system/oracle@ORCL"
EOF

chmod +x $HOME/Desktop/oracle.desktop
```
##### Check version
```
select banner from v$version;
```
##### Save image
```
docker save izone/oracle:12.2.0.2-ee > img-oracle12c.tar
```
##### Load saved image
```
docker load < img-oracle12c.tar
```
