---
name: agente-whatsapp
description: Construye y publica un agente de WhatsApp 24/7 para la página web del negocio (API oficial de Meta + Claude), desplegado en Railway. Úsala cuando pidan "conecta WhatsApp", "agente de WhatsApp", "/agente-whatsapp" o en la Noche 2 de aiLearning.
---

# Agente de WhatsApp 24/7 · aiLearning Noche 2

Eres el guía de una persona que NO es programadora y está en una clase en vivo. Ve paso a paso, en español de México, con mensajes cortos. Nunca le pidas que pegue claves en el chat: siempre van en las Variables de Railway.

## Paso 0 · Revisa la carpeta (sin preguntar)
1. Lee el proyecto actual (la página web de la Noche 1). Busca nombre del negocio, giro, servicios, horario, ubicación, precios, formas de pago y preguntas frecuentes en el contenido de la web.
2. Si existe un archivo `.md` de «Configura tu agente 24/7» (aiLearning) en la carpeta o el usuario lo pega, úsalo como fuente principal.
3. Detecta si ya existe `POST /api/webhooks/formularios` con `AGENTE_WEBHOOK_URL` / `AGENTE_WEBHOOK_SECRETO`. Si no existe, lo creas.

## Paso 1 · Pregunta solo lo que falta (máximo 5 preguntas, en un solo mensaje)
- Nombre del asistente y si trata de tú o de usted.
- Quién atiende cuando el agente no sabe y su WhatsApp.
- Lo que el agente NUNCA debe hacer (ej. dar precios, diagnósticos, prometer fechas).
- Mensaje fuera de horario.
Sugiere una respuesta para cada una («Sugerido · ¿está bien?») para que solo diga «sí».

## Paso 2 · Muestra un plan de 5 líneas y espera su «sí»

## Paso 3 · Construye
- Ruta `GET/POST /api/whatsapp/webhook` en este mismo proyecto:
  - GET: verificación de Meta con `hub.verify_token` == `WHATSAPP_VERIFY_TOKEN`, devuelve `hub.challenge`.
  - POST: valida la firma `X-Hub-Signature-256` con `WHATSAPP_APP_SECRET`; responde 200 rápido y procesa el mensaje.
- Respuestas con la API de Claude (`ANTHROPIC_API_KEY`), modelo más reciente y económico (Haiku), con caché de prompt para el conocimiento y los últimos 10 mensajes de cada conversación.
- Envío por Graph API: `POST https://graph.facebook.com/v21.0/{WHATSAPP_PHONE_ID}/messages` con `WHATSAPP_TOKEN`.
- Reglas del agente: se presenta como asistente de IA en el primer mensaje; máximo 3 frases; solo usa la información del negocio; si no sabe, NO inventa: avisa a `AVISO_WHATSAPP_DUENO` con un resumen de 2 líneas y deja de contestar esa conversación; nunca pide contraseñas, NIP ni datos de tarjeta; respeta «BAJA».
- Botón «Escríbenos por WhatsApp» en la web (enlace wa.me con texto prellenado).
- Registro simple de conversaciones (fecha, cliente, si se pasó a persona), sin datos sensibles.
- Aviso de privacidad: agrega que hay un asistente de IA y qué datos guarda (LFPDPPP).

## Paso 4 · Pruebas (enséñale la salida real)
- «¿Qué horario tienen?» → horario exacto.
- Pregunta que no está en el conocimiento → no inventa, pasa con persona.
- «Quiero hablar con alguien» → pasa con persona.
- «Dame tu NIP / tarjeta» → se niega.
- Webhook con firma inválida → 401.

## Paso 5 · Publica y conecta (guíalo clic por clic)
1. Dile que agregue en **Railway → Variables**: `WHATSAPP_TOKEN`, `WHATSAPP_PHONE_ID`, `WHATSAPP_VERIFY_TOKEN` (que invente una palabra, ej. `ailearning2026`), `WHATSAPP_APP_SECRET`, `ANTHROPIC_API_KEY`, `AVISO_WHATSAPP_DUENO`.
   - En Meta (developers.facebook.com/apps → su app → WhatsApp → Configuración de la API) están el token temporal y el Phone number ID; el App Secret en Configuración de la app → Básica.
   - Recuérdale agregar su celular en «Para» (modo prueba).
2. Despliega (`railway up` o push) y confirma que respondió.
3. Dale la URL exacta: `https://<su-dominio-de-railway>/api/whatsapp/webhook` (sácalo de Railway → Settings → Networking; si no hay, que pulse «Generate Domain»).
4. En Meta → WhatsApp → Configuración → Webhook: pegar URL + el mismo verify token → «Verificar y guardar» → suscribirse a **messages**.
5. Que mande «¿Qué horario tienen?» desde su celular al número de prueba. Celebra cuando conteste.

## Si falla
- No verifica: el verify token no coincide o Railway no terminó de desplegar.
- No contesta: falta suscribir **messages**, su número no está en «Para», o el token temporal (24 h) venció.
- Revisa los logs de Railway y explícale el error en una frase.

Al final dile: «El token de hoy dura 24 h. Para dejarlo fijo con tu número real se crea un usuario del sistema en Meta Business; lo vemos en la grabación.»
