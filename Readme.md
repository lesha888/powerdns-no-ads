Local PowerDNS with ads filter
==============================

### Original repo

It is mirror of [original repo](https://git.lyalyuev.info/silver/pdns)

### Dependensies:

* [docker](https://www.docker.com/)
* [docker-compose](https://docs.docker.com/compose/)

### Spin up:

#### Clone repo

```bash
sudo mkdir /opt/docker
sudo chown <user> /opt/docker
git clone https://github.com/DmitriyLyalyuev/powerdns-no-ads /opt/docker/pdns
cd /opt/docker/pdns
```

#### Create DB

```bash
docker-compose up -d mysql
```

#### MySQL credentials

To get MySQL console run:

```bash
docker exec -ti pdns_mysql_1 mysql -u root -p
```

Default root password for MySQL is '**12345**'.

Create user and database:

```mysql
CREATE USER 'powerdns_user'@'%' IDENTIFIED BY 'powerdns';
GRANT ALL PRIVILEGES ON powerdns.* TO 'powerdns_user'@'%';
CREATE DATABASE powerdns;
exit
```

#### Run your own DNS server with ads filter

```bash
docker-compose up -d
```

This will run docker container with mount "/opt/docker/pdns/powerdns-server" folder to it as volume /etc/powerdns.

### Update ads lists:

To update database run 

```bash
docker exec -ti pdns_pdns_1 bash
cd /etc/powerdns/bind
./getnewlist.sh && ./import.sh && ./clean.sh
exit
```

It will download new ads servers lists and generate new sql dump file, then import it to the MySQL docker comtainer.

### White listing:

White list of domains you can specify on top of "getnewlist.sh".
