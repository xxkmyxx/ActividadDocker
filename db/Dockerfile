# STAGE 1: Preparación
FROM mysql:8 AS builder

# Copiar script de inicialización
COPY init.sql /docker-entrypoint-initdb.d/

# STAGE 2: Production
FROM mysql:8 AS production

# Variables de entorno
ENV MYSQL_ROOT_PASSWORD=admin123
ENV MYSQL_DATABASE=tienda_perritos
ENV MYSQL_USER=alumno
ENV MYSQL_PASSWORD=alumno123

# Copiar desde el stage anterior
COPY --from=builder /docker-entrypoint-initdb.d/init.sql /docker-entrypoint-initdb.d/

EXPOSE 3306