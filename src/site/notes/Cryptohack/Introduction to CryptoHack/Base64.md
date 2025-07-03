---
{"dg-publish":true,"permalink":"/cryptohack/introduction-to-crypto-hack/base64/","dgPassFrontmatter":true,"noteIcon":""}
---

![Pasted image 20250703044137.png](/img/user/Pasted%20image%2020250703044137.png)

Otro esquema de codificación común es Base64, que permite representar datos binarios como una cadena ASCII utilizando un alfabeto de 64 caracteres. Un carácter de una cadena Base64 codifica 6 dígitos binarios (bits), por lo que 4 caracteres Base64 codifican tres bytes de 8 bits.

Base64 es el sistema más utilizado en línea, por lo que datos binarios, como imágenes, pueden incluirse fácilmente en archivos HTML o CSS.

Tome la siguiente cadena hexadecimal, descodifíquela en bytes y luego codifíquela en Base64. 

Consejo: **En Python, después de importar el módulo base64 con import base64, puedes utilizar el base64.b64encode()Función. Recuerda decodificar primero el hexadecimal como indica la descripción del desafío. **

En este caso nos topamos con la necesidad de importar base64 en python, para hacer uso de su funcion de encode y asi hallar la flag.

``` python
#!/usr/bin/env python3
import base64

hexa = "72bca9b68fc16ac7beeb8f849dca1d8a783e8acf9679bf9269f7bf"
hexa_to_bytes = bytes.fromhex(hexa)

print(base64.b64encode(hexa_to_bytes).decode('utf-8'))
```

``` python
[cesar@parrot]─[~/Downloads]
└──╼ $python3 sol.py 
crypto/Base+64+Encoding+is+Web+Safe/
```

