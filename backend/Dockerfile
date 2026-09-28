# Imagem oficial do Node (versão alpine = menor)
FROM node:22-alpine

WORKDIR /app

# Copia só os manifestos primeiro: se o código mudar mas as
# dependências não, o Docker reaproveita a camada do npm ci (cache)
COPY package.json package-lock.json ./
RUN npm ci --omit=dev

# Agora o restante do código
COPY . .

EXPOSE 3000
CMD ["node", "server.js"]
