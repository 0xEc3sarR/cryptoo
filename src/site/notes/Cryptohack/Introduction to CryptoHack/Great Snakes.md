---
{"dg-publish":true,"permalink":"/cryptohack/introduction-to-crypto-hack/great-snakes/","dgPassFrontmatter":true,"noteIcon":""}
---

![Pasted image 20250703034632.png](/img/user/Pasted%20image%2020250703034632.png)

Descargamos el archivo de python que se nos proporciono:
- Una vez que lo abrimos nos aparece lo siguiente.

![Pasted image 20250703034824.png](/img/user/Pasted%20image%2020250703034824.png)
Lo unico que debemos hacer es ejecutar el codigo de python:

╼ $python3 great_snakes_35381fca29d68d8f3f25c9fa0a9026fb.py 
Here is your flag:
crypto{z3n_0f_pyth0n}

Explicacion sobre en que consiste el codigo de python:

- Este sript de python es muy tipico de un reto CTF (Capture The Flag) o de criptografia basica, donde se oculta una cadena (usualmente una flag) mediante una operacion reversible, en este caso una operacion XOR.

# Analizamos el codigo

Esto indica que el script debe ejecutarse en python3

``` python
#!/usr/bin/env python3
```

Importa el modulo sys, utilizado para acceder a la informacion del sistema (como la version de Python).

``` python
import sys
```

Aqui verifica si se esta ejecutando con Python 2, y muestra mensaje si es asi. (Python 2 ya esta obsoleto).

``` python
if sys.version_info.major == 2:
    print("You are running Python 2, which is no longer supported. Please update to Python 3.")
```

Esta es una lista de enteros que representan caracteres encriptados.

``` python
ords = [81, 64, 75, 66, 70, 93, 73, 72, 1, 92, 109, 2, 84, 109, 66, 75, 70, 90, 2, 92, 79]
```

Codigo para imprimir los resulados:

- Esta parte es la mas importante:
		Aplica una operacion XOR con 0x32 (es decir, 50 en decimal) a cada numero de la lista.
		Luego convierte el resultado en un caracter (chr( ... )).
		Finalmente los une a todos en una sola cadena y la imprime.

``` python
print("Here is your flag:")
print("".join(chr(o ^ 0x32) for o in ords))
```

## ¿Qué hace el XOR?

XOR es una operacion reversible:
- Si haces a ^ b = c, entonces c ^ b = a
- En este caso se utilizo XOR con 50 (0x32) para ofuscar cada caracter del mensaje. Para recuperar el mensaje original se vuelve a aplicar XOR con 50.

Por ejemplo el desencriptado paso a paso:

``` python
chr(81 ^ 0x32) = chr(81 ^ 50) = chr(99) = 'c'
```

Y asi vamos desencriptando toda la lista.

``` python
print("".join(chr(o ^ 0x32) for o in ords))
```

El resultado final es:

``` python
crypto{z3n_0f_pyth0n}
```

Podemos concluir que este codigo simplemente muestra una flag que ha sido ofuscada con XOR 0x32, una tecnica clasica y muy basica en retos de criptografia para principiantes.




