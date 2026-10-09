# 🚀 PoC Infraestructura Web - Guadalquivir Cloud Tech

Prueba de Concepto (PoC) para el despliegue y validación de una plataforma web estática en un entorno contenedorizado sobre **Nginx** y **Docker Compose**.

---

## 🛠️ Tecnologías Utilizadas

* **Docker & Docker Compose**: Contenedorización de la aplicación y orquestación del servicio.
* **Nginx (Alpine Linux)**: Servidor web de alto rendimiento y bajo consumo (`nginx:1.27-alpine`).
* **HTML5 & CSS3**: Código fuente de la landing page estática y hoja de estilos corporativa.
* **Windows 11 / WSL2**: Entorno de desarrollo anfitrión.
* **curl / PowerShell**: Herramientas de terminal para la verificación de cabeceras HTTP y Hardening.

---

## 📁 Estructura del Proyecto

```text
ngnix-static-poc/
├── conf/
│   └── default.conf          # Configuración del servidor web (VirtualHost + Hardening)
├── html/
│   ├── css/
│   │   └── style.css         # Estilos visuales de la aplicación
│   ├── img/
│   │   └── guadalquivir-recorrido.webp  # Imagen estática de la landing page
│   └── index.html            # Documento principal HTML5
├── docker-compose.yml        # Configuración del contenedor y mapeo de volúmenes/puertos
└── README.md                 # Documentación del proyecto
---

## 🧪 Validación y Evidencias del Despliegue

### 1. Puesta en Marcha del Contenedor

Una vez creados todos los archivos del proyecto, procedemos a levantar los servicios en segundo plano con el comando `docker compose up -d`.

<img width="1524" height="1079" alt="Captura de pantalla 2026-10-09 092523" src="https://github.com/user-attachments/assets/23229e27-12b3-4a78-bcfb-7a4dc5d47c86" />
### 2. Resolución de Incidencia de Puerto Ocupado

Al desplegar, surgió un conflicto porque el puerto `8080` estaba ocupado por otro contenedor previo. Se solucionó deteniendo los contenedores activos mediante `docker stop $(docker ps -q)`, permitiendo levantar el entorno de nuevo sin problemas.

<img width="960" height="1050" alt="Captura de pantalla 2026-10-09 092537" src="https://github.com/user-attachments/assets/6a09b386-b38c-4423-a2a1-534b553d323b" />
### 3. Comprobación de la Web en Navegador

Al ingresar a `http://localhost:8080/`, confirmamos que el contenedor sirve correctamente el HTML, CSS y los assets montados en el volumen.

<img width="642" height="277" alt="Captura de pantalla 2026-10-09 093030" src="https://github.com/user-attachments/assets/87662c85-b689-40fc-8b65-f9bb23a491e9" />

### 4. Auditoría de Seguridad y Hardening (Cabeceras HTTP)

Para validar la seguridad del servidor web, se ejecutó en la terminal el comando `curl.exe -I http://localhost:8080`. Esto realiza una petición HEAD que devuelve únicamente las cabeceras HTTP de respuesta.

Se comprueba que la versión exacta de Nginx permanece oculta gracias a la directiva `server_tokens off;` de nuestro archivo `conf/default.conf`, reduciendo la exposición a posibles vulnerabilidades.


