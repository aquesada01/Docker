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


