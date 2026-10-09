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
## Validación 
* Una vez he creado todos los archivos procedo a subir los contenedores con docker compose up -d
<img width="1524" height="1079" alt="Captura de pantalla 2026-10-09 092523" src="https://github.com/user-attachments/assets/23229e27-12b3-4a78-bcfb-7a4dc5d47c86" />
* Y ya está corriendo. Tenia un problema que ya previamente tenía otro contenedor en el mismo puerto por lo que lo he tenido que cerrar con docker stop $(docker ps -q), y una vez cerrado ya he podido subirlo sin problema
<img width="960" height="1050" alt="Captura de pantalla 2026-10-09 092537" src="https://github.com/user-attachments/assets/6a09b386-b38c-4423-a2a1-534b553d323b" />
* Una vez accede a http://localhost:8080/ , se ve que funciona todo en el contenedor
<img width="642" height="277" alt="Captura de pantalla 2026-10-09 093030" src="https://github.com/user-attachments/assets/87662c85-b689-40fc-8b65-f9bb23a491e9" />
* Para validad la seguridad y la configuración del servidor web, he ejecutado en la terminal el comando curl.exe -I http://localhost:8080. Esto me ha permitido realizar una petición de tipo HEAD a la infraestructura, solicitando solo las cabeceras HTTP del servidor sin descargar el cuerpo de la página. De lo mas relevante se puede observar es que la versión de nginx esta oculta gracias a sever_tockens off; que añadi en conf/default.



