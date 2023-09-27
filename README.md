# Parte 2: Contenerizar una aplicación

## Obtener aplicación.

+ 1: Clonar el repositorio de la aplicación usando el siguiente comando: `git clone https://github.com/docker/getting-started-app.git`, posteriormente revisar el contenido.

![Clonar repositorio](/images/Paso2Img1.png)

## Construye la imagen de la aplicación.

+ 1: En el `getting-started-app` directorio, la misma ubicación que el `package.json` archivo, cree un archivo llamado `Dockerfile`. Puede utilizar los siguientes comandos para crear un Dockerfile basado en su sistema operativo.
`cd /path/to/getting-started-app`

`touch Dockerfile`

![Archivo Dockerfile](/images/Paso2Img2.png)

+ 2: Usando un editor de texto o editor de código, agregue el siguiente contenido al Dockerfile:

`FROM node:18-alpine
WORKDIR /app
COPY . .
RUN yarn install --production
CMD ["node", "src/index.js"]
EXPOSE 3000`

![Modificación Dockerfile](/images/Paso2Img3.png)

+ 3: Construya la imagen usando los siguientes comandos:

`docker build -t getting-started .`

![Construcción de imagen](/images/Paso2Img4.png)

## Iniciar un contenedor de aplicaciones.

+ 1: Ejecute su contenedor usando el comando `docker run -dp 127.0.0.1:3000:3000 getting-started`

![Ejecución de contenedor](/images/Paso2Img5.png)

![Revisión de aplicación](/images/Paso2Img6.png)

# Parte 3: Actualizar aplicación

## Ajustamos el código fuente

+ 1: Actualizamos el nuevo texto que queremos mostrar en nuestra aplicación

![Ajuste de código](/images/Paso3Img1.png)

+ 2: Procedemos entonces a actualizar nuestra vesión de la imagen, pero antes debemos retirar el contenedor viejo

`docker stop 5208c88b882b`

`docker rm 5208c88b882b`

`docker build -t getting-started .`

![Remove de contenedor viejo](/images/Paso3Ing2.png)

![Actualizar imágen](/images/Paso3Img3.png)

+ 3: Iniciamos nuestro nuevo contenedor con el código previamente actualizado:

`docker run -dp 127.0.0.1:3000:3000 getting-started`

![Actualizar imágen](/images/Paso3Img4.png)

![Actualizar imágen](/images/Paso3Img5.png)


# 4: Comparte la aplicación

## Crear un repositorio

+ 1: Creamos un repositorio en Docker Hub

![Creación de repositorio](/images/Paso4Img1.png)

## Hacemos push a la imagen de Docker

+ 1: Inicialmente nos logueamos en Docker Hub

`docker login -u YOUR-USER-NAME`

+ 2: Utilizamos el comando de docker tag para renombrar la imágen

`docker tag getting-started YOUR-USER-NAME/getting-started`

+ 3: Hacemos push a nuestra imágen

`docker push YOUR-USER-NAME/getting-started`

![Push a docker hub](/images/Paso4Img2.png)

![Paso4Img3](/images/Paso4Img3.png)

# 5: persistir la base de datos

+ 1: Inicialmente creamos nuestro volumen

`docker volume create todo-db`

+ 2: Detenemos y eliminamos el contenedor que está corriendo

`docker rm -f <id>`

+ 3: Iniciamos un nuevo contenedor con el volumen creado previamente

`docker run -dp 127.0.0.1:3000:3000 --mount type=volume,src=todo-db,target=/etc/todos getting-started`

![Creación de volumen](/images/Paso5Img1.png)

![Vista de volumen creado](/images/Paso5Img2.png)

+ 4: Procedemos entonces a agregar items en nuestra app

![Items TodoApp](/images/Paso5Img3.png)

+ 5: Detenemos el contenedor, lo eliminamos y procedemos a iniciar uno nuevo con el volumen creado

![Ejecución de nuevo contenedor](/images/Paso5Img4.png)

Al verificar en nuestra aplicación, evidenciamos que la lista de items se graba correctamente

![Verificación de items](/images/Paso5Img5.png)

# 6: Usar soportes de enlace 

## Probando Monturas Vinculantes 

+ 1: Abra una terminal y cambie de directorio al getting-started-app directorio.

+ 2: En la terminal ejecute el comando para comenzar bashen un Ubuntu contenedor con un montaje de enlace.

    `docker run -it --mount type=bind,src="$(pwd)",target=/src ubuntu bash`

    ![Bashen en Ubuntu](/images/Paso6pmv1.png)

+ 3: Después de ejecutar el comando, Docker inicia una bash sesión interactiva en el directorio raíz del sistema de archivos del contenedor.

    ![Sesion Intectiva](/images/Paso6pmv2.png)

