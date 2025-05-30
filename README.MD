# Dockerfiles

## Dockerfile Pagamentos PHP
FROM php:8.2-cli-alpine - passo a imagem base da linguagem que sera utilizado no projeto

WORKDIR /app - cria um diretorio dentro do container para eu utilizar na aplição

COPY index.php . - copio o arquivo da minha maquina para o diretorio do container

EXPOSE 3003 - aponto qual porta a aplicação vai ficar rodando dentro do container

CMD ["php", "-S", "0.0.0.0:3003"] - utilizado para fazer a API rodar e subir


## Dockerfile Pedidos PYTHON
FROM python:3.9-slim - passo a imagem base da linguagem que sera utilizado no projeto

WORKDIR /usr/src/app - crio um diretorio com essas pastas dentro do container 

COPY requirements.txt ./ - primeiro passo para o diretorio o .txt que contem as bibliotecas e frameworks que vou usar

RUN pip install --no-cache-dir -r requirements.txt - mando ele rodar um comando install das bibliotecas do .txt que passei

COPY . . - termino de passar tudo que tem no meu diretorio local que ta o arquivo Dockerfile

EXPOSE 3002 - aponto a porta que a aplicação vai ficar rodando no container

CMD ["python", "app.py"] - comando cmd para rodar a API


## Dockerfile Produtos NODE.JS
FROM node:18-alpine - passo a imagem que vou utilizar na API

WORKDIR /usr/src/app - crio um diretorio dentro do container com esse path

COPY package*.json ./ - passo para o container primeiro os arquivos package*.json para o container, pois contem regras

RUN npm install --only=production - mando instalar as dependencias necessarias para rodar em produção e ignora as outras

COPY . . - copio para o container o restante dos arquivos do diretorio da maquina que contem o dockerfile

EXPOSE 3001 - aponto a porta que a aplicação vai rodar no container

CMD [ "npm", "start" ] - passo o comando cmd necessario para rodar a API


# Sobre o docker-compose.yml

db: Pro banco MySQL, eu digo pra usar a imagem mysql:5.7, dou o nome mysql_db_ecommerce pro contêiner, defino o login e senha e mando criar o banco ecommerce. Utilizo a porta 3306 e os dados são salvos no mysql_data pra não sumir.

redis: Pro Redis, uso a redis:6.2-alpine, chamo o conteiner de redis_cache_ecommerce, passo a porta 6379 no PC pro redis usar e os dados tambem vão pra um volume redis_data.

products: Pra API de Produtos, do o build da pasta ./api_produtos que ele vai ler o dockerfile e subir a API.digo que o conteiner se chama produtos_api_node e passo a porta 3001 para ele usar.

orders: Pra API de Pedidos, do o build da pasta ./api_pedidos para ele ler o dockerfile e subir a API. digo que conteiner é o orders_api_python, falo para usa a porta 3002 e ele só sobe (depends_on) depois do db, redis e products.

payments: Pra API de Pagamentos, do o build da pasta ./api_pagamentos para ele ler o dockerfile e subir a API. digo que o conteiner é pagamentos_api_php e falo para usa a porta 3003 e esperar (depends_on) a API orders subir.

networks:
ecommerce_network: Cria uma rede interna só para as APIs conversarem entre si usando os nomes dos serviços (tipo, orders acha db fácil).

volumes:
mysql_data: e redis_data: São tipo uns "pen drives" que o Docker cuida, pra guardar os dados do MySQL e do Redis e eles não se perderem quando os contêineres pararem.
