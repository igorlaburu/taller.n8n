---
marp: true
theme: default
paginate: true
style: |
  section {
    font-family: 'Segoe UI', Arial, sans-serif;
    font-size: 26px;
    background: #ffffff;
    color: #1a1a2e;
  }
  section.cover {
    background: linear-gradient(135deg, #1a1a2e 0%, #16213e 60%, #0f3460 100%);
    color: #ffffff;
    justify-content: center;
    align-items: flex-start;
    padding: 60px;
  }
  section.cover h1 { color: #e94560; font-size: 52px; margin-bottom: 10px; }
  section.cover h2 { color: #a8b2d8; font-size: 28px; font-weight: 300; }
  section.cover p  { color: #8892b0; }
  section.dark {
    background: #1a1a2e;
    color: #cdd6f4;
  }
  section.dark h1 { color: #e94560; }
  section.dark h2 { color: #89b4fa; }
  h1 { color: #e94560; }
  h2 { color: #0f3460; }
  table { font-size: 22px; }
  th { background: #0f3460; color: white; }
  tr:nth-child(even) { background: #f0f4ff; }
  code { background: #f0f4ff; color: #e94560; padding: 2px 6px; border-radius: 4px; font-size: 22px; }
  pre { background: #1e1e2e; color: #cdd6f4; padding: 20px; border-radius: 8px; font-size: 18px; }
  blockquote {
    border-left: 4px solid #e94560;
    padding-left: 16px;
    color: #555;
    font-style: italic;
  }
---

<!-- _class: cover -->

# Automatizacion con IA y n8n

## De la teoria a tu primer flujo en produccion

<br>

**Antes de empezar — escanea el codigo QR**

```
  +---------------------------+
  |                           |
  |   [  INSERTAR QR AQUI  ]  |
  |                           |
  |  bit.ly/encuesta-taller   |
  +---------------------------+
```

_Mientras rellenais la encuesta, comenzamos._

---

# Agenda

| Bloque | |
|---|---|
| La Inteligencia Artificial hoy | ~20 min |
| Sorpresa: vuestras respuestas, analizadas | ~10 min |
| Como funciona n8n — fundamentos | ~15 min |
| Replicamos el flujo juntos | ~20 min |
| Agente IA con herramientas | ~15 min |
| Casos de uso + preguntas | ~10 min |

---

# Una historia muy corta

```
1950  Alan Turing propone la "maquina que piensa"
1956  Nace el termino "Inteligencia Artificial"
1980s Sistemas expertos basados en reglas
1990s Machine Learning: aprender de datos
2012  Deep Learning: redes neuronales profundas
2017  Transformers: la arquitectura que lo cambio todo
2022  ChatGPT: la IA llega al publico general
2024  Agentes IA, multimodalidad, razonamiento
2026  IA integrada en todo flujo de trabajo
```

> El ritmo de avance se ha multiplicado por 10 en los ultimos 3 anos.

---

# Que es un modelo de lenguaje (LLM)?

Una red neuronal entrenada con enormes cantidades de texto que aprende a predecir la siguiente palabra — y con eso, aprende a razonar, escribir, traducir, programar y resumir.

**No es magia. Es estadistica a escala planetaria.**

| Modelo | Empresa | Nota |
|---|---|---|
| GPT-4o | OpenAI | El mas conocido |
| Claude 4 | Anthropic | Razonamiento avanzado |
| Gemini | Google | Ecosistema Google |
| Llama 3 | Meta | Open source, corre en local |
| Mistral | Europea | Eficiente y open source |

---

# Que puede hacer un LLM?

| Tarea | Ejemplo real |
|---|---|
| **Redactar** | Emails, informes, propuestas |
| **Resumir** | Documentos largos en bullet points |
| **Clasificar** | "Es esto una queja, consulta o elogio?" |
| **Extraer** | "Dame el numero de factura de este PDF" |
| **Traducir** | Con contexto y tono apropiado |
| **Programar** | Generar, revisar y explicar codigo |
| **Analizar** | Patrones en texto libre, sentimiento |
| **Conversar** | Chatbots con contexto y memoria |

---

# Que son los Agentes IA?

Un **LLM** responde preguntas.
Un **Agente IA** toma decisiones y usa herramientas para lograr un objetivo.

```
Usuario: "Busca los ultimos 3 informes de ventas
          y hazme un resumen ejecutivo"

Agente:
  1. Decide que necesita buscar archivos
  2. Usa la herramienta "buscar en Google Drive"
  3. Lee los documentos encontrados
  4. Formula el resumen con la informacion real
  5. Devuelve el informe listo
```

El agente **planifica**, **actua** y **observa** en bucle hasta completar el objetivo.

---

# Limitaciones actuales (importantes)

- **Alucinaciones**: puede inventar hechos con total confianza
- **Fecha de corte**: el modelo no sabe lo que paso despues de su entrenamiento
- **Contexto limitado**: olvida lo que se dijo hace mucho en la conversacion
- **No tiene memoria real**: cada sesion empieza de cero (sin arquitectura adicional)
- **No razona, predice**: la logica formal estricta todavia falla en casos complejos
- **Coste por token**: procesar textos muy largos tiene coste
- **Sesgo**: refleja los sesgos del texto con el que fue entrenado

> La IA es un colaborador muy capaz pero no es omnisciente ni infalible.

---

# Estado del arte y aplicaciones

**Lo que ya es realidad:**
- Modelos multimodales (texto + imagen + audio + video)
- Agentes que usan el ordenador (navegan, hacen clic, rellenan formularios)
- Razonamiento extendido: piensan antes de responder (o1, DeepSeek-R1)
- Modelos locales (Llama, Mistral, Phi) — sin mandar datos a la nube
- RAG: conectar LLMs con tu documentacion privada

**En organizaciones:**
- Atencion al cliente: chatbot 24/7, clasificacion de tickets
- RRHH: criba de CVs, FAQs internas, onboarding
- Operaciones: facturas, resumen de reuniones, informes automaticos
- Legal: revision de contratos, extraccion de clausulas

---

# Que esperar de algo hecho con IA

- No es perfecto desde el primer dia — se ajusta y mejora con el tiempo
- Necesita datos de calidad para dar resultados de calidad
- El 20% de casos raros consume el 80% del tiempo de desarrollo
- El mayor valor esta en tareas repetitivas y de volumen alto
- La supervision humana sigue siendo necesaria en decisiones criticas

> La pregunta no es **"puede la IA hacerlo?"**
> La pregunta es **"tiene sentido automatizarlo?"**

---

# Lo que acaba de pasar mientras hablabamos

Cuando escaneaste el QR y rellenaste la encuesta:

```
Tu telefono
    |
    | [Formulario n8n nativo]
    v
[n8n Form Trigger]
    |
    v
[Guardar en n8n Table "respuestas_taller"]
    |
    v
Pantalla: "Gracias por participar!"
```

Cada respuesta se guardo automaticamente.
Nadie la proceso manualmente. **Ahora vamos a ver el analisis.**

---

<!-- _class: dark -->

# [ DEMO EN VIVO ]

## Abrir en el navegador:

```
https://TU-N8N/webhook/generar-informe
```

_El flujo leera todas las respuestas de la tabla,
las enviara a ChatGPT y generara una pagina HTML
con el analisis en tiempo real._

---

# Que es n8n

**n8n** es una plataforma de automatizacion de flujos de trabajo visual y open source.

- Mas de 400 integraciones (nodos)
- Interfaz visual de arrastrar y soltar
- Soporta logica con JavaScript cuando lo necesitas
- Self-hosted (tu servidor) o n8n.cloud
- Gratis para uso propio en self-hosted

**Por que n8n y no Zapier o Make?**
- Sin limite de operaciones en self-hosted
- Tus datos no salen de tu servidor
- Puedes integrar cualquier API con HTTP Request

---

# El Canvas y los elementos basicos

```
+-------------------------------------------------------+
|  n8n  | Workflows  Executions  Credentials  Settings  |
+-------------------------------------------------------+
|                                                       |
|  [Trigger] ----> [Nodo A] ----> [Nodo B] ----> [End]  |
|                                                       |
|  [+] Nuevo nodo    [Save]  [Execute]  [Active: OFF]   |
+-------------------------------------------------------+
```

- **Trigger**: primer nodo, lo que inicia el flujo (1 por flujo)
- **Nodos**: cada paso del proceso — recibe items, los transforma, los pasa
- **Flechas**: los datos fluyen por ellas
- **Active ON**: el flujo responde a eventos reales en produccion

---

# Tipos de Trigger

| Trigger | Cuando se activa |
|---|---|
| **Webhook** | Cuando alguien hace una peticion HTTP a la URL |
| **Form Trigger** | Cuando alguien envia un formulario n8n |
| **Schedule** | En un horario (cada hora, cada lunes...) |
| **Email** | Al recibir un email |
| **Manual** | Solo cuando tu pulsas "Execute" |
| **App trigger** | Al ocurrir un evento en Slack, Gmail, etc. |

---

# Tipos de Nodos

| Categoria | Ejemplos |
|---|---|
| **Datos** | Set, Code, Aggregate, Split Out |
| **Control de flujo** | IF, Switch, Merge, Loop |
| **HTTP / APIs** | HTTP Request, Webhook |
| **Almacenamiento** | n8n Tables, Google Sheets, Postgres |
| **IA** | AI Agent, OpenAI, Anthropic, Ollama |
| **Comunicacion** | Gmail, Slack, Telegram, SMTP |
| **Archivos** | Read/Write File, Extract from PDF |

---

# Los datos: Items y JSON

Todo en n8n son **items** — objetos JSON que viajan de nodo en nodo.

```json
[
  {
    "nombre": "Ana",
    "rol": "Directora de Operaciones",
    "experiencia": "He probado ChatGPT",
    "proceso_a_automatizar": "Gestion de facturas"
  },
  {
    "nombre": "Luis",
    "rol": "Tecnico IT",
    "experiencia": "Lo uso habitualmente",
    "proceso_a_automatizar": "Alertas de sistemas"
  }
]
```

Cada nodo recibe items, los procesa y produce nuevos items.

---

# Expresiones y Credenciales

**Expresiones** — datos dinamicos dentro de la configuracion: `{{ expresion }}`

```
{{ 'Hola, ' + $json.nombre + '!' }}          →  "Hola, Ana!"
{{ $now.format('DD/MM/YYYY') }}              →  "07/03/2026"
{{ $('Otro Nodo').item.json.campo }}         →  valor de otro nodo
{{ $json.texto.toUpperCase() }}              →  "TEXTO EN MAYUSCULAS"
```

Para activar: clic en el campo > icono **{}** en la esquina.

**Credenciales** — API keys y tokens guardados de forma segura:
- Settings > Credentials > Add Credential
- El nodo las usa sin exponer el valor en pantalla
- Nunca escribas un API key directamente en un campo

---

# Los dos flujos — vistos desde dentro

**Flujo 1: Formulario de Encuesta**
```
[n8n Form Trigger]  →  [Set: limpiar campos]  →  [n8n Table: Insert Row]
```

**Flujo 2: Generar Informe HTML**
```
[Webhook GET]
    →  [n8n Table: Get All Rows]
    →  [Aggregate: juntar todos los items]
    →  [Code: formatear texto para la IA]
    →  [OpenAI: analizar y redactar]
    →  [Set: construir HTML]
    →  [Respond to Webhook: Content-Type text/html]
```

Abrimos ambos flujos en n8n ahora.

---

# Replica: Flujo de Encuesta

**Pasos en n8n:**

1. Nuevo workflow > **n8n Form Trigger**
   - Titulo: "Encuesta del Taller"
   - Campos: Nombre (text), Sector (dropdown), Experiencia IA (dropdown), Proceso a automatizar (textarea)

2. Nodo **Set** — limpiar y renombrar campos con expresiones `{{ $json['Nombre del campo'] }}`

3. Nodo **n8n > Table Record > Create**
   - Tabla: "respuestas_taller"
   - Campos mapeados

4. Guardar > Activar > Probar desde el movil

---

# Replica: Flujo de Informe

**Pasos en n8n:**

1. Nuevo workflow > **Webhook** (GET, "When last node finishes")

2. **n8n > Table Record > Get Many** — tabla "respuestas_taller", Return All

3. **Aggregate** — "Aggregate All Item Data" en campo "respuestas"

4. **Code** — convertir el array en texto estructurado para OpenAI

5. **OpenAI** — modelo gpt-4o-mini, prompt de analisis, max 800 tokens

6. **Set** — construir el HTML con el resultado `{{ $json.message.content }}`

7. **Respond to Webhook** — body: el HTML, header Content-Type: text/html

---

# LLM directo vs Agente IA

**LLM directo** — lo que hicimos en el flujo de informe:
```
Pregunta → OpenAI → Respuesta
Solo sabe lo que aprendio hasta su fecha de entrenamiento
```

**Agente con herramientas** — el siguiente flujo:
```
Pregunta → Agente decide → Busca en Wikipedia → Lee resultado
         → Agente decide → Ya tiene suficiente → Formula respuesta
```

En n8n el **AI Agent** tiene sub-nodos conectados por debajo:
- **Chat Model**: el LLM que razona (OpenAI, Claude, Ollama...)
- **Memory**: recuerda la conversacion (Buffer Window)
- **Tools**: herramientas que puede usar (Wikipedia, HTTP, SQL...)

---

# Flujo 3: Agente con Wikipedia + Memoria

```
[Webhook POST]
      |
  [AI Agent]  ←  system: "Eres un asistente. Responde en espanol.
      |              Usa Wikipedia para info factual."
      |
   sub-nodos:
   ├── [OpenAI Chat Model: gpt-4o-mini]
   ├── [Window Buffer Memory: sesion_id del body]
   └── [Wikipedia Tool]
      |
[Set: formatear respuesta]
      |
[Respond to Webhook]
```

Prueba con: `{"pregunta": "Quien fue Alan Turing?", "sesion_id": "user-01"}`
Segundo mensaje: `{"pregunta": "Y que aporto a la informatica?", "sesion_id": "user-01"}`

---

# Patrones de automatizacion en organizaciones

**Captura → Enriquece → Almacena**
```
Formulario → IA clasifica/extrae → Base de datos
```
Facturas, solicitudes internas, leads, incidencias

**Evento → Analiza → Notifica**
```
Email/webhook recibido → IA analiza urgencia → Slack si es critico
```
Soporte al cliente, monitorizacion, alertas

**Programado → Agrega → Informa**
```
Cada lunes → Recoge datos de la semana → IA resume → Email al equipo
```
Informes automaticos, digests, KPIs

**Pregunta → Agente busca → Responde**
```
Chat → Agente con acceso a documentos internos → Respuesta
```
Chatbot de RRHH, soporte tecnico nivel 1, FAQ interna

---

# Recursos para seguir

**n8n:**
- `docs.n8n.io` — documentacion completa
- `community.n8n.io` — foro con miles de soluciones
- `n8n.io/workflows` — templates listos para importar

**IA:**
- `platform.openai.com` — API de OpenAI (necesitas credito)
- `console.anthropic.com` — API de Claude

**Instalar n8n en local:**
```bash
npx n8n                                          # con Node.js
docker run -p 5678:5678 docker.n8n.io/n8nio/n8n  # con Docker
```

**Material de este taller:** flujos JSON + cheatsheet en `/flujos/` y `/material/`

---

<!-- _class: cover -->

# Preguntas?

<br>

**Material del taller:**

```
github.com / [TU-USUARIO] / taller-n8n
```

_Gracias por participar_
