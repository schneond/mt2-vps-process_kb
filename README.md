# mt2-vps-process_kb
 KB for setting up VPS to run mt2 server

# FreeBSD 14

Download FreeBSD amd64 ISO file from https://www.freebsd.org/releases (this manual is for FreeBSD 14, iso used: https://download.freebsd.org/releases/ISO-IMAGES/14.0/CHECKSUM.SHA256-FreeBSD-14.0-RELEASE-amd64).
Install system (manual in progres...), add user/s, setup IPv4/IPv6.
Log into server after the instalation via root user.
Enable **SSH** access to server:
    sysrc sshd_enable=YES
