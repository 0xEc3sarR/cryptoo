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

## Codigo 1:
- Ahora pasaremos a hacer el codigo, lo cual es mas sencillo ya que solamente usaremos **sage math** para hacer el calculo del sistema de ecuaciones sin hacer tantos cambios de variables.
- Pasare a darle una explicacion detallada del codigo, ya que si se entiene este poco, se puede entender sin problema con **sympy**.

```
from sage.all import *
from Crypto.Util.number import long_to_bytes

def create_variables():
    x,y,z = var('x,y,z', domain=ZZ)
    return x,y,z

def solve_system(x, y, z, v1, v2, v3, v4):
    return solve([
        x**3 + z**2 + y == v1,
        y**3 + x**2 + z == v2,
        z**3 + y**2 + x == v3,
        x + y + z == v4
        ], x, y, z, solution_dict=True)[0]

def get_flag(sols):
    return b''.join([long_to_bytes(int(n)) for n in [sols[x], sols[y], sols[z]]])

exec(open('output.txt').read())

def pwn():
    x, y, z = create_variables()
    sols = solve_system(x, y, z, v1, v2, v3, v4)
    flag = get_flag(sols)
    print(flag)


if __name__ == "__main__":
    pwn()
```

A continuacion una segunda version del codigo usando librerias como sympy, ya que a veces existe problemas con sage math, por lo tanto al final les dejare un enlace a como instalarlo en linux, ya que nativamente en windows es inestable.

## Explicacion codigo 1:

Empezamos por las librerias importadas:

```
from sage.all import *
from Crypto.Util.number import long_to_bytes
```

- Importamos todo sage math
- Importamos long_to_bytes para convertir enteros grandes a bytes (reconstruir la flag)

Primera funcion del codigo:

```
def create_variables():
    x, y, z = var('x, y, z', domain=ZZ)
    return x, y, z
```

- Definimos 3 variables simbolicas enteras x,y,z.
- Estas representaran los valores desconocidos cnd1, cnd2, cnd3.
- Sage puede trabajar con esas variables para construir ecuaciones, etc.
- var() -> es una funcion en sage math que crea variables simbolicas. 
- 'x, y, z' es una cadena de texto que le dice a sage math que quiere 3 variables llamadas, x, y, z.
```
x, y, z = var('x, y, z')
```
estamos diciendo: "dame 3 variables simbolicas reales" por defecto.
- Que significa domain=ZZ? -> Esto restringe el dominio de variables a los numeros enteros Z (de ahi ZZ en sage), ya que ZZ es el conjunto de los enteros en SageMath: Z = {..., -2, -1, 0, 1, 2,...}.

Conjuntos numericos en SageMath:

| Conjunto | Representacion      | Descripcion                                                                           |
| -------- | ------------------- | ------------------------------------------------------------------------------------- |
| $$N$$    | $$NN$$              | Números naturales (no negativos): {0,1,2,… }\{0, 1, 2, \dots\}{0,1,2,…}               |
| $$Z$$    | $$ZZ$$              | Números enteros: {…,−2,−1,0,1,2,… }\{\dots, -2, -1, 0, 1, 2, \dots\}{…,−2,−1,0,1,2,…} |
| $$Q$$    | $$QQ$$              | Números racionales                                                                    |
| $$R$$    | $$RR$$              | Números reales con precisión limitada (punto flotante)                                |
| $$R^n$$  | $$RealField(n)$$    | Reales con precisión de `n` bits                                                      |
| $$C$$    | $$CC$$              | Números complejos con precisión limitada                                              |
| $$C^n$$  | $$ComplexField(n)$$ | Complejos con precisión de `n` bits                                                   |
- Por ultimo retornamos esas varaibles ya creadas.

Segunda funcion:

```
def solve_system(x, y, z, v1, v2, v3, v4):
    return solve([
        x**3 + z**2 + y == v1,
        y**3 + x**2 + z == v2,
        z**3 + y**2 + x == v3,
        x + y + z == v4
    ], x, y, z, solution_dict=True)[0]
```

- Recibe las variables x, y, z y los valores v1, v2, v3, v4.
- Construye un sistema de 4 ecuaciones no lineales:
$$
x^3 + z^2 + y = v1​
$$
$$
y^3 + x^2 + z = v2
$$
$$
z^3 + y^2 + x = v3 
$$
$$
x + y + z = v4
$$

