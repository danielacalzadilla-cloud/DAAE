# Bot DAAE — UPB Santa Cruz

Chatbot de preguntas frecuentes con backend propio. No depende de Poe
ni de que los estudiantes tengan cuenta de Claude — funciona como una
página web pública normal.

## ¿Qué necesitas antes de empezar?

1. **Una API key de Anthropic** (para que el bot pueda "pensar"):
   - Ve a https://console.anthropic.com
   - Crea una cuenta (o inicia sesión)
   - Ve a "API Keys" y genera una nueva key (empieza con `sk-ant-...`)
   - Nota: esto tiene un costo de uso (no es lo mismo que una cuenta
     de Claude.ai). El costo depende de cuántas preguntas hagan los
     estudiantes — para un bot de FAQ de bajo volumen, suele ser muy
     económico (unos centavos de dólar por cada 100 preguntas).

## Opción recomendada: desplegar en Render (gratis)

1. Crea una cuenta en https://render.com (puedes usar tu cuenta de
   GitHub o Google para registrarte).
2. Sube esta carpeta a un repositorio de GitHub:
   - Crea un repositorio nuevo (puede ser privado).
   - Sube todos estos archivos (`server.js`, `system-prompt.js`,
     `package.json`, la carpeta `public/`).
3. En Render, haz clic en **"New +" → "Web Service"**.
4. Conecta tu repositorio de GitHub.
5. Configura:
   - **Build Command:** `npm install`
   - **Start Command:** `npm start`
   - **Instance Type:** Free
6. En la sección **"Environment Variables"**, agrega:
   - Key: `ANTHROPIC_API_KEY`
   - Value: tu API key de Anthropic (la que copiaste de console.anthropic.com)
7. Haz clic en **"Create Web Service"**.
8. Espera 2-3 minutos a que termine el despliegue. Render te dará una
   URL pública como `https://daae-bot.onrender.com` — ese es el
   enlace que compartes con los estudiantes.

### Nota sobre el plan gratuito de Render
El plan free "duerme" el servidor tras ~15 minutos sin uso, así que la
primera pregunta después de un rato de inactividad puede tardar unos
segundos extra en responder mientras el servidor despierta. Para uso
educativo de bajo/medio volumen esto no suele ser un problema.

## Alternativas a Render
- **Railway** (railway.app) — proceso muy similar, también tiene plan gratuito.
- **Vercel** — requiere adaptar `server.js` a función serverless, un
  poco más de trabajo pero también gratuito.

## Probar en tu computadora antes de desplegar (opcional)

Si tienes Node.js instalado:

```bash
npm install
export ANTHROPIC_API_KEY="tu-api-key-aqui"
npm start
```

Luego abre http://localhost:3000 en tu navegador.

## Editar las preguntas frecuentes

Todo el contenido del FAQ está en `system-prompt.js`. Puedes editar
las preguntas y respuestas directamente en ese archivo (formato `P:` /
`R:`) y volver a desplegar — Render vuelve a desplegar automáticamente
cada vez que subes cambios a GitHub.

## Estructura del proyecto

```
daae-bot/
├── server.js          # Backend (Express) — recibe preguntas y llama a la API de Anthropic
├── system-prompt.js    # Base de preguntas frecuentes + instrucciones del bot
├── package.json         # Dependencias del proyecto
└── public/
    └── index.html       # Interfaz de chat que ven los estudiantes
```
