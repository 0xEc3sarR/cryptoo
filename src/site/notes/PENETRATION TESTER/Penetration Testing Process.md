---
{"dg-publish":true,"permalink":"/penetration-tester/penetration-testing-process/","dgPassFrontmatter":true}
---

Si queremos practicas en entornos reales que nos puden servir en el futuro, podemos empezar con plataformar como https://hackerone.com/directory/programs (HackerOne) y https://bugcrowd.com/engagements?category=bug_bounty&page=1&sort_by=promoted&sort_direction=desc (Bugcrowd).

## Programa de estudios de la ruta del probador de penetración 

```
El plan simula una prueba de penetración contra la empresa Inlanefreight, dividida en varias etapas, que abarcan los conceptos y herramientas clave que nos diferenciarán como expertos en pruebas de penetración. El plan culmina con un módulo detallado sobre habilidades interpersonales críticas como la toma de notas, la organización, la documentación, la generación de informes y la comunicación con el cliente, y posteriormente con un simulacro de prueba de penetración completo para practicar todas nuestras habilidades en una gran red empresarial simulada. Los módulos que componen el plan se detallan a continuación: 
```

# Introduction
1. Proceso de prueba de penetración
2. Primeros pasos
# Reconnaissance, Enumeration & Attack Planning
1. Enumeración de red con Nmap
2. 4. Huella
3. Recopilación de información - Edición web
4. Evaluación de vulnerabilidad
5. Transferencias de archivos
6. Proyectiles y cargas útiles
7. Uso del marco Metasploit
# Exploitation & Lateral Movement
1. Ataques de contraseña
2. Ataque a los servicios comunes
3. Pivote, tunelización y reenvío de puertos
4. Enumeración y ataques de Active Directory
# Web Exploitation
1. Uso de servidores proxy web
2. Ataque de aplicaciones web con Ffuf
3. Fuerza bruta de inicio de sesión
4. Fundamentos de la inyección SQL
5. Fundamentos de SQLMap
6. Secuencias de comandos entre sitios (XSS)
7. Inclusión de archivos
8. Ataques de carga de archivos
9. Inyecciones de comando
10. Ataques web
11. Ataques a aplicaciones comunes
# Post-Exploitation
1. Escalada de privilegios de Linux
2. Escalada de privilegios de Windows
# Reporting & Capstone
1. Documentación e informes
2. Ataques a redes empresariales 


![Pasted image 20250702043529.png](/img/user/Pasted%20image%2020250702043529.png)

Todos los pasos que tomamos para explotar las vulnerabilidades se basan en la información que recopilamos sobre nuestros objetivos. Esta fase puede considerarse la piedra angular de cualquier prueba de penetración. Podemos obtener la información necesaria de diversas maneras. Sin embargo, podemos dividirla en las siguientes categorías:

    Inteligencia de fuentes abiertas
    Enumeración de infraestructura
    Enumeración de servicios
    Enumeración de hosts


Cualquier análisis puede ser muy complejo, ya que muchos factores diferentes y sus interdependencias juegan un papel importante. Además de trabajar con tres tiempos diferentes (pasado, presente y futuro) durante cada análisis, el origen y el destino juegan un papel importante. Existen cuatro tipos de análisis:

![Pasted image 20250702044723.png](/img/user/Pasted%20image%2020250702044723.png)


# Movimiento Lateral:

``` ruby
Si todo saliera bien y pudiéramos penetrar la red corporativa ( Exploitation) con éxito, recopilar información almacenada localmente y aumentar nuestros privilegios ( Post-Exploitation), a continuación entramos en el Lateral MovementEtapa. El objetivo es probar qué podría hacer un atacante dentro de toda la red. Al fin y al cabo, el objetivo principal no es solo explotar con éxito un sistema público, sino también obtener datos confidenciales o encontrar todas las maneras en que un atacante podría inutilizar la red. Uno de los ejemplos más comunes es el ransomware . Si un sistema de la red corporativa se infecta con ransomware, este puede propagarse por toda la red. Bloquea todos los sistemas mediante diversos métodos de cifrado, dejándolos inutilizables para toda la empresa hasta que se introduzca una clave de descifrado.
```

