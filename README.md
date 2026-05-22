## 🏗️ Arquitectura del Sistema

El sistema está diseñado bajo una arquitectura ligera, modular y de alta disponibilidad con tolerancia a fallos, optimizada específicamente para las demandas críticas de un entorno hospitalario y el despliegue ágil de un Hackathon de IA.
---
<img width="813" height="267" alt="image" src="https://github.com/user-attachments/assets/445b89b5-994e-4885-a7e2-9cfe592d6641" />
---
🧱 Componentes Core
🧠 Core del Agente & Backend (Orquestación con n8n y Nginx)
La lógica de negocio, la validación clínica y la ingesta de alertas de emergencia se orquestan mediante flujos de trabajo visuales en n8n (Self-hosted). Las peticiones entrantes desde la interfaz de usuario (Single Page Application) son administradas por un proxy inverso Nginx, encargado de redirigir los webhooks hacia el motor de automatización y de servir la UI web estática de forma eficiente.

🗄️ Base de Datos (SQL sobre Docker)
Se emplea PostgreSQL 17 con persistencia de volumen en un contenedor Docker, utilizando la base de datos n8n_data_hackaton. Este motor almacena:

polizas: Historial de carencias y preexistencias clínicas codificadas bajo el estándar internacional CIE-10.

historial_emergencias: Registro en tiempo real de los estados de triaje y auditorías clínico-legales, alineados estrictamente con la normativa de salud vigente en Ecuador.

🤖 Infraestructura de IA Resiliente (Triple Failover con Gemini)
Para garantizar una disponibilidad del 99.9% en situaciones críticas de salud, se integró un sistema de contingencia en cascada que conmuta dinámicamente entre tres instancias de LLMs:

Gemini 2.5 Flash (Instancia Principal)
Gemini 2.5 Flash (Respaldo Contingencia)
Gemini 2.5 Flash (Segunda Contingencia)


🛡️ Conectividad y Red Segura (Cloudflare Tunnels)
La plataforma implementa una interconexión segura que expone los servicios a internet sin necesidad de abrir puertos en el firewall ni exponer IPs públicas directas. Esto se logra mediante Cloudflare Tunnels utilizando tres contenedores independientes de cloudflared para aislar el tráfico:

cloudflared-index: Acceso al Frontend principal.

cloudflared-panel: Acceso al panel de administración.

cloudflared-webhooks: Canal exclusivo para las llamadas críticas a la API de triaje.
---
<img width="1731" height="628" alt="image" src="https://github.com/user-attachments/assets/4d1e79b7-8077-4ac3-8570-98836af9d37e" />


## 🚀 Instrucciones de Despliegue Local

Sigue estos pasos para clonar, configurar y levantar el entorno de desarrollo local en tu máquina.

### 📋 Prerrequisitos

Antes de comenzar, asegúrate de tener instalado lo siguiente en tu sistema:

* **Node.js** (Versión 18.0 o superior)
* **Docker & Docker Compose** (Para el despliegue de la base de datos y contenedores de red)
* **Wrangler CLI** (Herramientas de línea de comandos global para Cloudflare Workers)
    ```bash
    npm install -g wrangler
    ```

---

### 🛠️ Pasos para la Instalación

#### 1. Clonar el Repositorio
Primeramente, clona el proyecto desde el repositorio oficial y accede al directorio raíz:
```bash
git clone [https://github.com/DATACOREECUCOMPETITION/hackiathon-ia.git](https://github.com/DATACOREECUCOMPETITION/hackiathon-ia.git)
cd hackiathon-ia
