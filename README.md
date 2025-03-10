# 📊 ELK Stack con Spring Boot 🚀  

Este proyecto demuestra cómo integrar **Elasticsearch**, **Logstash** y **Kibana** (ELK Stack) con una aplicación **Spring Boot** para almacenar y visualizar logs de una API REST.  

---

## 📌 **Tabla de Contenidos**  
- [📖 Descripción](#-descripción)  
- [🛠️ Requisitos](#-requisitos)  
- [📥 Instalación](#-instalación)  
- [🐳 Configuración de Docker](#-configuración-de-docker)  
- [🚀 Ejecución de la Aplicación](#-ejecución-de-la-aplicación)  
- [🔎 Visualización en Kibana](#-visualización-en-kibana)  
- [📊 Creación de Dashboards](#-creación-de-dashboards)  

---

## 📖 **Descripción**  
Este proyecto tiene como objetivo mostrar cómo:  
✅ Configurar **ELK Stack** para almacenar logs de una aplicación.  
✅ Enviar logs desde **Spring Boot** a **Logstash**, que luego los almacena en **Elasticsearch**.  
✅ Visualizar los logs en **Kibana** con filtros y gráficos.  
✅ Crear dashboards para analizar las peticiones a la API REST.  

---

## 🛠️ **Requisitos**  
Antes de comenzar, asegúrate de tener instalados los siguientes programas:  

🔹 [Docker](https://www.docker.com/) 🐳  
🔹 [Docker Compose](https://docs.docker.com/compose/)  
🔹 [Java 17+](https://adoptium.net/) ☕  
🔹 [Maven](https://maven.apache.org/)  

---

## 📥 **Instalación**  

1️⃣ **Clonar el repositorio:**  
```sh
git clone https://github.com/tu-usuario/elk-springboot.git
cd elk-springboot
```

2️⃣ **Levantar los contenedores de ELK Stack:**  
```sh
docker-compose up -d
```

3️⃣ **Esperar a que Elasticsearch y Kibana estén listos.**  
Puedes comprobarlo con:  
```sh
docker ps
```

4️⃣ **Compilar y ejecutar la aplicación Spring Boot:**  
```sh
mvn clean package
mvn spring-boot:run
```

5️⃣ **Probar la API:**  
Abre un navegador o Postman y accede a:  
```
http://localhost:8080/api/hello
```
Esto generará logs en Elasticsearch.

---

## 🐳 **Configuración de Docker**  
📌 Archivo `docker-compose.yml`:  
```yaml
version: "3.8"

services:
  elasticsearch:
    image: docker.elastic.co/elasticsearch/elasticsearch:8.5.3
    container_name: elasticsearch
    environment:
      - discovery.type=single-node
      - xpack.security.enabled=false
    ports:
      - "9200:9200"

  kibana:
    image: docker.elastic.co/kibana/kibana:8.5.3
    container_name: kibana
    ports:
      - "5601:5601"
    depends_on:
      - elasticsearch

  logstash:
    image: docker.elastic.co/logstash/logstash:8.5.3
    container_name: logstash
    volumes:
      - ./logstash/pipeline:/usr/share/logstash/pipeline
    ports:
      - "5000:5000"
      - "9600:9600"
    depends_on:
      - elasticsearch
```

📌 **Explicación:**  
✅ **Elasticsearch:** Almacena los logs.  
✅ **Kibana:** Visualiza los logs en tiempo real.  

---

## 🚀 **Ejecución de la Aplicación**  
📌 **Compilar la aplicación**  
```sh
mvn clean package
mvn spring-boot:run
```

📌 **Verificar logs en consola**  
```sh
tail -f logs/app.log
```

📌 **Probar la API:**  
```sh
curl http://localhost:8080/api/hello
```

Esto generará logs que serán enviados a **Elasticsearch**.

---

## 🔎 **Visualización en Kibana**  
📌 **Accede a Kibana:**  
```
http://localhost:5601
```

📌 **Crear un Data View:**  
1️⃣ Ve a **Management > Stack Management**  
2️⃣ Selecciona **Data Views > Create Data View**  
3️⃣ Usa `logstash-*` como patrón  
4️⃣ Guarda y ve a **Discover** para ver los logs 📊  

---

## 📊 **Creación de Dashboards**  
📌 **Crear un gráfico de peticiones a la API:**  
1️⃣ Ve a **Visualizations > Create Visualization**  
2️⃣ Selecciona **Bar Chart**  
3️⃣ En **Eje X**, elige `@timestamp`  
4️⃣ En **Eje Y**, selecciona `Count`  
5️⃣ Filtra por `message: "/hello"`  
6️⃣ Guarda y añade a un **Dashboard**  

---
