---
{"dg-publish":true,"permalink":"/penetration-tester/getting-started/","dgPassFrontmatter":true,"noteIcon":""}
---

# Descripción general de Infosec 

Especializaciones que se ofrecen:


    Seguridad de redes e infraestructuras
    Seguridad de las aplicaciones
    Pruebas de seguridad
    Auditoría de sistemas
    Planificación de la continuidad del negocio
    Análisis forense digital
    Detección y respuesta a incidentes


![Pasted image 20250702211848.png](/img/user/Pasted%20image%2020250702211848.png)

![Pasted image 20250702211901.png](/img/user/Pasted%20image%2020250702211901.png)

Es clave tambien mantenerse organizado al momento de estudiar:

Ya sea que estemos realizando evaluaciones de clientes, participando en CTF, tomando un curso en la Academia o en otro lugar, o participando en cajas/laboratorios HTB, la organización siempre es crucial. Es fundamental priorizar la documentación clara y precisa desde el principio. Esta habilidad nos beneficiará independientemente de nuestra trayectoria en seguridad de la información o incluso en otras carreras profesionales. 

![Pasted image 20250702212701.png](/img/user/Pasted%20image%2020250702212701.png)

![Pasted image 20250702220231.png](/img/user/Pasted%20image%2020250702220231.png)

``` c++
Consejo: Aprender el lenguaje Markdown es fácil y muy útil para tomar notas, ya que se puede representar fácilmente de una forma visualmente atractiva y organizada. 
```

# Connecting Using VPN

Una red privada virtual (VPN) nos permite conectarnos a una red privada (interna) y acceder a hosts y recursos como si estuviéramos conectados directamente a la red privada de destino. Se trata de un canal de comunicación seguro a través de redes públicas compartidas para conectarse a una red privada (es decir, un empleado que se conecta remotamente a la red corporativa de su empresa desde su casa). Las VPN proporcionan cierto grado de privacidad y seguridad al cifrar las comunicaciones a través del canal para evitar escuchas y el acceso a los datos que lo atraviesan. 

![Pasted image 20250702221332.png](/img/user/Pasted%20image%2020250702221332.png)

## ¿Por qué utilizar una VPN?

Podemos utilizar un servicio VPN como NordVPN o Private Internet Accessy conectarnos a un servidor VPN en otra parte de nuestro país o región del mundo para ocultar nuestro tráfico de navegación o disfrazar nuestra dirección IP pública. Esto puede brindarnos cierto nivel de seguridad y privacidad. Sin embargo, dado que nos conectamos al servidor de una empresa, siempre existe la posibilidad de que se registren datos o que el servicio VPN no siga las mejores prácticas de seguridad o las funciones de seguridad que anuncia. Usar un servicio VPN conlleva el riesgo de que el proveedor no cumpla con lo que promete y registre todos los datos. Usar un servicio VPN no garantiza el anonimato ni la privacidad, pero es útil para eludir ciertas restricciones de red/cortafuegos o al conectarse a una posible red hostil (por ejemplo, la red inalámbrica de un aeropuerto público). Nunca se debe usar un servicio VPN pensando que nos protegerá de las consecuencias de realizar actividades maliciosas. 

## Conexión a HTB VPN

![Pasted image 20250702221733.png](/img/user/Pasted%20image%2020250702221733.png)

![Pasted image 20250702221807.png](/img/user/Pasted%20image%2020250702221807.png)

Mecanografía **netstat -rn** nos mostrará las redes accesibles a través de la VPN.

![Pasted image 20250702221854.png](/img/user/Pasted%20image%2020250702221854.png)

Tipos de SHELL:

![Pasted image 20250702222054.png](/img/user/Pasted%20image%2020250702222054.png)

Algunos de los puertos mas comunes:

![Pasted image 20250702222121.png](/img/user/Pasted%20image%2020250702222121.png)

 ## The current OWASP Top 10 list is:

![Pasted image 20250702222152.png](/img/user/Pasted%20image%2020250702222152.png)


## Usando Tmux

