instalación EASY-RSA && crear estructura
fichero vars

Lo repito
INTERMEDIA

read -p "Se inicializa pki. Pulsa Enter "
cd /home/$usuario/ca/easy-rsa/
./easyrsa init-pki
ls -la pki/

read -p "Generamos solicitud de certificado. Pulsa Enter "

./easyrsa build-ca subca
mkdir tmp
cp /home/$usuario/ca/easy-rsa/pki/reqq

s/ca.req  tmp/subca.req

RAIZ
mkdir  /home/$usuario/tmp/
scp /tmp/subca.req USUARIO@IP:/home/$usuario/tmp/

cp /home/$usuario/tmp/ /home/$usuario/ca/easy-rsa/
./easyrsa import-req subca.req subca

read -p "Generamos certificado para la CA intermedia  -subca.crt. Pulsa Enter "
./easyrsa sign-req ca subca


scp /home/$usuario/ca/easy-rsa/pki/issued/subca.crt USUARIO@IP:/home/$usuario/tmp/
scp /home/$usuario/ca/easy-rsa/pki/ca.crt USUARIO@IP:/home/$usuario/tmp/

INTERMEDIA
cp /home/$usuario/tmp/ca.crt /home/$usuario/ca/easy-rsa/pki/

read -p "Generación Infraestructura y Creación CRL de certificados revocados  . Pulsa Enter "

cd /home/$usuario/ca/easy-rsa/
./easyrsa gen-dh

./easyrsa gen-crl