En esta etapa, queremos comprobar hasta dónde podemos llegar manualmente en toda la red y qué vulnerabilidades internas podemos encontrar que puedan explotarse. Para ello, repasaremos varias fases:

    Pivoteando
    Pruebas evasivas
    Recopilación de información
    Evaluación de vulnerabilidad
    Explotación (de privilegios)
    Post-explotación


### Pivoteando

En la mayoría de los casos, el sistema que utilizamos no cuenta con las herramientas necesarias para enumerar la red interna de forma eficiente. Algunas técnicas nos permiten usar el host atacado como proxy y realizar todos los análisis desde nuestra máquina o VM de ataque. De esta forma, el sistema atacado representa y enruta todas las solicitudes de red enviadas desde nuestra máquina de ataque a la red interna y sus componentes.

De esta manera, nos aseguramos de que las redes no enrutables (y, por lo tanto, inaccesibles públicamente) aún puedan accederse. Esto nos permite analizarlas en busca de vulnerabilidades y penetrar más profundamente en la red. Este proceso también se conoce como Pivoting o Tunneling.

Un ejemplo básico podría ser que tengamos una impresora en casa a la que no se puede acceder desde Internet, pero podemos enviar trabajos de impresión desde nuestra red doméstica. Si uno de los hosts de nuestra red doméstica ha sido comprometido, podría aprovecharse para enviar estos trabajos a la impresora. Aunque este es un ejemplo simple (e improbable), ilustra el objetivo de pivoting, que consiste en acceder a sistemas inaccesibles a través de un sistema intermediario.
### Pruebas evasivas

Además, en esta etapa, debemos considerar si las pruebas evasivas forman parte del alcance de la evaluación. Existen diferentes procedimientos para cada táctica, lo que nos ayuda a disimular estas solicitudes para no generar alarma interna entre los administradores y el equipo azul.

Hay muchas formas de protegerse contra el movimiento lateral, incluida la red (micro) segmentation, threat monitoring, IPS/ IDS, EDRPara evitarlos eficazmente, necesitamos comprender cómo funcionan y a qué responden. Luego, podemos adaptar y aplicar métodos y estrategias que ayuden a evitar su detección.
### Recopilación de información

Antes de apuntar a la red interna, primero debemos obtener una overviewDe qué sistemas y a cuántos se puede acceder desde nuestro sistema. Es posible que esta información ya esté disponible desde la última etapa de postexplotación, donde analizamos con más detalle la configuración del sistema.

Regresamos a la etapa de Recopilación de Información, pero esta vez lo hacemos desde dentro de la red con una perspectiva diferente. Una vez detectados todos los hosts y servidores, podemos enumerarlos individualmente.
### Evaluación de vulnerabilidad

La evaluación de vulnerabilidades desde el interior de la red difiere de los procedimientos anteriores. Esto se debe a que se producen muchos más errores dentro de una red que en los hosts y servidores expuestos a Internet. En este caso, groupsa la que se ha asignado y la rightsLos diferentes componentes del sistema desempeñan un papel esencial. Además, es habitual que los usuarios compartan información y documentos y trabajen en ellos conjuntamente.

Este tipo de información nos resulta especialmente interesante al planificar nuestros ataques. Por ejemplo, si comprometemos una cuenta de usuario asignada a un grupo de desarrolladores, podríamos obtener acceso a la mayoría de los recursos utilizados por los desarrolladores de la empresa. Esto probablemente nos proporcionará información interna crucial sobre los sistemas y podría ayudarnos a identificar vulnerabilidades o a ampliar nuestro acceso.
### Explotación (de privilegios)

Una vez que hayamos encontrado y priorizado estas rutas, podemos pasar al paso en el que las usaremos para acceder a los demás sistemas. A menudo encontramos maneras de descifrar contraseñas y hashes y obtener mayores privilegios. Otro método estándar es usar nuestras credenciales existentes en otros sistemas. También habrá situaciones en las que ni siquiera tengamos que descifrar los hashes, sino que podamos usarlos directamente. Por ejemplo, podemos usar la herramienta Responder para interceptar hashes NTLMv2. Si podemos interceptar un hash de un administrador, entonces podemos usar... pass-the-hashtécnica para iniciar sesión como ese administrador (en la mayoría de los casos) en varios hosts y servidores.

