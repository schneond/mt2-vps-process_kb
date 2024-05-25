# mt2-vps-process_kb
 KB for setting up VPS to run mt2 server

# FreeBSD 14

    - Download FreeBSD amd64 ISO file from https://www.freebsd.org/releases (this manual is for FreeBSD 14, iso used: https://download.freebsd.org/releases/ISO-IMAGES/14.0/CHECKSUM.SHA256-FreeBSD-14.0-RELEASE-amd64).
    - Install system (manual in progres...), add user/s, setup IPv4/IPv6.
    - Log into server after the instalation via root user.
    - Enable SSH access to server:
        # sysrc sshd_enable=YES
    

**Now paste those commands**

    # cd / && pkg install mysql56-server-5.6.51.pkg mysql56-client-5.6.51.pkg
    # sysrc mysql_enable=YES
    # pwd_mkdb -p /etc/master.passwd
    # chown -R mysql /var/db/mysql && chgrp -R mysql /var/db/mysql && chmod -R 777 /var/db/mysql
    # service mysql-server start
    # chmod -R 777 /var/db/mysql
    # /usr/local/bin/mysqladmin -u root password maminka
    # mysql -p

        CREATE USER 'root'@'%' IDENTIFIED BY 'maminka';
        GRANT ALL PRIVILEGES ON *.* TO 'root'@'%' WITH GRANT OPTION;
        flush privileges;
        
        CREATE USER 'kaja'@'localhost' IDENTIFIED BY '787898';
        GRANT ALL PRIVILEGES ON *.* TO 'mt2'@'localhost' WITH GRANT OPTION;
        flush privileges;
        quit

    # cd / && tar zxvf mysqlTRI.gz
    # chown -R mysql /var/db/mysql && chgrp -R mysql /var/db/mysql && chmod -R 777 /var/db/mysql
