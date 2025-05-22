---
{"dg-publish":true,"permalink":"/hack-the-box/maquinas/easy/blue/","dgPassFrontmatter":true}
---

Empezamos realizando un escaneo de nmap

```
nmap -sCV 10.10.10.40 -T5 -oN blue_machine -v
```

![Pasted image 20250522000912.png](/img/user/Pasted%20image%2020250522000912.png)
Observamos que tenemos algunos puertos importantes como el 139 o 445.

Procederemos a hacer una enumeracion basica de smb con el objetivo de detectar el sistema operativo, nombre de host y dominio del equipo remoto.

```
nmap -p 139,445 --script=smb-os-discovery 10.10.10.40
```

![Pasted image 20250522001503.png](/img/user/Pasted%20image%2020250522001503.png)

Descubrimos que el hostname de Blue vienea ser 'haris-PC', tambien descubrimos que el sistema operativo corriendo en la maquina objetivo es **windows 7**.

Vamos a descubrir cuantos recursos compartidos hay en Blue.

```
nmap -p 445 --script=smb-enum-shares 10.10.10.40
```

Nos retorna lo siguiente, quiere decir que el puerto esta siendo filtrado por un firewall. Es decir, el paquete fue enviado, pero no recibio ninguna respuesta
![Pasted image 20250522002159.png](/img/user/Pasted%20image%2020250522002159.png)

Al igual con el puerto 139:

![Pasted image 20250522002327.png](/img/user/Pasted%20image%2020250522002327.png)

Si listamos con smbclient nos sale lo siguiente:
```
smbclient -L //10.10.10.40 -N
```

![Pasted image 20250522002414.png](/img/user/Pasted%20image%2020250522002414.png)

Pero por practica vamos a usar msfconsole:

- Empezaremos buscando modulos smb utiles:
```
	- search type:auxiliary smb
```

Veremos varios resultados, pero los mas utiles para enumeracion son:

- **auxiliary/scanner/smb/smb_enumshares**
- **auxiliary/scanner/smb/smb_enumusers**
- **auxiliary/scanner/smb/smb_version**
- **auxiliary/scanner/smb/smb_login** (si tienes credenciales)

## Enumerar recursos compatidos smb (shares)
Usaremos credenciales por defecto como por ejemplo: guest

- set RHOSTS 10.10.10.40
- set THREADS 5
- set SMBUser guest
- set SMBPass ""
- run

![Pasted image 20250522003301.png](/img/user/Pasted%20image%2020250522003301.png)

## Enumerar usuarios smb (users)

![Pasted image 20250522003642.png](/img/user/Pasted%20image%2020250522003642.png)

Aqui podemos ver que el usuario guest, no tiene permisos suficientes para poder enumerar usuarios, intentaremos con los usuarios.

## Como hemos encontrado recursos compartidos procederemos a ingresar

Para interactuar con los recursos compartidos usaremos smbclient:

```
smbclient //10.10.10.40/shares -N
```

Intentamos con **Share**:

![Pasted image 20250522160800.png](/img/user/Pasted%20image%2020250522160800.png)




Intentamos con "Users"

![Pasted image 20250522004743.png](/img/user/Pasted%20image%2020250522004743.png)

