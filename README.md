# Installation of metin2 on VPS [FreeBSD 14]
> KB -Nastavení VPS pro spuštění Mt2 serveru. 

> [!TIP]
> Používej [MobaXterm](https://mobaxterm.mobatek.net/) nebo [mRemoteNG](https://mremoteng.org/) místo Putty pro ulehčení práce.

## Mysql 5.6 download & instalace
Jelikož na konci roku 2023 mysql5.6 skoncila podpora a smazala se z oficialních portů, je třeba to obejít. Tady je ke stažení kompletní **mysql5.6** client i server část a **lib**ky potřebné k rozjetí **mysql**.
- [mysql5.6-client.pkg](https://github.com/schneond/mt2-vps-process_kb/raw/main/mysql56-client.pkg) :point_down:

- [mysql5.6-server.pkg](https://github.com/schneond/mt2-vps-process_kb/raw/main/mysql56-server.pkg) :point_down:

- [mysql5.6 libs 64bit](https://github.com/schneond/mt2-vps-process_kb/raw/main/mysql56-server.pkg) :point_down:
> [!NOTE]
> Pomocí WinSCP přenes všechny 3 soubory mysql do složky **/**.

**SSH na server a použít tyto příkazy:**
 ```ruby
cd / && pkg install mysql56-server-5.6.51.pkg mysql56-client-5.6.51.pkg
 ```
  ```ruby
sysrc mysql_enable=YES
 ```
 ```ruby
pwd_mkdb -p /etc/master.passwd
 ```
  ```ruby
chown -R mysql /var/db/mysql && chgrp -R mysql /var/db/mysql && chmod -R 777 /var/db/mysql
 ```
 ```ruby
service mysql-server start
 ```
 ```ruby
chmod -R 777 /var/db/mysql
 ```
 Zde místo "_heslo_" použít cokoliv chceš používat k přihlašování do mysql.
 ```ruby
/usr/local/bin/mysqladmin -u root password heslo 
 ```
### :tada: Nainstalováno! :tada:
## FreeBSD 14

- **Stáhnout** FreeBSD amd64 ISO z https://www.freebsd.org/releases (tento návod slouží pro FreeBSD 14, použité iso: [FreeBSD14](https://download.freebsd.org/releases/ISO-IMAGES/14.0/CHECKSUM.SHA256-FreeBSD-14.0-RELEASE-amd64)).
- Instalovat systém (návod se vytváří...), přidat uživatele, nastavit IPv4/IPv6.
- Po nainstalování se přihlásit jako **root** uživatel.
- Povolit SSH přístup:
 ```ruby
   sysrc sshd_enable=YES
```    

**Teď do konzole a použít tyto příkazy:**
```
# cd / && pkg install mysql56-server-5.6.51.pkg mysql56-client-5.6.51.pkg
# sysrc mysql_enable=YES
# pwd_mkdb -p /etc/master.passwd
# chown -R mysql /var/db/mysql && chgrp -R mysql /var/db/mysql && chmod -R 777 /var/db/mysql
# service mysql-server start
# chmod -R 777 /var/db/mysql
# /usr/local/bin/mysqladmin -u root password maminka 
# mysql -p
```
```ruby
CREATE USER 'root'@'%' IDENTIFIED BY 'maminka';
GRANT ALL PRIVILEGES ON *.* TO 'root'@'%' WITH GRANT OPTION;
flush privileges;
        
CREATE USER 'kaja'@'localhost' IDENTIFIED BY '787898';
GRANT ALL PRIVILEGES ON *.* TO 'mt2'@'localhost' WITH GRANT OPTION;
flush privileges;
quit
```
```
# cd / && tar zxvf mysqlTRI.gz
# chown -R mysql /var/db/mysql && chgrp -R mysql /var/db/mysql && chmod -R 777 /var/db/mysql
```
## Obsah serverfiles
![MobaXterm](/assets/mobaxterm.png)

### Základní FreeBSD příkazy
| Příkaz | Popis |
| --- | --- |
| `cd` | Za 'cd' přidat cestu do jaké složky chceš jít |
| `pwd` | Zobrazí v jaké složce se náchážíš |
| `ls -la` | Zobrazí obsah složky ve které aktuálně jsi |
| `nano` | Za 'nano' přidat jméno textového souboru, který chceš upravit |

### Nastavení připojení pouze pro specifické IPs
Vytvoření configuračního souboru pro **PF** službu (Firewall)
```ruby
nano /etc/pf.conf
```
Dovnitř vložit tento obsah:
```ruby
# Jmeno sitove karty z ifconfig
ext_if="vtnet0"

# Zde pridat svou IP adresu, aby ses dostal pres SSH dovnitr
table <ssh_whitelist> { xx.xxx.xxx.xx, xx.xxx.xxx.xx }

# Defaultni nastaveni blokace vsech IPs
block in all

# Povoleni odchoziho pripojeni vsech IPs
pass out all keep state

# Povoleni SSH pristupu pro specificke IPs z listu <ssh_whitelist>
pass in on $ext_if proto tcp from <ssh_whitelist> to any port 22 flags S/SA keep state
```
Aktivování služby **PF**
```ruby
pfctl -e && sysrc pf_enable=YES
```
Poslední věc, reloadnout configurační soubor. // Toto dělat pokaždé, kdy přidáš novou IP adresu
```ruby
pfctl -f /etc/pf.conf
```
**Hotovo** :tada:
> [!NOTE]
> Kontrolu o statistice vyhledání/pokusu o přihlášení/zabkovonání lze vylistovat pomocí příkazu "**pfctl -si**".

**Progress**
- [ ] Návod na instalaci FreeBSD 
- [x] Návod na instalaci mysql5.6 :tada:
- [ ] Návod na kompilaci source game a server
- [ ] Návod na rozbalení a zabalení packu klienta
- [x] Odkaz ke stažení různých toolů :tada:











> [!NOTE]
> Useful information that users should know, even when skimming content.

> [!TIP]
> Helpful advice for doing things better or more easily.

> [!IMPORTANT]
> Key information users need to know to achieve their goal.

> [!WARNING]
> Urgent info that needs immediate user attention to avoid problems.

> [!CAUTION]
> Advises about risks or negative outcomes of certain actions.
