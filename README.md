🏥 SATE-I: Sistema de Auditoría de Triaje Automático y Emergencias
🏗️ Arquitectura del Sistema e Infraestructura
El sistema se diseñó bajo una arquitectura híbrida, modular y altamente resiliente, optimizada para entornos de misión crítica donde la disponibilidad y el aislamiento de datos son fundamentales. En lugar de depender de entornos monolíticos o pesados, se optó por un enfoque desacoplado que combina la flexibilidad de contenedores livianos con el rendimiento de servicios nativos.

🧠 Core del Backend y Orquestación de Procesos
Motor de Flujos: Despliegue nativo de N8N corriendo bajo un servicio de sistema (systemd) en el host principal. Este motor actúa como el cerebro del sistema, ejecutando la lógica de negocio, el procesamiento de las reglas del Triaje Manchester (regulado por el MSP) y la intercomunicación de capas.

Tolerancia a Fallos (Triple-LLM Failover Circuit): Para mitigar caídas de servicio o errores de cuota (503 Service Unavailable / 429 Rate Limit), se implementó un pipeline en cascada con 3 instancias consecutivas de la API de Google Gemini (Gemini 1.5 Flash y Pro) con control de errores activo (onError: continue). Si la instancia principal falla o experimenta latencia elevada, el flujo conmuta en tiempo real a las instancias de respaldo. En caso de un colapso general de red, el sistema activa un Bypass Catastrófico Local que fuerza un dictamen estructurado para garantizar que la SPA nunca se rompa de cara al usuario.

🗄️ Capa de Datos y Almacenamiento
Motor de Base de Datos: PostgreSQL 17 dockerizado sobre una imagen ultra-liviana basada en Alpine Linux.

Aislamiento y Persistencia: El contenedor expone el puerto 5432 hacia el host de forma segura, permitiendo que el servicio nativo de N8N persista los historiales de emergencia, estructuras de pólizas y análisis clínico-legales sin contaminar el entorno global del servidor.

🌐 Capa de Presentación (Frontend SPA Premium)
Interfaz de Usuario: Una Single Page Application (SPA) moderna, responsiva y diseñada bajo una interfaz limpia de modo claro (light theme).

Servidor Web y Proxy Inverso: Nginx dockerizado en Alpine Linux expone el puerto 80 del host, sirviendo el frontend estático y gestionando las reglas de ruteo inverso para redirigir de forma segura las peticiones hacia los endpoints correspondientes (/webhook/ e ingresos de emergencia).

Mecanismo de Contingencia del Cliente: El frontend incluye un script de interceptación inteligente; si el backend devuelve un error interno (HTTP 500) o hay problemas de conectividad en el túnel, la SPA conmuta inmediatamente a una simulación clínica y legal exacta para mantener la demo operativa bajo cualquier circunstancia.

🛡️ Red y Seguridad (Seguridad Perimetral)
Conectividad Segura: Despliegue de Cloudflare Tunnels (cloudflared) dockerizados independientes. Esta arquitectura elimina la necesidad de abrir puertos públicos en el router o configurar reglas de firewall complejas en el host.

Topología de Subdominios: El tráfico se segmenta de manera modular mediante tres túneles independientes amarrados a tokens específicos, exponiendo los servicios hacia la web con cifrado HTTPS automático:
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
