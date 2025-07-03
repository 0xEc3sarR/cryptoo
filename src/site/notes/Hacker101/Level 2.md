---
{"dg-publish":true,"permalink":"/hacker101/level-2/","dgPassFrontmatter":true,"noteIcon":""}
---


Reto: Encontrar 4 flagts:

# Primera flag encontrada:
- Nos damos cuenta que podemos editar el numero de paginas en la url para ver cuantas paginas hay:

https://21d82fcfea1bb0d62400bd9fb0797645.ctf.hacker101.com/page/7

En la pagina numero 7, nos dice que hay un contenido el cual no tenemos permisos para poder visualizar.

![Pasted image 20250526193439.png](/img/user/Imagenes/Pasted%20image%2020250526193439.png)

Pero si recordamos cuando entramos a una pagina que ya estaba ahi como el Markdown.

![Pasted image 20250526193557.png](/img/user/Imagenes/Pasted%20image%2020250526193557.png)

Nos sale que podemos editarla **Edit this page**.

- Entonces si intentamos editar la pagina que no podemos visualizar porque no tenemos permisos podriamos encontrar algo:
 ![Pasted image 20250526193809.png](/img/user/Imagenes/Pasted%20image%2020250526193809.png)

FLAG: **^FLAG^9400206f05ea24fedf2e9d86ffe531b16779c14aac9086ece8ff0a2e9b6b0a0b$FLAG$**


# Segunda flag encontrada:

- Una de las cosas que podemos revisar es si la pagina es vulnerable a Cross-Site Scripting(XSS).
Podemos probar editando o creando un nuevo archivo e insertar un payload malicioso.

```
<script>alert(1);</script>
```


![Pasted image 20250526194619.png](/img/user/Imagenes/Pasted%20image%2020250526194619.png)

Vemos que nos permite colocar el payload:

![Pasted image 20250526194657.png](/img/user/Imagenes/Pasted%20image%2020250526194657.png)

Si retrocedemos con **Go Home**

![Pasted image 20250526194731.png](/img/user/Imagenes/Pasted%20image%2020250526194731.png)

FLAG: **^FLAG^0460f36941119d51e05c8b4fb28af1699edba4d367086b3b1d28b2f8a24fd8ad$FLAG$**

# Tercera flag encontrada:

Busquemos alguna injeccion SQL en la url:

- Probemos primero esto:

https://21d82fcfea1bb0d62400bd9fb0797645.ctf.hacker101.com/page/1'

Nos sale lo siguiente:

![Pasted image 20250526204040.png](/img/user/Imagenes/Pasted%20image%2020250526204040.png)

Y si lo programos con el **/edit**:
https://21d82fcfea1bb0d62400bd9fb0797645.ctf.hacker101.com/page/edit/1'

Nos salio la flag:
![Pasted image 20250526204132.png](/img/user/Imagenes/Pasted%20image%2020250526204132.png)

Por que sucede esto?:
- Recordemos que una injeccion SQL ocurre cuando una aplicacion web inserta directamente datos proporcionados por el usuario en una consulta SQL sin validarlos ni escaparlos correctamente, lo que permite al atacante manipular o ejecutar comandos SQL arbitrarios.

En este caso, estoy accediendo a la url: 

```
https://21d82fcfea1bb0d62400bd9fb0797645.ctf.hacker101.com/page/edit/1'
```

Esto ocasiona un error, ya que la comilla extra genera una injeccion maliciosa porque el input no esta siendo protegido. Esta es la forma mas basica de reconocer si una pagina web es vulnerable a SQL injeccion o no.

```
SELECT * FROM pages WHERE id = '1'';
```

Aqui se puede observar como se rompe la consulta y por lo tanto hemos hecho una buena injeccion y nos proporcionaron la FLAG: **^FLAG^bbf198b41dffe173f994c31b13f6dcf1877a2ce24caf4aea9a28ca04ec4a59b3$FLAG$**

# Cuarta flag encontrada:

Tenemos lo siguiente:

![Pasted image 20250526205745.png](/img/user/Imagenes/Pasted%20image%2020250526205745.png)

Un boton, como la pagina en si se centro en xss y sql, eso nos confirma que hay javascript, por lo tanto podemos hacer funconar el boton, probemos con un **onclick="alert(1)"**.

Y luego de eso si inspeccionamos la pagina, observamos que obtenemos la flag:

![Pasted image 20250526205958.png](/img/user/Imagenes/Pasted%20image%2020250526205958.png)

FLAG: **^FLAG^8071d454a14df3466e6a89124ca951eb92f34cd59cbffdfdaa8ecbd064c63666$FLAG$**

thanks.


