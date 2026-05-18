# STAGE 1: Build
FROM node:18-alpine AS builder

WORKDIR /app

COPY . .

# STAGE 2: Production con nginx
FROM nginx:alpine AS production

# Usuario no root
RUN addgroup -S appgroup && adduser -S appuser -G appgroup

# Limpia contenido por defecto
RUN rm -rf /usr/share/nginx/html/*

# Copia los archivos del stage anterior
COPY --from=builder /app/index.html /usr/share/nginx/html/
COPY --from=builder /app/app.js /usr/share/nginx/html/
COPY --from=builder /app/default.conf /etc/nginx/conf.d/default.conf

EXPOSE 80