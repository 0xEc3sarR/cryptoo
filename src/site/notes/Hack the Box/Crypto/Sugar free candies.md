---
{"dg-publish":true,"permalink":"/hack-the-box/crypto/sugar-free-candies/","dgPassFrontmatter":true}
---


Para empezar el reto tenemos el siguiente codigo que nos proporciona el reto:

```
from Crypto.Util.number import bytes_to_long

FLAG = open("flag.txt", "rb").read()

step = len(FLAG) // 3
candies = [bytes_to_long(FLAG[i:i+step]) for i in range(0, len(FLAG), step)]

cnd1, cnd2, cnd3 = candies

with open('output.txt', 'w') as f:
    f.write(f'v1 = {cnd1**3 + cnd2**2 + cnd1}\n')
    f.write(f'v4 = {cnd1 + cnd2 + cnd3}\n')
+ cnd3**2 + cnd2}\n')
    f.write(f'v2 = {cnd2**3 + cnd1**2 + cnd3}\n')
    f.write(f'v3 = {cnd3**3 
```

Y ahora tenemos el siguiente output.txt:

```
v1 = 1181239096013650837744125294978177790419553719590172794906535790528758829840751110126012179328061375399196613652870424327167341710919767887891371258453
v2 = 2710472017687233737830986182523923794327361982506952801148259340657557362009893794103841036477555389231149721438246037558380601526471290201500759382599
v3 = 3448392481703214771250575110613977019995990789986191254013989726393898522179975576074870115491914882384518345287960772371387233225699632815814340359065
v4 = 396216122131701300135834622026808509913659513306193
```

Examinamos el codigo:

- Vemos que existe un archivo flag.txt el cual es leido por el codigo python, luego este mismo abre el archivo como binario "rb" (read binary), lo cual significa que el contenido se leera como una secuencia de bytes (bytes), no como una cadena de texto (str) y es asignado a la variable "FLAG". 
```
FLAG = open("flag.txt", "rb").read()
```

- Podemos afirmar que la variable FLAG es de tipo byte y len(FLAG) retorna el numero de bytes, no de caracteres (aunque en ASCII puro son equivalentes).
- Imaginemos que en "flag.txt" estaba escrito 'cesar', entonces la variable FLAG = b'cesar', este prefijo "b''" indica una secuecia de bytes crudos, no es una cadena de texto.

Como se representa esto internamente?
- Cada letra en "cesar" es un byte en la codificacion ASCII:

| Letra | ASCII(decimal) | Hex  |
| ----- | -------------- | ---- |
| c     | 99             | 0x63 |
| e     | 101            | 0x65 |
| s     | 115            | 0x73 |
| a     | 97             | 0x61 |
| r     | 114            | 0x72 |

Ahora si intentamos nosotros la operacion de: 
```
step = len(FLAG) // 3
```

- FLAG = b'cesar' y len(FLAG) sera 5, porque hay 5 bytes:
	- c-e-s-a-r -> cada uno 1 byte en ASCII
- step = 5 // 3 = 1 -> ya que '//' te redondea al entero mas cercano hacia abajo

Si nos vamos a ver la variable 'candies':
```
candies = [bytes_to_long(FLAG[i:i+step]) for i in range(0, len(FLAG), step)]
```

- Observamos que se hace uso de 'bytes_to_long' de la librearia **from Crypto.Util.number import bytes_to_long** y a la par el 'slicing'

	Que es el slicing?
	- En python es una forma de extraer una porcion (subparte) de una secuencia como una lista, cadena (str), tupla o bytes.
		Sintaxis basica: secuencia[inicio:fin]
		devuelve una sub-secuencia desde el indice **inicio** hasta el indice **fin - 1** 

Ejemplo:
```
s = 'cesar'
print(s[1:4]) # resultado: "esa"
```

- Se empieza desde el indice 1 (letra "e")
- Termina justo antes del indice 4 (letra "a")

Tambien es posible usar un paso (step):
- s[inicio:fin:paso] -> Esto permite por ejemplo saltarte elementos:

```
s = "cesar"
print(s[::2]) # resultado: "csr" (salta uno de por medio)
```

	Con bytes:
```
	b = b"flag{cesar}"
	print(b[0:4]) # resultado: b"flag"
```

	Slicing en bucles