Después de todo, el Lateral MovementLa etapa busca avanzar a través de la red interna. Los datos y la información existentes pueden ser versátiles y, a menudo, se utilizan de diversas maneras.
### Post-explotación

Una vez alcanzado uno o más hosts o servidores, repasamos los pasos de la etapa de postexplotación para cada sistema. Aquí, recopilamos de nuevo información del sistema, datos de los usuarios creados e información empresarial que pueda presentarse como prueba. Sin embargo, debemos considerar de nuevo cómo debe gestionarse esta información y las normas definidas en el contrato sobre datos sensibles.

Finalmente, estamos listos para pasar a la Proof-of-Conceptfase para mostrar nuestro arduo trabajo y ayudar a nuestro cliente, y los responsables de la remediación a reproducir eficientemente nuestros resultados. 

# Proof-of-Concept

Es un término de gestión de proyectos. En la gestión de proyectos, sirve como prueba de que un proyecto es factible en principio. Los criterios para ello pueden residir en factores técnicos o comerciales. Por lo tanto, constituye la base para el trabajo posterior; en nuestro caso, los pasos necesarios para asegurar la red corporativa mediante la confirmación de las vulnerabilidades detectadas. En otras palabras, sirve como base para la toma de decisiones sobre el curso de acción posterior. Al mismo tiempo, permite identificar y minimizar los riesgos. 

# Post-Engagement

Al igual que existe un trabajo preliminar considerable antes del inicio oficial de una colaboración (cuando comienzan las pruebas), debemos realizar numerosas actividades (muchas de ellas contractualmente vinculantes) tras la finalización de los escaneos, la explotación, el movimiento lateral y las actividades posteriores a la explotación. Cada colaboración es única, por lo que estas actividades pueden variar ligeramente, pero generalmente deben realizarse para concluirla por completo. 


### Limpieza

Una vez finalizadas las pruebas, debemos realizar cualquier limpieza necesaria, como eliminar herramientas/scripts cargados en los sistemas de destino, revertir cualquier cambio (menor) de configuración que hayamos realizado, etc. Debemos tener notas detalladas de todas nuestras actividades, lo que facilita y agiliza cualquier limpieza. Si no podemos acceder a un sistema donde se debe eliminar un artefacto o revertir otro cambio, debemos avisar al cliente e incluir estos problemas en los apéndices del informe. Incluso si podemos eliminar los archivos cargados y revertir los cambios (como agregar una cuenta de administrador local), debemos documentar estos cambios en los apéndices del informe por si el cliente recibe alertas que deban seguir y confirmar que la actividad en cuestión formó parte de nuestras pruebas autorizadas.
### Documentación e informes

Antes de completar la evaluación y desconectarnos de la red interna del cliente o enviar correos electrónicos de notificación de "detención" para indicar el fin de las pruebas (es decir, el fin de la interacción con los hosts del cliente), debemos asegurarnos de contar con la documentación adecuada de todos los hallazgos que planeamos incluir en nuestro informe. Esto incluye la salida de comandos, capturas de pantalla, una lista de los hosts afectados y cualquier otra información específica del entorno del cliente o del hallazgo. También debemos asegurarnos de haber recuperado todos los resultados de análisis y registros si el cliente alojó una máquina virtual en su infraestructura para una prueba de penetración interna, así como cualquier otro dato que pueda incluirse en el informe o como documentación complementaria. No debemos conservar ninguna información de identificación personal (PII), información potencialmente incriminatoria ni otros datos confidenciales que hayamos encontrado durante las pruebas.

