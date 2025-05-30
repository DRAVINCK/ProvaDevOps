# Usar uma imagem base oficial do Python
FROM python:3.9-slim

# Definir o diretório de trabalho no contêiner
WORKDIR /usr/src/app

# Copiar o arquivo de dependências primeiro para aproveitar o cache do Docker
COPY requirements.txt ./

# Instalar as dependências
RUN pip install --no-cache-dir -r requirements.txt

# Copiar o restante do código da aplicação para o diretório de trabalho
COPY . .

# Expor a porta em que o aplicativo Flask roda
EXPOSE 3002

# Comando para rodar a aplicação quando o contêiner iniciar
CMD ["python", "app.py"]