- Muy util cuando quieres dividir algo en partes iguales:

```
for i in range(0, len(data), 5):
	bloque = data[i:i+5]
```

	Casos especiales:
```
	s[:n] # Desde el principio hasta el indice n-1
	s[n:] # Desde el indice n hasta el final
	s[::-1] # Inversion de la secuencia
```


| Sintaxis | Significado                   |
| -------- | ----------------------------- |
| s[a:b]   | Del indice 'a' hasta 'b-1'    |
| s[:b]    | Desde el inicio hasta 'b-1'   |
| s[a:]    | Desde el 'a' hasta el final   |
| s[a:b:c] | De 'a' a 'b-1', en pasos de c |

Por lo tando podemos entender que el codigo de 'candies' divide la flag en 3 bloques de 21 bytes cada uno, y los convierte a enteros grandes, que se guardan en la lista.

Ejemplo:
	HTB{solving_equations_for_parts_of_the_flag_over_the_integers!}

```
HTB{solving_equations # [0:21] va desde el 0 hasta el 20 (el 21 no se incluye)
_for_parts_of_the_fla # [21:42] va desde el 21 hasta el 41 (el 42 no se incluye)
g_over_the_integers!} # [42:63] va desde el 42 hasta el 62 (el 63 no se incluye)
```

Luego estos valores son dividios en 3 diferentes variables, cada una toma un tercio del bloque:

```
cnd1, cnd2, cnd3 = candies # cnd1 = 1/3, cnd2 = 1/3, cnd3 = 1/3 = 3/3 de la flag
```

Ahora tenemos la ultima parte del codigo que consiste en abrir el archivo que escribir las operaciones con las cuales se mostrara en el output.txt:

```
with open('output.txt', 'w') as f:
    f.write(f'v1 = {cnd1**3 + cnd3**2 + cnd2}\n')
    f.write(f'v2 = {cnd2**3 + cnd1**2 + cnd3}\n')
    f.write(f'v3 = {cnd3**3 + cnd2**2 + cnd1}\n')
    f.write(f'v4 = {cnd1 + cnd2 + cnd3}\n')
```

Empezamos revisando que **with open('output.txt', 'w') as f:**
- Aqui se abre el archivo output.txt en otras palabras creara el archivo si no esta creado y empezara a escribir en el, de lo contrario borra su contenido anterior y lo abre para escribrir desde cero.
- **f.write** : instruccion que sive para escribir texto dentro de ese archivo en este caso se ejecuta varias y cada una con una respectiva operacion.

Veamos un ejemplo como termina el output.txt:

- Digamos que cnd1 = 10, cnd2 = 20, cnd3 = 30, en archivo tendria algo asi:

```
v1 = 10**3 + 30**2 + 20
v2 = 20**3 + 10**2 + 30
v3 = 30**3 + 20**2 + 10
v4 = 10 + 20 + 30
```

Pero calculado seria asi:

```
v1 = 1000 + 900 + 20 = 1920
v2 = 8000 + 100 + 30 = 8130
v3 = 27000 + 400 + 10 = 27410
v4 = 60
```

# Conclusion del reto:

- Consiste en recuperar los valores originales de cnd1, cnd2 y cnd3 a partir de:

$$
v1 = cnd1^2 + cnd3^2 + cnd2
$$
$$v2 = cnd2^3 + cnd1^2 + cnd3$$
$$v3 = cnd3^3 + cnd2^2 + cnd1$$
$$v4 = cnd1 + cnd2 + cnd3$$

En el output.txt se encuentran los valores numericos de v1, v2, v3 y v4.

# Solucion:

Para hacer la ecuacion lineal mas simple:
- La ecuacion:
$$v4 = x + y + z$$
- nos permite expresar una variable en funcion de las otras dos. Por ejemplo:

$$z = v4 - x - y$$
- Esto es util porque podemos sustituir **z** en las otras ecuaciones para reducir el sistema de 3 variables a solo 2 variables.

## Vamos a sustituir:

- Ecuacion 1:
$$v1 = x^2 + (v4 - x - y)^2 + y$$

- Ecuacion 2:
$$v2 = y^3 + x^2 + (v4 - x - y)$$
- Ecuacion 3:
$$v3 = (v4 - x - y)^3 + y^2 + x$$