- Cabe recalcar que que **solve()** es la funcion de Sage para resolver ecuaciones o sistemas de ecuaciones.
- Le pasamos una lista de ecuaciones como primer argumento.
- Luego le decimos explicitamente que queremos resolver con respecto a las variables x, y, z.
-  **solution_dict=True** -> Esto le dice a sage que retorne las soluciones como un diccionario en lugar de una lista de ecuaciones.

Con **solution_dict=True** obtenes:

```
[{x: 123, y: 456, z: 789}]
```

Sin **solution_dict=True** obtienes:

```
[[x == 123, y == 456, z == 789]]
```

El formato tipo diccionario es mucho mas facil de usar programaticamente, por eso es usado.

-  Explicacion de **[0]** -> **solve()** devuelve una lista de soluciones, incluso si hay solo una.
- **[0]** accede a la primera solucion encontrada.
Entonces:

```
solve(...)[0]
```

- toma solo la primera solucion valida (a veces hay varias, pero en este caso se espera que haya una unica solucion entera valida).

Tercera funcion del codigo:

```
def get_flag(sols):
    return b''.join([long_to_bytes(int(n)) for n in [sols[x], sols[y], sols[z]]])
```

- La funcion toma una solucion del sistema de ecuaciones (el diccionario sols) que contiene valores de **x**, **y**, **z**, y:
	1. Convierte cada uno de esos numeros grandes (enteros) a bytes, usando **long_to_bytes**.
	2. Une esos bloques de bytes en un solo string de bytes con **b''.join(..)**
	3. Asi, reconstruye la **FLAG** original que estaba codificada.

## Desglose paso a paso del programa

Por ejemplo:

- Este se un diccionario con la solucion de las variables sombolicas.

```
sols = {x: 557928492384, y: 349823902832, z: 184382103920}
```

- Estos valores vienen de:

```
sols = solve_system(x, y, z, v1, v2, v3, v4)
```

**[sols[x], sols[y], sols[z]]**
- Extraemos los valores asociados a x, y, z en el oden en que fueron generados originalmente (cnd1, cnd2, cnd3).
- Por ejemplo:

```
- [557928492384, 349823902832, 184382103920]
```

**int(n)**

- En sage, los numeros simbolicos a veces no son del tipo **int** de python, sino del tipo **Integer**, Rational, etc. Por eso, para asegurar compatibilidad con **long_to_bytes**, se hace:

```
int(n)
```

- Eso garantiza que el valor sea un entero nativo de Python.

**long_to_bytes(...)**
- Esta funcion proviene de **Crypto.Util.number** y convierte un numero grange (por ejemplo: 557928492384) a su representacion en bytes:

```
>>> long_to_bytes(557928492384)
b'Hello'
```

- Asi se revierte lo que se hizo con **bytes_to_long** al encriptar la flag.

**b''.join([...])**
- Junta todos esos fragmentos de bytes en un solo bloque continuo.

```
[b'HTB{', b'cesar_', b'flag}'] → b'HTB{cesar_flag}'
```

## Codigo 2:

- Este codigo cumple con la misma funcion sin sage math, pero es un poco mas robusto por eso mismo.

```
from Crypto.Util.number import * # bytes_to_long
from sympy import symbols, solve, Eq # import *



def create_variables():
    x, y, z = symbols('x y z', integer=True)
    return x, y, z


def solve_system(v1, v2, v3, v4):
    x, y, z = create_variables()
    equations = [
        Eq(x**3 + z**2 + y, v1),
        Eq(y**3 + x**2 + z, v2),
        Eq(z**3 + y**2 + x, v3),
        Eq(x + y + z, v4),
    ]
    solutions = solve(equations, (x, y, z), dict=True)
    return solutions[0] if solutions else None

def get_flag(sols):
    x, y, z = create_variables()
    parts = [long_to_bytes(int(sols[var])) for var in [x, y, z]]
    return b''.join(parts)

def load_values():
    with open('output.txt') as f:
        data = f.read()
    return {
        'v1' : int(data.split('v1 = ')[1].split('\n')[0]),
        'v2' : int(data.split('v2 = ')[1].split('\n')[0]),
        'v3' : int(data.split('v3 = ')[1].split('\n')[0]),
        'v4' : int(data.split('v4 = ')[1])
    }

def pwn():
    values = load_values()
    solutions = solve_system(**values)
    if solutions:
        flag = get_flag(solutions)
        print(flag.decode())
    else:
        print("No se encontraron soluciones válidas")

if __name__ == '__main__':
    pwn()
```

# Resultado final: HTB{solving_equations_for_parts_of_the_flag_over_the_integers!}

