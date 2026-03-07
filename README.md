# Taller de Automatizacion con n8n

Material completo para el taller introductorio de n8n.

## Estructura

```
taller.n8n/
├── presentacion/
│   └── taller-n8n.md          # Slides del taller (formato Marp)
├── flujos/
│   ├── 01-primer-webhook.json  # Flujo 1: Webhook basico con respuesta
│   └── 02-webhook-con-ia.json  # Flujo 2: Webhook + ChatGPT (OpenAI)
└── material/
    ├── cheatsheet.md           # Referencia rapida de expresiones y nodos
    └── ejercicios.md           # 10 ejercicios progresivos
```

## Como usar este material

### Generar el PDF de la presentacion

La presentacion esta en formato **Marp** (Markdown para slides). Para exportar a PDF:

```bash
# Con npx (sin instalar nada)
npx @marp-team/marp-cli presentacion/taller-n8n.md --pdf

# O exportar a HTML interactivo
npx @marp-team/marp-cli presentacion/taller-n8n.md --html
```

Tambien puedes instalar la extension **Marp for VS Code** y exportar desde el editor.

### Importar los flujos en n8n

1. Abre n8n en tu navegador
2. Ve a **Workflows** > boton **+** > **Import from file** (o pega el JSON con Ctrl+Shift+V)
3. Selecciona el archivo `.json` correspondiente
4. Guarda el flujo

### Flujo 1 — Primer Webhook

Webhook simple que recibe un nombre y devuelve un saludo personalizado.

**Prueba rapida:**
```bash
curl -X POST http://localhost:5678/webhook-test/primer-webhook \
  -H "Content-Type: application/json" \
  -d '{"nombre": "Ana"}'
```

### Flujo 2 — Webhook con IA

Recibe una pregunta, la envia a ChatGPT y devuelve la respuesta.

**Prerequisito**: Crear credencial OpenAI en n8n con tu API key.

**Prueba rapida:**
```bash
curl -X POST http://localhost:5678/webhook-test/preguntar-ia \
  -H "Content-Type: application/json" \
  -d '{"pregunta": "Que es n8n?", "usuario": "Ana"}'
```

## Instalar n8n localmente

```bash
# Con npx (sin instalacion permanente)
npx n8n

# Con Docker
docker run -it --rm --name n8n -p 5678:5678 \
  -v ~/.n8n:/home/node/.n8n \
  docker.n8n.io/n8nio/n8n

# Acceder en: http://localhost:5678
```

## Nivel del taller

- **Publico**: Principiantes sin experiencia previa en n8n
- **Duracion estimada**: 3-4 horas
- **Prerequisitos**: Navegador web y acceso a n8n
