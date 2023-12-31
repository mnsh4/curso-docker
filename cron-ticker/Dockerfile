# Dependencias de desarrollo

# Establece la imagen base de Node.js y asigna un alias "deps" a esta etapa del Dockerfile
FROM node:19.2-alpine3.16 as deps
# Establece el directorio de trabajo dentro del contenedor en "/app"
WORKDIR /app
# Copia el archivo "package.json" desde el directorio local (fuera del contenedor) al directorio de trabajo "/app" en el contenedor
COPY package.json ./
# Ejecuta "yarn install" para instalar las dependencias de Node.js especificadas en el archivo "package.json"
RUN yarn install

# Build y Tests
FROM node:19.2-alpine3.16 as builder
WORKDIR /app
COPY --from=deps /app/node_modules ./node_modules
COPY . .
RUN yarn test

# Dependencias de Producción
FROM node:19.2-alpine3.16 as prod-deps
WORKDIR /app
COPY package.json ./
RUN yarn install --prod

# Ejecutar la APP
FROM node:19.2-alpine3.16 as runner
WORKDIR /app
COPY --from=prod-deps /app/node_modules ./node_modules
COPY app.js ./
COPY tasks/ ./tasks
CMD [ "node", "app.js" ]
