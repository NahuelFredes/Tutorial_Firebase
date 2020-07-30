# Tutorial de Firebase
Firebase es una plataforma orientada al desarrollo de aplicaciones web y aplicaciones móviles, la cual nos provee diferentes herramientas que estaremos viendo a lo largo del tutorial.

## Empezar a usar Firebase en Web
*Primero debe tener creado un proyecto. Para ello debe iniciar sesion en [Firebase](https://firebase.google.com/?hl=es-419) y luego creearlo [aquí](https://console.firebase.google.com/u/0/?hl=es-419)*

Una vez creado el proyecto deberas agregarle tu aplicacion web
- En la página de descripción general del proyecto, clickeá en el ícono de Web <> para iniciar el flujo de trabajo de configuración.
- Introducís el nombre a la aplicación y das en siguiente
- Firebase te proveerá un codigo script el cual deberás copiar y pegar en tu archivo .HTML o en tu .JS

## Comenzar a utilizar Firebase en tu app Web
*Firebase es un SDK por lo que daremos un pequeño vistazo a algunas de las diferentes herramientas que este provee*

### Authentication
*Es una herramienta de gestion de usuarios, mails, contraseñas, autenticación, etc*
#### Primeros pasos
Deberás crear un formulario para que tus usuarios puedan registrarse

<pre>
<h1>Registrarse</h1>
  <input type="email" id="email" placeholder="Coloca aqui tu email" />
  <input type="password" id="pass" placeholder="Coloca aqui tu password" />
  <button onclick="enviar()" >Enviar</button>
</pre>
