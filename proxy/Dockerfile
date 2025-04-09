# Dockerfile en el directorio proxy
FROM nginx:alpine

# Copiar el archivo de configuración de Nginx
COPY nginx.conf /etc/nginx/nginx.conf

# Copiar los archivos del frontend al directorio de Nginx
COPY ../frontend /usr/share/nginx/html