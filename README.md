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