+ 3: Cambiamos al directorio SCR con el comando -cs scr

    ![Cambio de directorio](/images/Paso6pmv3.png)

+ 4: Cree un nuevo archivo llamado myfile.txt.

    `touch myfile.txt`

    ![Creacion de archivo myfile.txt](/images/Paso6pmv4.png)

+ 5: Eliminamos el archivo y validamos que ya no se encuentra nuevamente. 

    `rm myfile.txt`

    ![Eliminar archivo myfile.txt](/images/Paso6pmv5.png)

+ 6: Usamos la combinación de teclas Crtl+D para detener la sesión del contenedor interactivo 

    ![Deterner sesion interactiva](/images/Paso6pmv6.png)

## Contenedores de Desarrollo

+ 1: Asegúrese de no tener ningún getting-startedcontenedor ejecutándose actualmente.

+ 2: Ejecute el siguiente comando desde el getting-started-appdirectorio.

    ` docker run -dp 127.0.0.1:3000:3000 \ `
     `-w /app --mount type=bind,src="$(pwd)",target=/app \ `
     `node:18-alpine \ `
     `sh -c "yarn install && yarn run dev"`
    
    ![Comando Docker run](/images/Paso6pcdd1.png)

+ 3: Puedes ver los registros usando docker logs container-id. Sabrás que estás listo para comenzar cuando veas esto:

    ![Comando Docker logs](/images/Paso6pcdd2.png)

+ 4: Cuando haya terminado de ver los registros, salga presionando Ctrl+ C.

    ![Salir](/images/Paso6pcdd3.png)

## Desarrolla tu aplicación con el contenedor de desarrollo

+ 1: 1.	En el src/static/js/app.jsarchivo, en la línea 109, cambie el botón "Agregar elemento" para que diga simplemente "Agregar":

    ![Cambio en app.js](/images/Paso6dcd1.png)

+ 2: Actualice la página en su navegador web y debería ver el cambio reflejado casi de inmediato. 
    
     ![Visualización de página](/images/Paso6dcd2.png)

+ 3: No dude en realizar cualquier otro cambio que desee. Cada vez que realiza un cambio y guarda un archivo, el nodemonproceso reinicia la aplicación dentro del contenedor automáticamente. Cuando haya terminado, detenga el contenedor y cree su nueva imagen usando: 

    ` docker build -t getting-started . `

    ![Nueva imagen del contenedor](/images/Paso6dcd3.png)

# 7: Aplicaciones de múliples contenedores

## Inciar MySql

+ 1: Crea la red. 

    `docker network create todo-app`

    ![Crear red](/images/Paso7imy1.png)