Ya deberíamos tener una lista detallada de los hallazgos que incluiremos en el informe y todos los detalles necesarios para adaptarlos al entorno del cliente. Nuestro informe (que se detalla en el módulo Documentación e Informes ) debería constar de lo siguiente:

    Una cadena de ataque (en caso de compromiso interno total o externo al acceso interno) que detalla los pasos tomados para lograr el compromiso
    Un resumen ejecutivo sólido que una audiencia sin conocimientos técnicos pueda comprender
    Hallazgos detallados específicos del entorno del cliente que incluyen una calificación de riesgo, el impacto de los hallazgos, recomendaciones de remediación y referencias externas de alta calidad relacionadas con el problema.
    Pasos adecuados para reproducir cada hallazgo para que el equipo responsable de la remediación pueda comprender y probar el problema mientras implementa soluciones.
    Recomendaciones a corto, mediano y largo plazo específicas para el medio ambiente.
    Apéndices que incluyen información como el alcance del objetivo, datos OSINT (si son relevantes para el compromiso), análisis de descifrado de contraseñas (si es relevante), puertos/servicios descubiertos, hosts comprometidos, cuentas comprometidas, archivos transferidos a sistemas propiedad del cliente, cualquier creación de cuenta/modificaciones del sistema, un análisis de seguridad de Active Directory (si es relevante), datos de escaneo relevantes/documentación complementaria y cualquier otra información necesaria para explicar un hallazgo o recomendación específicos con más detalle.

En esta etapa, crearemos un borrador del informe, que será el primer entregable que recibirá nuestro cliente. A partir de aquí, podrá comentarlo y solicitar las aclaraciones o modificaciones necesarias.
### Reunión de revisión de informes

Una vez entregado el borrador del informe y tras la distribución interna del cliente y su revisión a fondo, se suele celebrar una reunión de revisión para analizar los resultados de la evaluación. Esta reunión suele contar con la participación de las mismas personas, tanto del cliente como de la firma que realiza la evaluación. Dependiendo del tipo de hallazgos, el cliente puede incorporar expertos técnicos adicionales si el hallazgo está relacionado con un sistema o aplicación de su responsabilidad. Normalmente, no leemos el informe completo palabra por palabra, sino que repasamos brevemente cada hallazgo y ofrecemos una explicación desde nuestra propia perspectiva/experiencia. El cliente tendrá la oportunidad de hacer preguntas sobre cualquier aspecto del informe, solicitar aclaraciones o señalar problemas que deban corregirse. A menudo, el cliente presenta una lista de preguntas sobre hallazgos específicos y no desea cubrir cada uno en detalle (como los de bajo riesgo).
### Aceptación de entregables

El Alcance del Trabajo debe definir claramente la aceptación de cualquier entregable del proyecto. En las evaluaciones de pruebas de penetración, generalmente, entregamos un informe marcado DRAFTy dar al cliente la oportunidad de revisarlo y comentarlo. Una vez que el cliente haya enviado sus comentarios (es decir, respuestas de la gerencia, solicitudes de aclaración/cambios, evidencia adicional, etc.), ya sea por correo electrónico o (idealmente) durante una reunión de revisión del informe, podemos emitirle una nueva versión del informe marcada FINALAlgunas firmas de auditoría con las que los clientes pueden estar en deuda no aceptarán un informe de prueba de penetración con un DRAFTDesignación. A otras empresas no les importará, pero lo mejor es mantener un enfoque uniforme para todos los clientes.
### Pruebas posteriores a la remediación

La mayoría de los proyectos incluyen pruebas posteriores a la remediación como parte del costo total del proyecto. En esta fase, revisaremos cualquier documentación proporcionada por el cliente que muestre evidencia de la remediación o simplemente una lista de los hallazgos corregidos. Necesitaremos acceder nuevamente al entorno objetivo y probar cada problema para garantizar que se haya corregido correctamente. Emitiremos un informe posterior a la remediación que muestre claramente el estado del entorno antes y después de las pruebas. Por ejemplo, podemos incluir una tabla como la siguiente: 

![Pasted image 20250702050339.png](/img/user/Pasted%20image%2020250702050339.png)


# Pasos de práctica

Piensa en las habilidades que has adquirido y qué es lo que más te interesa de ellas. A partir de ahí, podemos seleccionar algunos módulos adicionales para ampliar nuestros conocimientos, máquinas para practicar y Prolabs o Endgames para ponernos a prueba. Los números a continuación son un buen ejemplo inicial:

    2x Módulos
    3x Máquinas retiradas
    5x Máquinas activas
    1x Laboratorio profesional/Fin del juego


