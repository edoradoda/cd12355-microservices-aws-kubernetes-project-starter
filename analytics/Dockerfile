# Dockerfile
FROM python:3.9-slim

# Establecer el directorio de trabajo
WORKDIR /app

# Copiar los archivos de requisitos y la aplicación
COPY requirements.txt requirements.txt
RUN pip install -r requirements.txt
RUN pip install python-dotenv
COPY . .

# Exponer el puerto en el que correrá la aplicación
# EXPOSE 5153

# Comando para ejecutar la aplicación
CMD ["python", "app.py"]