# Asistente de ventas - UrbanStep - n8n

Workflow de automatización que atiende ventas por Telegram con un agente de IA, integrado a CRM, control de stock y seguimiento comercial, pensado para una tienda de e-commerce.

## 📋 Descripción

Automatiza el ciclo completo de ventas de UrbanStep (tienda de calzado urbano) desde la captura del lead hasta el cierre y el seguimiento post-venta: recibe clientes desde un formulario web, los atiende por Telegram con un agente de IA que conoce el stock real, cierra el pedido, deriva a un vendedor humano para el pago y mantiene al equipo de CRM, ventas y soporte sincronizados en todo momento.

- Captura de leads vía formulario web, con alta automática del cliente en la base de datos y en HubSpot CRM.
- Agente de IA (Claude, con GPT como modelo de respaldo) que consulta stock real por herramienta, arma el pedido (producto, talle entre 35 y 45, cantidad), aplica promociones vigentes y confirma la compra con el cliente.
- Distinción automática entre cliente nuevo y recurrente, con historial de compras disponible para el agente en cada conversación.
- Derivación a un vendedor humano por Telegram una vez confirmada la venta, con aviso al equipo de ventas por Slack y actualización del deal en HubSpot.
- Derivación separada para reclamos y consultas fuera de alcance, con aviso al equipo de soporte por un canal de Slack distinto.
- Descuento automático de stock y registro de la compra en la base de datos al confirmarse la venta.
- Capa de análisis con IA que clasifica cada conversación (sentimiento, intención de compra, etapa del funnel, objeción principal, prioridad) y usa ese contexto para que el agente ajuste tono y enfoque en mensajes futuros.
- Seguimiento comercial automático: un trigger programado detecta clientes sin respuesta hace 24 o 48 horas y les envía un email de reenganche.
- Encuesta NPS post-venta con clasificación automática del comentario según el puntaje.
- Buffer de mensajes para agrupar varios mensajes seguidos del mismo cliente antes de procesarlos como una sola consulta.
- Monitoreo de errores: cualquier nodo que falle envía una alerta a un canal de Slack con el motivo probable (credencial vencida, rate limit, error de SQL, etc.).

## 🔄 Flujo del proceso

1. El cliente completa un formulario web → se crea su ficha en la base de datos y un lead en HubSpot.
2. El cliente escribe por Telegram. Si todavía no completó el formulario, el bot se lo pide antes de continuar.
3. El agente de IA retoma sus datos (interés, categoría, talle) y el historial de compras para guiar la conversación.
4. Ante consultas de stock, precio o talle, el agente consulta el catálogo real antes de responder.
5. Al definir el pedido, lo confirma con el cliente y lo deriva a un vendedor humano para gestionar el pago.
6. Se descuenta el stock, se registra la compra y se notifica al equipo de ventas por Slack; en HubSpot se marca la oportunidad.
7. Una capa de IA analiza la conversación en segundo plano y clasifica al cliente para mejorar la atención futura.
8. Un proceso programado revisa diariamente a los clientes sin respuesta y dispara emails de seguimiento.

## 🛠 Tecnologías usadas

**Automatización:** n8n (orquestación del flujo completo)

**IA:** Anthropic Claude (agente conversacional y capas de análisis), OpenAI GPT (modelo de respaldo)

**Datos:** PostgreSQL / Supabase (clientes, formulario, compras, eventos, análisis), Redis (buffer de mensajes)

**Integraciones:** Telegram Bot API (canal de atención), HubSpot (CRM), Slack (notificaciones a ventas, soporte y errores), Gmail (emails de seguimiento), Formularios n8n (captura de leads y encuesta NPS)

![](https://skillicons.dev/icons?i=n8n,postgres,redis,js)

## 💻 Instalación y despliegue

```bash
# 1. Importar el workflow
# En tu instancia de n8n: Workflows → Import from File → seleccionar el .json de este repo

# 2. Configurar credenciales en n8n
# - Anthropic / OpenAI (API keys)
# - PostgreSQL o Supabase (base de datos de clientes, formulario, compras, eventos y análisis)
# - Redis (conexión para el buffer de mensajes)
# - Telegram (token del bot)
# - HubSpot (app token del CRM)
# - Slack (OAuth2, con acceso a los canales de ventas, soporte y alertas de errores)
# - Gmail (OAuth2, para los emails de seguimiento)

# 3. Crear el esquema de base de datos
# Tablas necesarias: clientes, formulario, compras_clientes, eventos, acciones,
# analisis, productos_stock, n8n_chat_histories

# 4. Reemplazar valores propios del negocio
# - IDs de los canales de Slack (ventas, soporte, alerta-errores)
# - Nombre del bot de Telegram en el mensaje de confirmación del formulario web
# - Catálogo de productos y stock (tabla productos_stock)
# - Reglas de promociones y condiciones (nodo "Aplicar Descuento/s1")

# 5. Activar el workflow en n8n
```

## 📫 Contacto

Si buscas un perfil especializado en automatización de procesos con IA, trabajando con n8n, SQL y JavaScript asistido por IA:

- 📧 Email: [marcosantini03@gmail.com](mailto:marcosantini03@gmail.com)
- 💼 LinkedIn: [LinkedIn](https://www.linkedin.com/in/marco-santini-a0391b309/)
- 📄 CV: [CV](https://docs.google.com/document/d/1rJgnOuMdzq41D2oTrjc76o5_w-R3_nv15-Lxg2TFE54/edit?usp=sharing)