Los multiplexores de terminales, como tmux o ScreenSon excelentes utilidades para ampliar las funciones de una terminal estándar de Linux, como tener varias ventanas dentro de una misma terminal y saltar entre ellas. Veamos algunos ejemplos de uso. tmux, que es el más común de los dos. Si tmuxno está presente en nuestro sistema Linux, podemos instalarlo con el siguiente comando: 

``` c++
0xEC3sarR@htb[/htb]$ sudo apt install tmux -y
```

![Pasted image 20250702223013.png](/img/user/Pasted%20image%2020250702223013.png)

Primero usamos el prefijo Ctrl + B, soltamos y luego hacemos uso de la letra C por ejemplo o % para dividir terminales.

![Pasted image 20250702223102.png](/img/user/Pasted%20image%2020250702223102.png)

Hoja de referencia a comandos de tmux para dominarlos completamente.

https://tmuxcheatsheet.com/

Video de ippsec: https://www.youtube.com/watch?v=Lqehvpe_djs

## Comandos basicos y necesarios de vim:

![Pasted image 20250702222528.png](/img/user/Pasted%20image%2020250702222528.png)

Hoja de trucos para vim: https://vimsheet.com/


nc IP PORT

![Pasted image 20250702222708.png](/img/user/Pasted%20image%2020250702222708.png)
![Pasted image 20250702222738.png](/img/user/Pasted%20image%2020250702222738.png)

# Escaneo de servicios:

## Nmap

Comencemos con el análisis más básico. Supongamos que queremos realizar un análisis básico contra un objetivo ubicado en 10.129.42.253. Para ello, debemos escribir nmap 10.129.42.253y pulsamos Enter. Vemos que el NmapEl análisis se completó muy rápidamente. Esto se debe a que, si no especificamos ninguna opción adicional, Nmap solo analizará los 1000 puertos más comunes por defecto. El resultado del análisis revela que los puertos 21, 22, 80, 139 y 445 están disponibles. 

![Pasted image 20250703010738.png](/img/user/Pasted%20image%2020250703010738.png)

![Pasted image 20250703010806.png](/img/user/Pasted%20image%2020250703010806.png)

## Scripts de Nmap

Especificando -sC Ejecutará muchos scripts predeterminados útiles en un objetivo, pero hay casos en los que se requiere ejecutar un script específico. Por ejemplo, en una evaluación, se nos podría solicitar que auditemos una instalación grande de Citrix. Podríamos usar esto. Nmapscript para auditar la vulnerabilidad grave de Citrix NetScaler ( CVE-2019–19781 ), mientras Nmap También tiene otros scripts para auditar una instalación de Citrix. 

![Pasted image 20250703010928.png](/img/user/Pasted%20image%2020250703010928.png)

## Ataque a los servicios de red

### Banner Grabbing

``` c++
Como se mencionó anteriormente, la captura de banners es una técnica útil para identificar rápidamente un servicio. A menudo, un servicio intentará identificarse mostrando un banner al iniciarse una conexión. Nmap intentará capturar los banners si la sintaxis... nmap -sV --script=banner <target>se especifica. También podemos intentar esto manualmente usando NetcatTomemos otro ejemplo, utilizando el ncversión de Netcat: 
```

**nc -nv 10.129.42.253 21**

![Pasted image 20250703011715.png](/img/user/Pasted%20image%2020250703011715.png)


## FTP

Vale la pena familiarizarse con FTP, ya que es un protocolo estándar y este servicio a menudo puede contener datos interesantes. NmapEl análisis del puerto predeterminado para FTP (21) revela la instalación de vsftpd 3.0.3 que identificamos previamente. Además, también informa que la autenticación anónima está habilitada y que... pubEl directorio está disponible. 

![Pasted image 20250703012516.png](/img/user/Pasted%20image%2020250703012516.png)

Conectemonos a ftp: **ftp -p 10.129.42.252**

![Pasted image 20250703012550.png](/img/user/Pasted%20image%2020250703012550.png)


## SMB

