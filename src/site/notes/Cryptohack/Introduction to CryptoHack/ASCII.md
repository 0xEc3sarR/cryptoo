---
{"dg-publish":true,"permalink":"/cryptohack/introduction-to-crypto-hack/ascii/","dgPassFrontmatter":true,"noteIcon":""}
---

![Pasted image 20250703040810.png](/img/user/Pasted%20image%2020250703040810.png)

Para resolver este problema debemos recordar lo que hicimos anteriormente:

- El propio problema nos indica que: **En Python, el chr()La función se puede utilizar para convertir un número ordinal ASCII en un carácter (el ord()La función hace lo contrario).**

Por lo tanto nos estan dando caracteres ASCII lo cual como indica es un estandar de codificacion de 7 bits que permite la representacion de texto utilizando los numeros enteros del 0 al 127.

Entonces podemos reciclar codigo:

Aqui colocamos todo los nuermso ASCII que nos proporciona el problema.
``` python
ord = [99, 114, 121, 112, 116, 111, 123, 65, 83, 67, 73, 73, 95, 112, 114, 49, 110, 116, 52, 98, 108, 51, 125]
```
por ultimo, la parte mas importante del codigo la impresion de los resultados:

Aqui estoy uniendo con **"".join(chr(o) for o in ord)** aqui digo que para cada o dentro de **ord** voy a convertirlo en un caracter y con el **.join** los voy uniendo.
``` python
print("Here your flag")
print("".join(chr(o) for o in ord)
```

Finalmente mi codigo quedaria algo asi:
``` python
#!/usr/bin/env python3

# Problema:
# ASCII es un estándar de codificación de 7 bits que permite la representación de texto utilizando los números enteros del 0 al 127.
# Utilizando la siguiente matriz de enteros, convierta los números a sus caracteres ASCII correspondientes para obtener una bandera.

ord = [99, 114, 121, 112, 116, 111, 123, 65, 83, 67, 73, 73, 95, 112, 114, 49, 110, 116, 52, 98, 108, 51, 125]

print("Here your flag")
print("".join(chr(o) for o in ord)
```
