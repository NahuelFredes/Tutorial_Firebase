# Tutorial de Firebase
Firebase es una plataforma orientada al desarrollo de aplicaciones web y aplicaciones móviles, la cual nos provee diferentes herramientas que estaremos viendo a lo largo del tutorial.

## Comenzar a usar Firebase en Web
*Primero debe tener creado un proyecto. Para ello debe iniciar sesion en [Firebase](https://firebase.google.com/?hl=es-419) y luego creearlo [aquí](https://console.firebase.google.com/?hl=es)*

Una vez creado el proyecto deberas agregarle tu aplicacion web
- En la página de descripción general del proyecto, clickeá en el ícono de Web <> para iniciar el flujo de trabajo de configuración.
- Introducís el nombre a la aplicación y das en siguiente
- Firebase te proveerá un codigo script el cual deberás copiar y pegar en tu archivo .HTML o en tu .JS

## Usos principales de Firebase en tu app Web
*Firebase es un SDK por lo que daremos un pequeño vistazo a algunas de las diferentes herramientas que este provee*

### Authentication
*Es una herramienta de gestion de usuarios, mails, contraseñas, autenticación, etc*
#### Registro de Usuarios
*Deberás añadir la libreria de [authentication](https://firebase.google.com/docs/web/setup?hl=es-419#libraries_hosting-urls)*

Necesitas crear un formulario de registro para usuarios:
*Podés utilizar el siguiente (sin las comillas iniciales en cada tag)*

<pre><code>
    <'h1>Registrarse</h1>
    <'input type="email" id="email" placeholder="Coloca aqui tu email" />
    <'input type="password" id="pass" placeholder="Coloca aqui tu password" />
    <'button onclick="registrar()" >Registrarse</button>
</code></pre>

Y en la seccion Script:
*Primero se guardan los datos del formulario en variables de email y contraseña y luego se llama a la funcion de firebase "firebase.auth()" para despues llamar a "createUserWithEmailAndPassword()" y pasarle como parametros el email y contraseña previamente guardados. Luego se puede agregar una funcion para que en caso de que ocurra algun error salte una alerta*
<pre><code>
  function registrar(){
    var email=document.getElementById('email').value;
    var pass=document.getElementById('pass').value;
    firebase.auth().createUserWithEmailAndPassword(email, pass)
    .catch(function(error) {
        var errorCode = error.code;
        var errorMessage = error.message;
        alert(errorMessage);
      });
  }
</code></pre>

#### Función de Autenticación
*Esta es una gran herramienta para verificar que los mails sean legitimos*

Haremos que cuando el usuario se registre, automaticamente se le envie una peticion de autencicaión a su mail:

**Creamos la funcion verificar la cual guardará el currentUser que acaba de registrarse y le aplicará la funcion para enviar la verificacion de email.**
<pre><code>
function verificar(){
  var user = firebase.auth().currentUser;
  user.sendEmailVerification().then(function() {
  }).catch(function(error) {
  });
}
</code></pre>

**Y aqui le agregamos .then(function(){ verificar();}) a la funcion de registrar() para que llame a la funcion verificar() y asi el usuario pueda verificar su mail**
<pre><code>
  function registrar(){
    var email=document.getElementById('email').value;
    var pass=document.getElementById('pass').value;
    firebase.auth().createUserWithEmailAndPassword(email, pass)
    .catch(function(error) {
        var errorCode = error.code;
        var errorMessage = error.message;
        alert(errorMessage);
      }).then(function(){
        verificar();
      });
  }
</code></pre>

Tambien podemos añadir un texto que nos muestre si el usuario esta verificado o no:

**Creamos un campo de texto con id para aplicar la funcion (sin las comillas iniciales en cada tag)**

<pre><code>
<'h2 id="logueado"></h2>
</code></pre>
  
**Establecemos un observador en el objeto Auth y hacemos una funcion pasando un parametro user(se puede llamar de cualquier forma). Ese parametro lo utilizaremos para aplicar las funciones de firebase.auth y obtener el email, emailVerificado y cualquier otro que necesitemos como displayName, photoURL, etc. Hacemos un if simple para ver si el usuario esta logueado o no, retornando un texto en cada caso y además dentro del caso en el que esté logueado, se vera si esta verificado o no con otro if simple.**
<pre><code>
firebase.auth().onAuthStateChanged(function(user) {
  if (user) {
    // Si el usuario esta logueado
    var email = user.email;
    var emailVerified = user.emailVerified;
    var textoVerificado="";
    if(emailVerified===false){
      textoVerificado="Email no verificado";
    }
    else{
      textoVerificado="Email verificado";
    }
    var providerData = user.providerData;
    document.getElementById('logueado').innerHTML=`<p>Logueado como <p>`+user.email+
    ` `+ textoVerificado
    } 
  else {
    // Si el usuario no esta logueado
    document.getElementById('logueado').innerHTML="No logueado";
  }
});
</code></pre>

#### Función de LogIn y LogOut
Utilizaremos un formulario para LogIn y un boton para LogOut:
*Copiar sin las comillas al principio de cada tag*
<pre><code>

  <'h1>Ingresar</h1>
  <'input type="email" id="email_log" placeholder="Coloca aqui tu email" />
  <'input type="password" id="pass_log" placeholder="Coloca aqui tu password" />
  <'button onclick="login()" >Enviar</button>
  
  <'br> <button onclick="logout()"> Cerrar Sesion </button>
</code></pre>
















