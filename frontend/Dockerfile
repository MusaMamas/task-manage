# --- Build-vaihe ---
FROM node:22-alpine AS builder

WORKDIR /app

COPY package*.json ./
RUN npm install

COPY . .

# VITE_API_URL pitää olla määriteltynä build-aikana (ARG → ENV)
ARG VITE_API_URL
ENV VITE_API_URL=${VITE_API_URL}

RUN npm run build

# --- Runtime-vaihe: Nginx serving static files ---
FROM nginx:alpine

# Poista oletus-config ja lisää oma (SPA:lle sopiva)
RUN rm /etc/nginx/conf.d/default.conf
COPY nginx.conf /etc/nginx/conf.d/default.conf

# Kopioi buildatut tiedostot
COPY --from=builder /app/dist /usr/share/nginx/html

EXPOSE 80

CMD ["nginx", "-g", "daemon off;"]