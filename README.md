# PoC Infraestructura Web - Guadalquivir Cloud Tech

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
```
---
## wireframe de la web
<img width="1415" height="693" alt="wireframe" src="https://github.com/user-attachments/assets/58f4c760-29b3-4f52-a613-0c383437c495" />
Este es el wireframe de la web ya actualizada para el cliete Guadalquivir Cloud Tech.

## Landing ya en funcionando
<img width="1919" height="997" alt="image" src="https://github.com/user-attachments/assets/80ee47a9-a398-4933-b520-4c775c2943aa" />



## Validación y Evidencias del Despliegue

### 1. Puesta en Marcha del Contenedor

Una vez creados todos los archivos del proyecto, procedemos a levantar los servicios en segundo plano con el comando `docker compose up -d`.

<img width="1524" height="1079" alt="Captura de pantalla 2026-10-09 092523" src="https://github.com/user-attachments/assets/23229e27-12b3-4a78-bcfb-7a4dc5d47c86" />
### 2. Resolución de Incidencia de Puerto Ocupado

Al desplegar, surgió un conflicto porque el puerto `8080` estaba ocupado por otro contenedor previo. Se solucionó deteniendo los contenedores activos mediante `docker stop $(docker ps -q)`, permitiendo levantar el entorno de nuevo sin problemas.

<img width="960" height="1050" alt="Captura de pantalla 2026-10-09 092537" src="https://github.com/user-attachments/assets/6a09b386-b38c-4423-a2a1-534b553d323b" />
### 3. Comprobación de la Web en Navegador

Al ingresar a `http://localhost:8080/`, confirmamos que el contenedor sirve correctamente el HTML, CSS y los assets montados en el volumen.

<img width="642" height="277" alt="Captura de pantalla 2026-10-09 093030" src="https://github.com/user-attachments/assets/87662c85-b689-40fc-8b65-f9bb23a491e9" />

También he comprobado el estado de la web.
<img width="955" height="1003" alt="image" src="https://github.com/user-attachments/assets/783c2a7e-4cd0-4b37-bd81-d06e0c33f72b" />

Inspección de registros del contenedor mediante docker compose logs --tail=15, confirmando la captura de la petición HTTP GET con respuesta 200 OK
<img width="658" height="211" alt="image" src="https://github.com/user-attachments/assets/bacd3793-0d9e-48d0-888e-6ac0fb2703f8" />


## 4. Resolución de Incidencia Simulada (#INC-2026-081)

**Descripción:** Errores 403 Forbidden / 404 Not Found tras la actualización de assets e imágenes.

**Pasos de diagnóstico y resolución:**
1. **Monitorización de logs:** Se ejecutó `docker compose logs -f` para identificar el código de respuesta HTTP exacto emitido por Nginx.
2. **Auditoría dentro del contenedor:** Se utilizó `docker compose exec web-poc sh` para validar los permisos POSIX del sistema de archivos interno.
3. **Correcciones aplicadas:**
   - **Permisos (403):** Se aplicaron permisos de lectura globales (`chmod -R 644 html/`) para asegurar el acceso al usuario `nginx`.
   - **Diferenciación Mayúsculas/Minúsculas (404):** Se corrigieron las rutas en `index.html` garantizando la correspondencia exacta (case-sensitivity) con el sistema de archivos Linux de Alpine.