+ 2: Inicie un contenedor MySQL y conéctelo a la red. 
     
    ` $ docker run -d \`
        `--network todo-app --network-alias mysql \`
        `-v todo-mysql-data:/var/lib/mysql \`
        `-e MYSQL_ROOT_PASSWORD=secret \`
        `e MYSQL_DATABASE=todos \`
        `mysql:8.0`

     ![Inicar contenedor](/images/Paso7imy2.png)

+ 3: Para confirmar que tiene la base de datos en funcionamiento, conéctese a la base de datos y verifique que se conecte. 

    `docker exec -it <mysql-container-id> mysql -u root -p`

     ![Conectarse a la BD](/images/Paso7imy3.png)

+ 4: Salga del shell MySQL para regresar al shell de su máquina.

     ![Salir de MySql](/images/Paso7imy4.png)

## Conéctate a MySql

+ 1: Inicie un nuevo contenedor usando la imagen nicolaka/netshoot. 
    
    `docker run -it --network todo-app nicolaka/netshoot`

     ![Inicar contenedor con nicolaka](/images/Paso7cmy1.png)

+ 2: Dentro del contenedor, usarás el digcomando, que es una herramienta DNS útil. Vas a buscar la dirección IP para el nombre de host mysql.

    `dig mysql`

    ![Resultado](/images/Paso7cmy2.png)

## Ejecute su aplicación con MySql

+ 1: Especifique cada una de las variables necesarias para la conexion a MySql. Asegúrese de estar en el getting-started-appdirectorio cuando ejecute este comando.

    `$ docker run -dp 127.0.0.1:3000:3000 \`
    ` -w /app -v "$(pwd):/app" \`
    `  --network todo-app \`
    `  -e MYSQL_HOST=mysql \`
    `  -e MYSQL_USER=root \`
    `  -e MYSQL_PASSWORD=secret \`
    `  -e MYSQL_DB=todos \`
    `  node:18-alpine \`
    `  sh -c "yarn install && yarn run dev"`

     ![Ejecutar con MySql](/images/Paso7emy1.png)

+ 2: Observa los registros del contenedor con: 

    `docker logs -f <container-id>`

+ 3: Abra la aplicación en su navegador y agregue algunos elementos a su lista de tareas pendientes.

    ![Creado tareas](/images/Paso7emy2.png)

+ 4: Conéctese a la base de datos mysql y demuestre que los elementos se están escribiendo en la base de datos.

    `docker exec -it <mysql-container-id> mysql -p todos`

    Y en el shell de mysql, ejecuta lo siguiente:

    `select * from todo_items;`

    ![Resultado de la consulta](/images/Paso7emy3.png)

# 8: Usar Docker Compose

## Crear el archivo compose.yaml. 

+   ![Crear archivo](/images/Paso8cac1.png)

## Definir el servicio de la aplicación

+ 1: Ábralo compose.yamlen un editor de texto o código y comience definiendo el nombre y la imagen del primer servicio (o contenedor) que desea ejecutar como parte de su aplicación. El nombre se convertirá automáticamente en un alias de red, lo que será útil al definir su servicio MySQL.

    ![Definir Imagen](/images/Paso8dsa1.png)

+ 2: Por lo general, verá commandcerca de la imagedefinición, aunque no hay ningún requisito para realizar el pedido. Agregue el commanda su compose.yamlarchivo.

    ![Agregar commanda](/images/Paso8dsa2.png)
        
+ 3: Ahora migre la -p 3000:3000parte del comando definiendo el portspara el servicio.

    ![Migrar](/images/Paso8dsa3.png)

+ 4: A continuación, migre tanto el directorio de trabajo ( -w /app) como la asignación de volumen ( -v "$(pwd):/app") utilizando las definiciones working_diry .volumes.

    ![Migrar directorio de trabajo](/images/Paso8dsa4.png)

+ 5: Finalmente, necesita migrar las definiciones de variables de entorno usando la environmentclave.

    ![Migrar definición de variables](/images/Paso8dsa5.png)

## Definir el servicio MySql

+ 1: Primero defina el nuevo servicio y asígnele un nombre mysqlpara que obtenga automáticamente el alias de red. También especifique la imagen que se utilizará.

    ![Definir servicio](/images/Paso8dsm1.png)

+ 2: A continuación, defina la asignación de volumen. Cuando ejecutó el contenedor con docker run, Docker creó el volumen nombrado automáticamente. Sin embargo, eso no sucede cuando se ejecuta con Compose. 

    ![Definir asiugnación de volumen](/images/Paso8dsm2.png)

+ 3: Finalmente, debe especificar las variables de entorno.

    ![Especificar variables de entorno](/images/Paso8dsm3.png)

## Ejecute la pila de aplicaciones

+ 1: Asegúrese de que no se estén ejecutando otras copias de los contenedores primero

    ![Validar contenedores](/images/Paso8epa1.png)

+ 2: Inicie la pila de aplicaciones usando el docker compose upcomando. Agregue la -dbandera para ejecutar todo en segundo plano.

    ![Inicar pila de aplicaciones](/images/Paso8epa2.png)    

+ 3: 3.	Mire los registros usando el docker compose logs -f comando. Si ya ejecutó el comando, verá un resultado similar a este:

    ![Comando logs -f](/images/Paso8epa3.png)      

# 9: Mejores prácticas de creación de imágenes

## Capas de imagen

+ 1: Utilice el docker image history comando para ver las capas de la getting-startedimagen.

    `docker image history getting-started`

    ![Comando Docker history](/images/Paso9ci1.png) 

+ 2: Notarás que varias de las líneas están truncadas. Si agrega la --no-truncbandera, obtendrá el resultado completo.

    `docker image history --no-trunc getting-started`

    ![Comando Docker history --](/images/Paso9ci2.png) 

## Almacenamiento en caché de capas

+ 1: Actualice el Dockerfile para copiar el package.jsonprimero, instale las dependencias y luego copie todo lo demás.

      `  # syntax=docker/dockerfile:1`
    `FROM node:18-alpine`
    `WORKDIR /app`
    `COPY package.json yarn.lock ./`
    `RUN yarn install --production`
    `COPY . .`
    `CMD ["node", "src/index.js"]`

    ![Actualizar Dockerfile](/images/Paso9acc1.png) 

+ 2: Cree un archivo con el nombre .dockerignoreen la misma carpeta que Dockerfile con el siguiente contenido.

    `node_modules`

    ![Crear arhcivo .dockerignoreen](/images/Paso9acc2.png) 

+ 3: Cree una nueva imagen usando docker build.

    `docker build -t getting-started .`

    ![Crear nueva imagen](/images/Paso9acc3.png) 

+ 4: Ahora, haga un cambio en el src/static/index.html.

    ![Cambio en index.html](/images/Paso9acc4.png) 

+ 5: Cree la imagen de Docker ahora usando docker build -t getting-started .nuevamente. Esta vez, su resultado debería verse un poco diferente.

    ![Imagen con docker build](/images/Paso9acc5.png) 



