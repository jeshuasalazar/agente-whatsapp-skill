# Agente de WhatsApp 24/7 · Skill para Claude Code

Skill de **aiLearning** (Noche 2) que conecta un agente de WhatsApp a la página web de tu negocio: API oficial de Meta + Claude, desplegado en Railway.

## Instalar (1 minuto)

```bash
git clone https://github.com/jeshuasalazar/agente-whatsapp-skill.git
mkdir -p ~/.claude/skills && cp -R agente-whatsapp-skill/agente-whatsapp ~/.claude/skills/
```

## Usar

1. Abre la terminal en la carpeta de tu página web y escribe `claude`.
2. Escribe `/agente-whatsapp`.
3. Sigue los pasos: Claude lee tu web, te pregunta solo lo que falta, construye, prueba y te guía para conectarlo en Meta.

## Lo que necesitas

- Tu página web (la de la Noche 1) desplegada en Railway.
- Cuenta en [developers.facebook.com](https://developers.facebook.com/apps) (el número de prueba de Meta es gratis).
- Una llave de la API de Claude (`ANTHROPIC_API_KEY`).

> Las claves van siempre en **Railway → Variables**, nunca en el chat.

Hecho por [aiLearning](https://plataforma.ailearning.mx).
