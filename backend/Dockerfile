# STAGE 1: Build
FROM node:18-alpine AS builder

WORKDIR /app

COPY package*.json ./

RUN npm install

COPY . .

# STAGE 2: Production (imagen final más liviana)
FROM node:18-alpine AS production

# Usuario no root (buena práctica de seguridad)
RUN addgroup -S appgroup && adduser -S appuser -G appgroup

WORKDIR /app

# Solo copiamos lo necesario desde el stage anterior
COPY --from=builder /app/node_modules ./node_modules
COPY --from=builder /app .

# Cambiar al usuario no root
USER appuser

EXPOSE 3001

CMD ["npm", "start"]