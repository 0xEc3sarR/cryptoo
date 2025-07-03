---
{"dg-publish":true,"permalink":"/hack-the-box/maquinas/medium/bastard/","dgPassFrontmatter":true,"noteIcon":""}
---

Empezamos hacineco un nmap para poder ver los puertos que estan abiertos.

![Pasted image 20250523190316.png](/img/user/Imagenes/Pasted%20image%2020250523190316.png)

Los puertos abiertos relevantes que tenemos son:

80 -> detras existe una pagina web
135 -> smb
49154 

Accedemos al puerto 80, para ver que hay en la pagina web:

![Pasted image 20250523190433.png](/img/user/Imagenes/Pasted%20image%2020250523190433.png)

Si hacemos un whatweb:

```
└─$ whatweb 10.10.10.9
http://10.10.10.9 [200 OK] Content-Language[en], Country[RESERVED][ZZ], Drupal, HTTPServer[Microsoft-IIS/7.5], IP[10.10.10.9], JQuery, MetaGenerator[Drupal 7 (http://drupal.org)], Microsoft-IIS[7.5], PHP[5.3.28,], PasswordField[pass], Script[text/javascript], Title[Welcome to Bastard | Bastard], UncommonHeaders[x-content-type-options,x-generator], X-Frame-Options[SAMEORIGIN], X-Powered-By[PHP/5.3.28, ASP.NET]
```

Vemos que el CMS que esta corriendo en la pagina es drupal.
- Investigando sabemos que en drupal para saber su version se encuentra en el archivo: **changelog.txt**.

![Pasted image 20250523190630.png](/img/user/Imagenes/Pasted%20image%2020250523190630.png)

La version de drupa les 7.54

Si nos ponemos a investigar si existe algun exploit para explotar esta version de druapal, encontramos lo siguiente:

Encontramos el siguiente CVE: CVE-2018-7600

- Esta vulnerablilidad permite ejecucion remota de codigo (RCE) sin autenticacion.

Abrimos el msfconsole para empezar a explotar la vulnerabilidad.

```
msfconsole
```

