#### Stage 1: Build React
FROM node:18-alpine AS build

WORKDIR /app

COPY package.json package-lock.json ./
RUN npm ci

COPY . .

ARG REACT_APP_API_BASE_URL
ENV REACT_APP_API_BASE_URL=${REACT_APP_API_BASE_URL}

RUN npm run build

#### Stage 2: Nginx
FROM nginx:alpine

# Limpiar config default
RUN rm /etc/nginx/conf.d/default.conf

# Copiar build React
COPY --from=build /app/build /usr/share/nginx/html

# Copiar nginx config
COPY nginx.conf /etc/nginx/conf.d/default.conf

EXPOSE 80

CMD ["nginx", "-g", "daemon off;"]
