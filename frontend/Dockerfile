# Imagem oficial do Node (versão alpine = menor)
FROM node:22-alpine

WORKDIR /app

# Dependências primeiro, para aproveitar o cache de camadas
COPY package.json package-lock.json ./
RUN npm ci

COPY . .

# O Vite "embute" as variáveis VITE_* no código na hora do build,
# por isso a URL da API entra como argumento de build
ARG VITE_API_URL=http://localhost:3000
ENV VITE_API_URL=$VITE_API_URL
RUN npm run build

EXPOSE 4173
# vite preview serve a pasta dist (já com fallback para as rotas do React)
CMD ["npm", "run", "preview", "--", "--host", "0.0.0.0", "--port", "4173"]
