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

Ahora descargamos el script de "entrypoint.sh" y lo agregamos en el directorio del proyecto, nos debemos asegurar que dicho archivo tenga permisos de escritura lectura y execucion.

```bash
  sudo chmod 775 entrypoint.sh
```

Ahora descargamos los archivos Dockerfile, docker-compose.yml y el env-example a nuestro directorio raiz del proyecto.

Despues cambiamos el nombre del env-example a env y seteamos un password para la base de datos de postgres, al igual que en archivo docker-compose estos password deben coincidir.

Si deseas puedes agregar estas gemas a tu Gemfile:

```bash
  foreman
  sidekiq
  redis
```

Construimos la app con sus dependencias:

```bash
  docker-compose build web
```

Creamos la base de datos con Rails:

```bash
  docker-compose run --rm web rails db:create
```

Corremos el servidor:

```bash
  docker compose up
```

El puerto predefinido es el 3000 pero lo puedes modificar en el Dockerfile y en el docker-compose.yml

Cada vez que se quiera correr un comando propio de rails se debe hacer con estos prefijos:

```bash
  docker-compose run --rm web rails
```

Por ejemplo cuando se genera un controlador:

```bash
  docker-compose run --rm web rails g controller Pages index users
```