#Buscamos una imagen que se ajuste a nuestras necesidades (el servidor, el lenguaje, etc)
#Utlilizaremos servidor ngix y juntaremos el back node y el front angular
FROM nginx:stable-alpine

#Usamos el volumen mas eficiente para nuestro proyecto
#es una carpeta temporal que se utiliza para almacenar archivos temporales
VOLUME /tmp 

#Instalamos las dependencias necesarias

#Eliminamos todo lo que tengamos en la carpeta del servido
Run rm -rf /usr/share/nginx/html/*

#Copiamos el archivo en la ruta del servidor
COPY nginx.conf /etc/nginx/nginx.conf

#Tomamos lo que esta en dist y lo copiamos en la carpeta del servidor
COPY dist/billingApp /usr/share/nginx/html

#Copiamos el archivo mime.types en la carpeta del servidor
COPY mime.types /etc/nginx/mime.types


#Para el back end

#Instalammos OpenJDK 17
RUN apk --no-cache add openjdk17-jre

#Definimos la variable de entorno JAVA_HOME

# Definimos la ruta de instalación de Java (JAVA_HOME)
ENV JAVA_HOME=/usr/lib/jvm/java-17-openjdk

# Añadimos la carpeta de binarios de Java al PATH del sistema
ENV PATH $JAVA_HOME/bin:$PATH

#Verificamos la versión de Java instalada
RUN java -version


#Instalamos microservice

# Variable de entorno para pasar opciones a la JVM, se deja vacía por defecto
ENV JAVA_OPTS=""

# Argumento de construcción para indicar el archivo JAR a copiar
ARG JAR_FILE

# Copiamos el archivo JAR al contenedor y lo renombramos como app.jar
ADD ${JAR_FILE} app.jar

#Copiamos el archivo de configuración de la aplicación
COPY appshell.sh appshell.sh

#Los puertos que se van a exponer
#Puerto 80 para el servidor web y puerto 7080 para el back end
EXPOSE 80 7080

#entrypoint para ejecutar el script de inicio
ENTRYPOINT ["sh", "appshell.sh"]