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

