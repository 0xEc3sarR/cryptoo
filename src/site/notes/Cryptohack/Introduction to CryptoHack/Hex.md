---
{"dg-publish":true,"permalink":"/cryptohack/introduction-to-crypto-hack/hex/","dgPassFrontmatter":true,"noteIcon":""}
---

![Pasted image 20250703042003.png](/img/user/Pasted%20image%2020250703042003.png)

La explicacion del problema se encuentra en el codigo:
- El prblema nos proporciona:
		En Python, el **bytes.fromhex()**. La función se puede utilizar para convertir hexadecimales a bytes. **.hex()** Se puede llamar al método de instancia en cadenas de bytes para obtener la representación hexadecimal. 
``` python
#!/usr/bin/env python3

# Problema:
# A veces al cifrar algo, el texto cifrado suele contener bytes que no son caracteres ASCII imprimibles. 
# Si queremos compartir nuestros datos cifrados, es habitual codificarlos en un formato mas intuitivo y portatil entre diferentes sistemas.
# El sistema hexadecimal se puede usar para representar cadenas ASCII. Primero, cada letra se convierte en un numero ordinal segun la tabla ASCII
# Luego los numeros decimales se convierten a numeros de base 16, tambien conocidos como hexadecimales. Los numeros se pueden combinar para 
# formar una cadena larga.

# Por lo tanto se nos incluto una bandera codificada como cadena hexadecimal. Debemos decodificarla en bytes para obtener la bandera.

hexa = "63727970746f7b596f755f77696c6c5f62655f776f726b696e675f776974685f6865785f737472696e67735f615f6c6f747d"

hexa_to_bytes = bytes.fromhex(hexa)


print("Imprimir la secuencia de bytes")
print(hexa_to_bytes)
print('\n')
print("Imprimr secuencia de bytes a una cadena UTF-8 en python")
print(hexa_to_bytes.decode('utf-8'))

```
Algo que agregamos al codigo es la decodificacion, una vez que hemos decodificado y obtenemos la secuencia de bytes nos saldra algo asi.

``` python
Imprimir la secuencia de bytes
b'crypto{You_will_be_working_with_hex_strings_a_lot}'
```

Pero por gusto para mi, le agrego **.decode('utf-8')**

``` python
Imprimr secuencia de bytes a una cadena UTF-8 en python
crypto{You_will_be_working_with_hex_strings_a_lot}
```

Esto es muy imporatante porque a veces en algunos ejercicios de criptografia, los retos devuelven datos cifrados o ofuscados en bytes, y si no lo decodificas correctamente: 
- Veras basura como **b'\x68\x65\x6c\x6c\x6f'**
- o errores si los tratas como strings directamente.





