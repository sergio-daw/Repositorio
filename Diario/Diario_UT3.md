# Diario UT3 Servidores web (Apache y Nginx)

## 1. Qué he aprendido
Esta unidad trata sobre los servidores web Apache y Nginx. He aprendido a instalar y configurar Apache en Ubuntu, a crear un VirtualHost con su propio dominio usando `/etc/hosts`, a proteger una carpeta con usuario y contraseña, y a configurar HTTPS con un certificado autofirmado y la redirección de HTTP a HTTPS. También he instalado Nginx, he creado un server block y he visto sus diferencias con Apache, como que los sitios se activan con un enlace simbólico en lugar de con `a2ensite`.

## 2. Qué no entiendo
No entiendo del todo cómo se obtendría un certificado de Let's Encrypt con Certbot en un servidor real, porque en la práctica no se podía al no tener un dominio público.

## 3. Qué es lo que más me ha gustado y qué es lo que menos
Lo que más me ha gustado: comprobar con `curl` qué servidor responde a cada petición y conseguir que Apache y Nginx funcionen a la vez en la misma máquina, cada uno en su puerto.

Lo que menos me ha gustado: los errores por fallos pequeños al escribir comandos y rutas, como una letra mayúscula en una ruta, que cuestan de encontrar.

## 4. Qué más me gustaría saber relacionado con la Unidad
Me gustaría saber cómo se usa Nginx como proxy inverso delante de otros servidores, como Apache o Tomcat, y cómo se configura HTTPS en un servidor real con un dominio propio.
