---
{"dg-publish":true,"permalink":"/hack-the-box/crypto/very-easy/spg/","dgPassFrontmatter":true}
---

Para empezar el reto tenemos el siguiente codigo:

```
from hashlib import sha256
import string, random
from secret import MASTER_KEY, FLAG
from Crypto.Cipher import AES
from Crypto.Util.Padding import pad
from base64 import b64encode

ALPHABET = string.ascii_letters + string.digits + '~!@#$%^&*'

def generate_password():
    master_key = int.from_bytes(MASTER_KEY, 'little') # el primero es el LSB, el ultimo es el MSB
    password = ''

    while master_key:
        bit = master_key & 1
        if bit:
            password += random.choice(ALPHABET[:len(ALPHABET)//2])
        else:
            password += random.choice(ALPHABET[len(ALPHABET)//2:])
        master_key >>= 1

    return password

def main():
    password = generate_password()
    encryption_key = sha256(MASTER_KEY).digest()
    cipher = AES.new(encryption_key, AES.MODE_ECB)
    ciphertext = cipher.encrypt(pad(FLAG, 16))

    with open('output.txt', 'w') as f:
        f.write(f'Your Password : {password}\nEncrypted Flag : {b64encode(ciphertext).decode()}')

if __name__ == '__main__':
    main()
```

Procedamos a analiar el codigo:

- Primera parte del codigo: Librerias
```
from hashlib import sha256
import string, random
from secret import MASTER_KEY, FLAG
from Crypto.Cipher import AES
from Crypto.Util.Padding import pad
from base64 import b64encode
```

- **hashlib.sha256**: se usa para obtener el hash SHA-256, que sirve como clave de cifrado AES.
- **string, random**: para generar la contraseña con caracteres aleatorios.
- **secret import MASTER_KEY, FLAG**: importa dos variabels de archivo secret.py
- **Crypto.Cipher.AES**: permite usar el algoritmo de cifrado AES.
- **Crypto.Util.Padding.pad**: rellena los datos para que su longitud sea multiplo del tamaño de bloque (16 bytes).
- **base64.b64encode**: codifica datos binarios (el ciphertext) a una cadena legible.

- Segunda parte del codigo: variable ALPHABET
```
ALPHABET = string.ascii_letters + string.digits + '~!@#$%^&*'
```

**string.ascii_letters** -> Este valor viene del modulo **string** de python, contiene:
```
'abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ'
```
- Es decir que contiene todas las letras del alfabeto en ingles:
	- 26 letras minusculas
	- 26 letras mayusculas
- Total: 52 caracteres

**string.digits** -> Tambien viene del modulo string. Contiene:
```
'0123456789'
```
- Todos los digitos numericos.
- Total: 10 caracteres

Caracteres: '~!@#$%^&*' -> Esta es una cadena escrita directamente en el codigo, que incluye 10 caracteres especiales.

- Esto ayuda a crear passwords con mas complejidad.

**Total de caracteres: 26 + 26 + 10 + 10 = 72**

- Tercera parte del codigo:
```
def generate_password():
    master_key = int.from_bytes(MASTER_KEY, 'little')
    password = ''

    while master_key:
        bit = master_key & 1
        if bit:
            password += random.choice(ALPHABET[:len(ALPHABET)//2])
        else:
            password += random.choice(ALPHABET[len(ALPHABET)//2:])
        master_key >>= 1

    return password
```

Empezamos con la variable: **master_key**

- Algo a considerar es -> Que son los bytes significativos?
	- Cuando representamos un numero binario que ocupa varios bytes, cada byte representa una parte del numero. La importancia (o "significancia") de cada byte depende de su posicion.

Definicion: 
- Byte mas significativo (MSB - Most Significant Byte):
	- Es el que contribuye mas al valor total del numero.
	- Contiene los bits de mayor peso (como los millones en un numero decimal grande).
- Byte menos significatio (LSB - Least Significant Byte):
	- Es el que contribuye menos al valor total.
	- Contiene los bits de menor peso (como las unidades en un numero decimal).



```
master_key = int.from_bytes(MASTER_KEY, 'little')
```

- Convierte una secuencia de bytes (MASTER_KEY) en un numero entero (int) interpretandolos en **orden little endian**.
- La variable **MASTER_KEY** es una variable de tipo bytes y podria ser algo asi:
```
MASTER_KEY = b'\x01\x02\x03\x04'
```
- La funcion **int.from_bytes** convierte una secuencia de bytes a un numero entero.
- Sintaxis:
```
- int.from_bytes(bytes_objeto, byteorder)
```
- byteorder: puede ser **little** o **big**:
	- **little** -> El primer byte es el menos significativo (LSB)
	- **big** -> El primer byte es el mas significativo (MSB)

### Que significa 'little' endian?

- Indica como deben interpretarse los bytes en cuanto a su orden de significancia.
```
MASTER_KEY = b'\x01\x02\x03\x04'
```

- Usando:
```
int.from_bytes(MASTER_KEY, 'little')
```

Esto se interpreta asi:

| Byte | Posicion | Valor (hex) | Potencia de 256 | Valor decimal |
| ---- | -------- | ----------- | --------------- | ------------- |
| 0    | LSB      | 0x01        | 256^0           | 1             |
| 1    |          | 0x02        | 256^1           | 512           |
| 2    |          | 0x03        | 256^2           | 196608        |
| 3    | MSB      | 0x04        | 256^3           | 67108864      |
Entonces el resultado es: 1 + 512 + 196608 + 67108864 = 67305985


