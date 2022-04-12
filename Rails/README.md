# Configuracion de Docker para trabajar con RAILS 7

Las configuraciones de Docker para que trabaje con Rails 6 estan por todo lado, sin embargo en 7 cambian algunas cosas, ya no se usa webpack por lo tanto no es necesario sobrecargar el contenedor con nodejs yarn y python2.

## Instalar Rails en un contenedor

Creamos un contenedor con el que vamos a interactuar, luego lo vamos a eliminar apenas dejemos de usarlo, y tambien crearemos un volumen el cual se reflejara todos los cambios hechos en el contenedor en la direccion "/usr/src/app" con el directorio donde nos encontramos ahora.

Usamos un contexto general "ruby 3.1" mas adelante en el compose usaremos una version y un contexto explicito ya sea "alpine u otro"

El comando que se va a ejecutar seria sh, o tambien se puede usar bash pero por uniformidad se usa /bin/sh

```bash
  docker run -it --rm -v ${PWD}:/usr/src/app ruby:3.1 /bin/sh
```

Una vez dentro del contenedor ya podemos interactuar, por lo tanto es como estar en una "maquina virtual" entonces procedemos a instalar rails:

```bash
  gem install rails
```

Luego de instalar rails nos vamos a "/usr/src/app"

```bash
  cd /usr/src/app
```

Creamos un proyecto nuevo de rails recordemos que con los flags --api y --database= escogemos si queremos una API solamente y tambien la base de datos con el cual el Active Record de conectara en este caso uso Postgresql

```bash
  rails new my-api-name --api --database=postgresql
```

Ya creado el proyecto en el contenedor procedemos a salir y nos percatamos que tambien tenemos el proyecto y sus respectivos archivos en nuestra carpeta.

```bash
  exit
```

Ahora debemos dar permisos a nuestro directorio para ello vamos al directorio en si:

```bash
  cd my-api-name
  sudo chown -R $USER:$USER .
```