## Módulos 

![Pasted image 20250702050919.png](/img/user/Pasted%20image%2020250702050919.png)

## Máquinas retiradas

Cuando hayamos completado (al menos) dos módulos y estemos satisfechos con nuestras notas y documentación, podemos seleccionar tres máquinas retiradas diferentes. Estas también deberían tener diferente dificultad, pero recomendamos elegir two easy y one mediumMáquinas. Al final de cada módulo, encontrará máquinas retiradas recomendadas que le ayudarán a practicar las herramientas y los temas específicos que se tratan en el módulo. Estos hosts compartirán uno o más vectores de ataque relacionados con el módulo.

Con las máquinas retiradas, tenemos la ventaja significativa de poder encontrar artículos en línea de diversos autores (cada uno con enfoques diferentes) con los que comparar nuestras notas. Si optamos por adquirir una membresía VIP en la plataforma principal de HTB, también tendremos acceso a artículos oficiales de HTB que presentan otro punto de vista y, a menudo, incluyen algunas consideraciones defensivas. Podemos usar estos artículos para comprobar si hemos anotado todo lo necesario y si no hemos pasado nada por alto. El orden en el que podemos proceder a practicar con las máquinas retiradas es similar al siguiente: 

![Pasted image 20250702051023.png](/img/user/Pasted%20image%2020250702051023.png)

## Máquinas activas

Después de construir una buena base con los módulos y las máquinas retiradas, podemos aventurarnos a two easy, two medium, y one hardMáquina activa. También podemos consultar las recomendaciones del módulo correspondiente al final de cada módulo en Academy.

La ventaja de este método es que simulamos una situación lo más realista posible utilizando un único host con el que no estamos familiarizados y del que no podemos encontrar documentación (enfoque de caja negra). Mientras la máquina permanezca activa, no se publicarán informes oficiales. Esto significa que no podemos comprobar si tenemos todo o si nos hemos saltado algo de alguna fuente oficial. Esto nos obliga a confiar en nosotros mismos y en nuestras capacidades. Los pasos ideales para máquinas activas serían los siguientes: 

![Pasted image 20250702051201.png](/img/user/Pasted%20image%2020250702051201.png)

Para mas informacion sobre la redaccion de informes tenemos: https://academy.hackthebox.com/module/details/162


## Pro Lab/Fin del juego

Una vez que nos sintamos cómodos trabajando con hosts individuales y documentando nuestros hallazgos, podremos abordar los Prolabs y los Endgames. Estos laboratorios son grandes entornos multihost que a menudo simulan redes empresariales de diversos tamaños, similares a las que podríamos encontrar durante las pruebas de penetración reales para nuestros clientes. Esto nos presentará desafíos diferentes a los que estamos acostumbrados. 

## Concluyendo

Hemos cubierto una cantidad considerable de información en este módulo. Si recién estás comenzando... Penetration TesterEn cuanto a la ruta de rol laboral, recomendamos continuar en el orden en que se presentan los módulos. Si es nuevo en esto, omitir algunos módulos podría generar lagunas de conocimiento y dificultar la finalización de ciertos módulos sin los conocimientos previos necesarios. Si ya ha completado parcialmente la ruta, conviene repasar los módulos que ya ha completado y considerar los distintos pasos en el contexto del proceso de pruebas de penetración que se presenta en este módulo.

La práctica y la mejora continuas son vitales, independientemente de la etapa en la que se encuentre. Podemos mejorar continuamente nuestra metodología actual, aprender cosas diferentes y aprender nuevos conceptos. El campo de las tecnologías de la información cambia rápidamente. Se descubren nuevos ataques con frecuencia, y necesitamos estar al tanto de las últimas y mejores estrategias de acción para ser lo más eficaces posible y proporcionar a nuestros clientes la información necesaria para proteger sus entornos de un panorama de amenazas en constante evolución. Nunca deje de aprender y mejorar. Desafíese a diario. Tómese descansos. Disfrute del proceso y no olvide... Think Outside The Box! 

![Pasted image 20250702051449.png](/img/user/Pasted%20image%2020250702051449.png)

