# 🚀 SATE-I: Sistema de Pre-autorización Quirúrgica y Triaje de Emergencias con IA

A continuación se detalla la solución desarrollada para la **HackIAthon 2026**. SATE-I es un agente inteligente diseñado para optimizar, validar y acelerar el proceso de pre-autorización médica en cirugías y triaje en entornos hospitalarios de emergencia, reduciendo tiempos críticos de respuesta.

---

## 🌐 Enlace Público del Agente Funcional

El agente se encuentra desplegado y completamente operativo en la siguiente URL de producción:

🔗 **[https://webhooks.datacoreecuadorcompetition.online/]**

> 💡 **Nota para el Jurado:** El agente responde de forma automatizada a través de este endpoint de producción.

---

## 🛠️ Stack Tecnológico y Arquitectura

El sistema se diseñó bajo una arquitectura ligera, modular y de alta disponibilidad:

*   **Core del Agente / Backend:** Hono.js ejecutado sobre la infraestructura Edge de Cloudflare (Cloudflare Workers).
*   **Base de Datos:** Cloudflare D1 (SQL nativo en el Edge) para el almacenamiento de registros y estados del triaje.
*   **Modelos de IA:** Integración de LLMs optimizados para inferencia de baja latencia mediante el consumo de APIs externas especializadas.
*   **Túneles y Red:** Despliegue seguro utilizando Cloudflare Tunnels para la interconexión de entornos aislados.

---

## 🚀 Instrucciones de Despliegue Local

Para levantar el entorno de desarrollo local, sigue estos pasos:

### Prerrequisitos
* Node.js (versión 18 o superior)
* Wrangler CLI (`npm install -g wrangler`) para la gestión de Cloudflare Workers.

### Pasos
1. **Clonar el repositorio:**
   
```bash
   git clone [https://github.com/DATACOREECUCOMPETITION/hackiathon-ia.git](https://github.com/DATACOREECUCOMPETITION/hackiathon-ia.git)
   cd hackiathon-ia