SMB (Server Message Block) es un protocolo común en equipos Windows que proporciona múltiples vectores de movimiento vertical y lateral. Los datos confidenciales, incluidas las credenciales, pueden encontrarse en recursos compartidos de archivos de red, y algunas versiones de SMB pueden ser vulnerables a exploits de RCE como EternalBlue . Es crucial enumerar cuidadosamente esta considerable superficie de ataque potencial. Nmap tiene muchos scripts para enumerar SMB, como smb-os-discovery.nse , que interactuará con el servicio SMB para extraer la versión del sistema operativo informada. 

**nmap --script smb-os-discovery.nse -p445 10.10.10.40**

![Pasted image 20250703012959.png](/img/user/Pasted%20image%2020250703012959.png)

En este caso, el host ejecuta un sistema operativo Windows 7 heredado, y podríamos realizar una enumeración adicional para confirmar si es vulnerable a EternalBlue. Metasploit Framework cuenta con varios módulos para EternalBlue que pueden usarse para validar la vulnerabilidad y explotarla, como veremos en una próxima sección. Podemos ejecutar un análisis de nuestro objetivo para esta sección del módulo y recopilar información del servicio SMB. Podemos determinar que el host ejecuta un kernel de Linux, Samba versión 4.6.2, y el nombre de host es GS-SVCSCAN. 

![Pasted image 20250703013143.png](/img/user/Pasted%20image%2020250703013143.png)

## Acciones

SMB permite a usuarios y administradores compartir carpetas y permitir el acceso remoto a ellas por parte de otros usuarios. A menudo, estos recursos compartidos contienen archivos con información confidencial, como contraseñas. Una herramienta que puede enumerar e interactuar con recursos compartidos SMB es smbclient -L. La bandera especifica que queremos recuperar una lista de recursos compartidos disponibles en el host remoto, mientras que -Nsuprime la solicitud de contraseña.

**smbclient -N -L \\\\10.129.42.253**

![Pasted image 20250703013229.png](/img/user/Pasted%20image%2020250703013229.png)


![Pasted image 20250703013303.png](/img/user/Pasted%20image%2020250703013303.png)

![Pasted image 20250703013316.png](/img/user/Pasted%20image%2020250703013316.png)

## SNMP

Las cadenas de comunidad SNMP proporcionan información y estadísticas sobre un enrutador o dispositivo, lo que nos ayuda a acceder a él. Las cadenas de comunidad predeterminadas del fabricante... public y private. A menudo no cambian. En las versiones 1 y 2c de SNMP, el acceso se controla mediante una cadena de comunidad de texto plano, y si conocemos el nombre, podemos acceder a ella. El cifrado y la autenticación se añadieron en la versión 3 de SNMP. Se puede obtener mucha información de SNMP. El análisis de los parámetros del proceso podría revelar las credenciales transmitidas en la línea de comandos, que podrían reutilizarse para otros servicios accesibles externamente, dada la prevalencia de la reutilización de contraseñas en entornos empresariales. También se puede revelar la información de enrutamiento, los servicios vinculados a interfaces adicionales y la versión del software instalado.

**snmpwalk -v 2c -c public 10.129.42.253 1.3.6.1.2.1.1.5.0**

![Pasted image 20250703013806.png](/img/user/Pasted%20image%2020250703013806.png)


**snmpwalk -v 2c -c private  10.129.42.253 **

![Pasted image 20250703013829.png](/img/user/Pasted%20image%2020250703013829.png)

Se puede utilizar una herramienta como onesixtyone para forzar los nombres de las cadenas de la comunidad utilizando un archivo de diccionario de cadenas de la comunidad comunes, como dict.txt archivo incluido en el repositorio de GitHub de la herramienta. 

**onesixtyone -c dict.txt 10.129.42.254**

![Pasted image 20250703013915.png](/img/user/Pasted%20image%2020250703013915.png)


Ejercicios:


Hacemos un scano de nmap y respondemos las preguntas solicitadas sobre los puertos pedidos.


![Pasted image 20250703022637.png](/img/user/Pasted%20image%2020250703022637.png)

Luego ingresamos al smb despues de haber enumerado con las credenciales bob:Welcome1, la cual se nos esta proporcionando en el modulo.

![Pasted image 20250703022605.png](/img/user/Pasted%20image%2020250703022605.png)

