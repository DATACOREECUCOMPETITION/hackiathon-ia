## 🏗️ Arquitectura del Sistema

El sistema está diseñado bajo una arquitectura ligera, modular y de alta disponibilidad con tolerancia a fallos, optimizada específicamente para las demandas críticas de un entorno hospitalario y el despliegue ágil de un Hackathon de IA.
---
[ Frontend SPA ] ──( HTTPS )──> [ Nginx Reverse Proxy ]
                                         │
                                         ├──> [ Servidor Estático (UI) ]
                                         └──> [ Webhooks n8n (Orquestador) ]
                                                     │
                        ┌────────────────────────────┴────────────────────────────┐
                        ▼                                                         ▼
         [ PostgreSQL 17 (Docker) ]                                  [ Infraestructura IA (Failover) ]
       (n8n_data_hackaton / CIE-10)                                 ├── 1. Gemini 2.5 Flash (Principal)
                                                                    ├── 2. Gemini 2.5 Flash (Respaldo)
                                                                    ├── 3. Gemini 2.5 Flash (Contingencia)


 
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

Gemini 2.5 Flash (Respaldo de Alta Capacidad)

Gemini 2.5 Flash (Segunda Contingencia)

⚠️ Mecanismo de Mitigación: Si las APIs externas presentan latencias críticas o caídas de red, el flujo ejecuta de forma instantánea un bypass catastrófico local mediante JavaScript dentro de n8n, asegurando que el triaje de emergencia nunca se detenga.

🛡️ Conectividad y Red Segura (Cloudflare Tunnels)
La plataforma implementa una interconexión segura que expone los servicios a internet sin necesidad de abrir puertos en el firewall ni exponer IPs públicas directas. Esto se logra mediante Cloudflare Tunnels utilizando tres contenedores independientes de cloudflared para aislar el tráfico:

cloudflared-index: Acceso al Frontend principal.

cloudflared-panel: Acceso al panel de administración.

cloudflared-webhooks: Canal exclusivo para las llamadas críticas a la API de triaje.
---
<img width="1731" height="628" alt="image" src="https://github.com/user-attachments/assets/4d1e79b7-8077-4ac3-8570-98836af9d37e" />


## 🚀 Instrucciones de Despliegue Local

Para levantar el entorno de desarrollo local, sigue estos pasos:

### Prerrequisitos
* Node.js (versión 18 o superior)
* Wrangler CLI (`npm install -g wrangler`) para la gestión de Cloudflare Workers.

### Pasos
1. **Clonar el repositorio:**
   
```bash
   git clone [https://github.com/DATACOREECUCOMPETITION/hackiathon-ia.git]
   cd hackiathon-ia
