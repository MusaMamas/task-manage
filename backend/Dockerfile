# --- Build-vaihe ---
FROM node:22-alpine AS builder

WORKDIR /app

# Kopioi vain package-tiedostot ensin, jotta npm ci voidaan cachettaa
COPY package*.json ./

# Käytä npm install monorepon sisä-moduuleille
RUN npm install

# Kopioi loput backendin lähdekoodit
COPY . .

# Rakenna TypeScript → JavaScript (oletus: "build" script backend/package.jsonissa)
RUN npm run build

# --- Runtime-vaihe ---
FROM node:22-alpine AS runner

WORKDIR /app

# Kopioi vain tarvittavat jutut ajovaiheeseen
COPY --from=builder /app/package*.json ./
COPY --from=builder /app/node_modules ./node_modules
COPY --from=builder /app/dist ./dist

# Backend kuuntelee porttia 5000 (README) :contentReference[oaicite:0]{index=0}
EXPOSE 5000

# Käytetään tuotantoa
ENV NODE_ENV=production

# Oletus: backend käynnistyy komennolla "npm start"
CMD ["npm", "start"]
