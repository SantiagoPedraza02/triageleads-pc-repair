# Triage de Leads — Taller de Reparación de PC

Sistema de automatización que recibe consultas de clientes por Gmail, 
las clasifica y diagnostica con IA (Claude), pasa por un punto de 
aprobación humana (Human-in-the-Loop) antes de cualquier acción hacia 
el cliente, y responde por el canal correspondiente (Gmail y/o 
WhatsApp), dejando todo registrado en una base de datos relacional 
en Notion.

**Resumen del pipeline:**
1. **Trigger**: Gmail Trigger (modo "From now on", sin procesar historial viejo)
2. **IA**: Claude clasifica prioridad, calcula un puntaje y genera un diagnóstico preliminar
3. **Resiliencia**: reintentos automáticos ante fallos de la API + registro de errores en Notion
4. **Base relacional**: cada consulta ("Lead") queda vinculada a un registro de "Cliente" en Notion, evitando datos aislados
5. **Human-in-the-Loop**: el flujo se detiene con un nodo Wait hasta que un humano aprueba desde Slack
6. **Salida multicanal**: una vez aprobado, se responde por Gmail (mismo hilo) y/o WhatsApp (con plantilla aprobada)
7. **Dashboard**: vista de Notion agrupada por estado, con KPIs de volumen y tasa de error

## Herramientas usadas

- **n8n** — orquestación del flujo
- **Notion** — base de datos (Leads, Clientes, Errores) + dashboard
- **Claude (Anthropic)** — clasificación y diagnóstico de consultas
- **Gmail** — canal de entrada y respuesta al cliente
- **Slack** — notificación y punto de aprobación humana
- **WhatsApp Business API** — notificación proactiva al cliente


## Links
- **VIDEO SOBRE EL WORFLOW FUNCIONANDO**: [https://youtu.be/vOJ5kR5lkGc]
- **Base de datos (modo lectura)**: [https://discovered-sodalite-4b4.notion.site/e6f50502bc684d3c87d2b2e7635415c3?v=1975cd0246ba47aa8b53b0c84cece25f]

## Seguridad

Las credenciales (API keys, tokens de OAuth) no están incluidas en este 
repositorio ni visibles en las capturas de pantalla. El manejo de datos 
sigue un criterio de minimización: solo se envían a la IA los campos 
estrictamente necesarios para el diagnóstico, sin datos personales 
identificables. 
