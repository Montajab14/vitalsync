# Multi-stage build pour le back-end VitalSync
# Stage 1 (builder) : installe toutes les dépendances y compris devDependencies
# et exécute les tests Jest avant de construire l'image de production.
# Stage 2 (production) : repart d'une image Alpine vierge et ne copie que
# le code source et les dépendances de production. Cela réduit drastiquement
# la taille de l'image finale (pas de Jest, pas de supertest, pas de cache npm)
# et améliore la sécurité en limitant la surface d'attaque.

FROM node:20-alpine AS builder
WORKDIR /app
COPY package.json .
RUN npm install
COPY . .
RUN npm test

FROM node:20-alpine AS production
WORKDIR /app
COPY package.json .
RUN npm install --omit=dev
COPY --from=builder /app/server.js .
EXPOSE 3000
CMD ["node", "server.js"]
