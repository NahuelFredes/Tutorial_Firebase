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
*Podés utilizar el siguiente (sin la comilla inicial en cada tag)*

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
*Esta es una gran herramienta para verificar que los emails sean legitimos*

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

**Creamos un campo de texto con id para aplicar la funcion (sin la comilla inicial en cada tag)**

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
    document.getElementById('logueado').innerHTML=`<'p>Logueado como <'p>`+user.email+
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
*Copiar sin la comilla al principio de cada tag*

<pre><code>
  <'h1>Ingresar</h1>
  <'input type="email" id="email_log" placeholder="Coloca aqui tu email" />
  <'input type="password" id="pass_log" placeholder="Coloca aqui tu password" />
  <'button onclick="login()" >Iniciar Sesion</button>
  <'br> 
  <'button onclick="logout()"> Cerrar Sesion </button>
</code></pre>

Luego en la seccion de Script escribimos las funciones correspondientes:
**Simplemente creamos las variables para luego pasarle como parametros a la funcion signInWithEmailAndPassword() y en la funcion logout llamamos a signOut() del objeto auth()**
<pre><code>  
function login(){
    var email_log=document.getElementById('email_log').value;
    var pass_log=document.getElementById('pass_log').value;
    firebase.auth().signInWithEmailAndPassword(email_log, pass_log).catch(function(error) {
        var errorCode = error.code;
        var errorMessage = error.message;
        alert(errorMessage);
        });
    }

function logout(){
    firebase.auth().signOut()
}
</code></pre>

### Database
En Firestore tenemos dos alternativas bastante similares pero con algunas diferencias. La diferencia principal es que Realtime Database esta compuesta por un unico objeto Jason que contiene los datos, en cambio Cloud Firestore esta compuesta por Colecciones las cuales contienen objetos Jason. En esta guia haremos una introduccion en el uso de Cloud Firestore.

### Cloud Firestore
Al dirigirse a la seccion Databases en Firebase, seleccionamos Cloud Firestore, crear base de datos. Aqui nos apareceran dos opciones, seleccionamos la de modo de prueba ya que esta nos permitira testear y aprender el funcionamiento sin restricciones de seguridad (cabe destacar que cualquier persona podrá escribir y leer datos de tu base de datos durante 30 dias). El modo de produccion sirve para establecer reglas de seguridad sobre quien puede leer o escribir en nuestra base de datos.

**Una vez creada la base de datos nos podemos dirigir a la seccion Reglas y cambiar esta linea de codigo:**
    <pre><code>
    allow read, write: if request.time < timestamp.date(2020, 8, 30);
    </code></pre>
    
**Y ponemos esta para que asi la base de datos este totalmente disponible a todo el mundo en todo momento(se recomienda para uso de aprendizaje o testeo, de otra forma es conveniente establecer reglas de restriccion de seguridad):**
    <pre><code>
    allow read, write: if true;
    </code></pre>

#### Setteo Básico de Datos
*Pimero tenemos que agregar las librerias a nuestro proyecto, para ello copia el script del siguiente [link](https://firebase.google.com/docs/firestore/quickstart?hl=es-419#web)*

Vamos a crear un formulario simple con un boton para cargar datos:
*Copiar sin la comilla antes de cada tag*
<pre><code>
<'h1>Formulario</h1>
  <'input type="text" id="nombre" placeholder="Nombre" />
  <'input type="text" id="apellido" placeholder="Apellido" />
  <'input type="number" id="dni" placeholder="DNI" />
  <'br>
  <'button onclick="send()">Enviar</button>
</code></pre>













