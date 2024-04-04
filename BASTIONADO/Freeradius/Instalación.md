sudo apt install freeradius freeradius-ldap freeradius-utils

# Restringimos permisos

chown root:freerad /etc/freeradius/3.0/certs/dh
chown root:freerad /etc/ssl/private/ssl-cert-snakeoil.key
chown root:freerad /etc/ssl/certs/ssl-cert-snakeoil.pem
chown root:freerad /etc/ssl/certs/ca-certificates.crt

chmod 440 /etc/freeradius/3.0/certs/dh
chmod 440 /etc/ssl/certs/ca-certificates.crt
chmod 440 /etc/ssl/certs/ssl-cert-snakeoil.pem
chmod 440 /etc/ssl/private/ssl-cert-snakeoil.key

# Clientes Radius
nano /etc/freeradius/3.0/clientes.conf

client TP-Link-AP{
	ipaddr = 192.168.1.90
	secret = Camina-100
}

client pseudoradius{
	ipaddr = 192.168.1.20
	secret = Camina-100
}

