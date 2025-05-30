# Dockerfile para api_produtos_node/

# Usar uma imagem base oficial do Node.js. LTS (Long Term Support) é uma boa escolha.
# Alpine é uma versão menor, boa para produção.
FROM node:18-alpine
# Ou uma versão mais específica como node:20.14.0-alpine

# Definir o diretório de trabalho no contêiner
WORKDIR /usr/src/app

# Copiar os arquivos package.json e package-lock.json (se existir)
# Copiá-los separadamente aproveita o cache do Docker. Se package.json não mudar,
# o npm install não precisa rodar novamente em builds subsequentes.
COPY package*.json ./

# Instalar as dependências da aplicação
# Se você estiver usando npm e tiver um package-lock.json, 'npm ci' é recomendado
# para instalações mais rápidas e consistentes.
# RUN npm ci --only=production
RUN npm install --only=production
# O '--only=production' (ou NODE_ENV=production antes do npm install)
# é para não instalar dependências de desenvolvimento (devDependencies).

# Copiar o restante do código da aplicação para o diretório de trabalho
COPY . .

# Expor a porta em que a API de Produtos roda (definida no index.js)
EXPOSE 3001

# Comando para rodar a aplicação quando o contêiner iniciar
# Isso usará o script "start" definido no seu package.json ("node index.js")
CMD [ "npm", "start" ]