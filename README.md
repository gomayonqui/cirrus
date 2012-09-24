# HTTP-JSON-Server para la plataforma Velneo V7

Cirrus.js es una implementacion vJavaScript del protocolo HTTP corriendo sobre el objecto TCP/IP de Velneo V7 y que solo retorna JSON, orientado a la construcción de API's para dar acceso mediante Ajax o peticiones Http a aplicaciones desarrolladas sobre v7.

## Installación

#### Herencia
Para comenzar a usar Cirrus.js en la carpeta /bin puedes encontrar un archivo .vin el cual podrás instalar dentro de tu Velneo vServer y heredar en este algún proyecto del que desees servir datos web, es decir el webserver debe quedar "por encima" de la aplicación a servir a fin de poder usar las busquedas y procesos de datos de pertenecientes a esta.

#### Copia
Si prefieres copiar directamente el servidor dentro de tu aplicación para evitar los inconveniente de la herencia, copia la carpeta JSON-server dentro de tu solución, crea un script Js y copia el codigo de Cirrus.js dentro de ese script y finalmente asignale ese script al proceso "HTTP-SERVER".

Listo ahora tienes a Cirrus.js disponible y solo debes iniciar el objeto TCP en el plano de ejecucuón en el que lo desees usar.

## Uso rápido

"Hola Mundo" usando Cirrus.js

```javascript
	// Definicion del controlador
	helloController = {
		sayHello: function(params){
			return({mensaje: "Hola Mundo"});
		}
	}

	// Creando las rutas
	wApp.addRoutes({"GET /hello": "hello#sayHello"})

	//Procesando los requests
	var request = theRoot.varToString("REQUEST")
	theRoot.setVar("RESPONSE", Response(Request(request)))
```

Inicio objecto TCP en el puerto 3000

```bash
	>> curl http://localhost:3000/hello 
	"{"mensaje":"Hola Mundo"}"
```


## Modo de uso.

Cirrus.js es mas que simplemente un parseador de peticiones HTTP te da una estructura para que organices tu código y no tengas que trabajar el ruteo y parametros, toda esta funcionalidad que esta embebida dentro del objeto wApp, el cual englobla los datos de aplicación que quieres exponer.

#### Definición de rutas:

Las rutas son el intermediario entre las peticiones HTTP y la lógica de negocio que se encuentra en los controlladores de la aplicacción, andiendo rutas al router de wApp crearas la vias entre las peticiones y tu codigo:


```javascript
wApp.addRoutes({"GET /users": "users#index", "GET /users/new": "users#new"})
```
con la anterior sentencia estas enviando las peticiones GET dirigidas al path "/users" hacia el controlador usersController y la acción "index"

##### parametros:

Puedes definir parametros dentro de tus urls con el fin de darles un enfoque mas REST de tal forma que tengan sentido al leerlas.

```javascript
wApp.addRoutes({"GET /users/:id": "users#show"})
```

En esta ruta cuando llegue un request "/users/20" el "20" será añadido a la lista global de parametros, los que puedes acceder a traves de la variable params en los controladores que definas.

Adicional a esta definiciones Cirrus.js parseara cualquier parametro extra añadido al final de la URL, siguiendo el formato definido para HTTP (URL?param1=value1&param2=value2) y los añadirá a la lista global de parametros.

##### Soporte REST:

Si deseas crear la interface para un recurso (ususarios, facturas, articulos, categorias, etc) y quieres evitarte construir las rutas para listarlos, crearlos, mostrar espeficos, editarlos, actualizarlos, etc.  Cirrus.js te permite declarar recursos y generara las 7 rutas REST necesarias.

```javascript
wApp.addRoutes({"resource users": "users"})
```
esta declaración añadira automáticamente las siguientes rutas a tu aplicación:

* GET /users
* GET /users/new
* POST /users
* GET /users/:id
* GET /users/:id/edit
* PUT /users/:id
* DELETE /users/:id

## Peticiones con datos en el body

Puedes realizar peticiones POST o PUT que incluyan datos JSON como parte del la petición dentro del body, estos datos serán parseados y podrás acceder a ellos desde wApp.params.body

## Controladores - Añade tu código

Al igual que en el modelo MVC, los controladores son los mediadores entre las peticiones y el acceso a los datos, asi tu definirás controladores para manejar tu lógica de negocios y el acceso a los datos del vServer.


#### Crear el controlador:

Para definir el controlador debes de añadir a wApp un objeto javascript con el nombre del controlador seguido de "Controller", dentro de el definiras metodos que contendran la lógica de negocios y que tendrán acceso a la lista de parametros y finalmente el objeto JSON que representa la respuesta.

Definición del controlador

```javascript
wApp.usersController = {
	index: function(params){
		// tu lógica
		return(algun_objeto_json);
	},
	show: function(params){
		// tu lógica
		return(algun_objeto_json);
	}
}
```
## FAQ's

### como añado funcionalidades HTML + Javascript dentro de mi app v7

Puedes crear funcionalidades que se alimenten de la data de tu vServer y para ello Cirrus.js esta habilitada para peticiones JSONP para evitar la restricción que tienen los navegadores para "cross domain requests".

* http://www.funcion13.com/2012/04/12/como-realizar-peticiones-ajax-cross-domain-jsonp-jquery/

### y el front end HTTP server ?

Puedes usar Apache o Nginx y configurar proxys inversos de tal forma que sea capaz de servir archivos estaticos cuando existan o que redirija peticiones http hacia el vServer cuando se pidan datos dinámicos.

### es Cirrus.js equivalente a vModApache ?

No, vModApache es un modulo especializado para acceso a vServers desarrollado directamente por Velneo S.A y su función es mas parecida a las funcionalidades CGI, Cirrus.js por el contrarío pretende ser directamente un HTTP server orientado la creación de API's.

### Consume Cirrus.js enganches al vServer?

No para nada Cirrus.js corre directamente sobre las conexiones que permite el objeto TCP de v7 y no realiza una conexion VATP con el vServer.

### es Cirrus.js multiplataforma ?

Si claro, al correr directamente sobre el objeto TCP de v7 correra en cualquier plataforma donde el vServer tenga capacidad de correr.

### Quieres contribuir ?

Para contribuir a Cirrus.js, clona el repositorio verifica que todos los specs pasan (abre el archivo spec/SpecRunner.html), no existen dependencias y simplemente puedes revisar el codigo y realizar pull request al repositorio para añadir funcionalidades faltantes o corregir bugs encontrados.

Dipones ademas dentro de la suite de Jasmine.js de un apartado donde puedes tester el performance de tus cambios adiciones utilizando para ello la librería Benchmark.js.

## Licencia

Cirrus.js es liberado bajo las directivas de la licencia MIT

* http://www.opensource.org/licenses/MIT



