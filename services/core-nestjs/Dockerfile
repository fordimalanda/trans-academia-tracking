FROM node:20-alpine AS builder

WORKDIR /app

# Copie des fichiers de dépendances
COPY package*.json ./
COPY prisma ./prisma/

# Installation des packages et génération de Prisma
RUN npm ci
RUN npx prisma generate

# Copie du code source et compilation
COPY . .
RUN npm run build

# --- Stage de Production ---
FROM node:20-alpine AS runner

WORKDIR /app

ENV NODE_ENV=production

COPY package*.json ./
RUN npm ci --only=production

COPY --from=builder /app/node_modules/.prisma ./node_modules/.prisma
COPY --from=builder /app/node_modules/@prisma ./node_modules/@prisma
COPY --from=builder /app/dist ./dist
COPY --from=builder /app/prisma ./prisma

EXPOSE 3000 50051

CMD ["sh", "-c", "npx prisma migrate deploy && node dist/main.js"]