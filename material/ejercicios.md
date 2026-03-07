# Ejercicios del Taller de n8n

## Antes de empezar

Asegurate de tener n8n funcionando y accesible. Para importar un flujo:
1. Ve a **Workflows** > **Add Workflow** > icono de importar (o Ctrl+Shift+V)
2. Pega el contenido del archivo JSON
3. Guarda el flujo

---

## Ejercicio 1 — Explorar el Flujo Base (Webhook)

**Objetivo**: Entender como funciona el flujo `01-primer-webhook`.

**Pasos:**
1. Importa el flujo `flujos/01-primer-webhook.json`
2. Abre el nodo **Recibir Peticion** y copia la **Test URL**
3. Haz clic en **"Listen for test event"**
4. En otra terminal, ejecuta:
   ```bash
   curl -X POST [TU_TEST_URL] \
     -H "Content-Type: application/json" \
     -d '{"nombre": "TuNombre"}'
   ```
5. Observa los datos que llegan al nodo Webhook
6. Revisa la salida de cada nodo en el panel derecho

**Preguntas para reflexionar:**
- ¿En que nodo aparece el campo `nombre`?
- ¿Que diferencia hay entre la entrada y salida del nodo "Construir Respuesta"?
- ¿Que pasa si mandas el request sin el campo `nombre`?

---

## Ejercicio 2 — Modificar el Saludo

**Objetivo**: Practicar expresiones modificando un flujo existente.

**Pasos:**
1. En el flujo `01-primer-webhook`, edita el nodo **Construir Respuesta**
2. Modifica el campo `saludo` para que incluya la hora actual:
   ```
   Hola, {{ $json.body.nombre }}! Son las {{ $now.format('HH:mm') }}
   ```
3. Agrega un nuevo campo `nombre_mayusculas` con el nombre en mayusculas
4. Prueba el flujo y verifica la respuesta

**Resultado esperado:**
```json
{
  "saludo": "Hola, Ana! Son las 10:30",
  "nombre_recibido": "Ana",
  "nombre_mayusculas": "ANA",
  "timestamp": "2025-01-15 10:30:00",
  "longitud_nombre": 3
}
```

---

## Ejercicio 3 — Agregar una Condicion

**Objetivo**: Usar el nodo IF para ramificar el flujo.

**Pasos:**
1. En el flujo `01-primer-webhook`, agrega un nodo **IF** entre "Construir Respuesta" y "Responder al Cliente"
2. Configura la condicion: `longitud_nombre` es mayor que `5`
3. Conecta ambas ramas (**true** y **false**) a nodos separados de **Respond to Webhook**:
   - Rama true: responde `{ "mensaje": "Tienes un nombre largo!" }`
   - Rama false: responde `{ "mensaje": "Tienes un nombre corto!" }`
4. Prueba con nombres cortos (Ana, Luis) y largos (Alejandro, Valentina)

**Diagrama:**
```
[Construir Respuesta]
        |
       [IF] longitud > 5?
      /        \
  [true]      [false]
    |              |
[Resp: largo]  [Resp: corto]
```

---

## Ejercicio 4 — Llamar una API externa

**Objetivo**: Usar el nodo HTTP Request para consumir una API publica.

**Pasos:**
1. Crea un flujo nuevo
2. Agrega un nodo **Manual Trigger** (para ejecutar a mano)
3. Agrega un nodo **HTTP Request** con:
   - Method: `GET`
   - URL: `https://api.open-meteo.com/v1/forecast`
   - Query Parameters:
     - `latitude` = `40.4168`
     - `longitude` = `-3.7038`
     - `current_weather` = `true`
4. Agrega un nodo **Set** que extraiga solo:
   - `temperatura` = `{{ $json.current_weather.temperature }}`
   - `viento_kmh` = `{{ $json.current_weather.windspeed }}`
   - `ciudad` = `Madrid`
5. Ejecuta y verifica los datos del clima de Madrid

---

## Ejercicio 5 — Webhook + API externa

**Objetivo**: Combinar webhook con llamada a API.

**Pasos:**
1. Crea un flujo con un **Webhook** (POST, modo "When last node finishes")
2. Agrega un nodo **HTTP Request** que llame a:
   ```
   https://catfact.ninja/fact
   ```
3. Agrega un nodo **Set** que construya:
   - `hecho_felino` = el texto del hecho de gato
   - `solicitado_por` = el nombre que vino en el body del webhook
   - `cuando` = la fecha actual
4. Agrega un **Respond to Webhook** con el JSON formateado

**Prueba:**
```bash
curl -X POST [TU_TEST_URL] \
  -H "Content-Type: application/json" \
  -d '{"nombre": "Carlos"}'
```

---

## Ejercicio 6 — Webhook con IA (Basico)

**Objetivo**: Usar el flujo `02-webhook-con-ia` y entender como funciona.

**Prerequisito**: Necesitas una API key de OpenAI (platform.openai.com)

**Pasos:**
1. Importa el flujo `flujos/02-webhook-con-ia.json`
2. Crea una credencial de OpenAI:
   - Settings > Credentials > Add Credential > OpenAI API
   - Pega tu API Key
3. Selecciona la credencial en el nodo "Preguntar a ChatGPT"
4. Activa "Listen for test event" en el webhook
5. Prueba con:
   ```bash
   curl -X POST [TU_TEST_URL] \
     -H "Content-Type: application/json" \
     -d '{"pregunta": "Que es n8n en una frase?", "usuario": "Ana"}'
   ```
6. Observa la respuesta del modelo y el campo `tokens_usados`

---

## Ejercicio 7 — Personalizar el System Prompt

**Objetivo**: Cambiar el comportamiento de la IA modificando el system prompt.

**Pasos (sobre el flujo del ejercicio 6):**
1. Edita el nodo **Preguntar a ChatGPT**
2. Cambia el System Message a:
   ```
   Eres un asistente de soporte tecnico para una empresa de software.
   Responde siempre en espanol, de forma tecnica pero clara.
   Si la pregunta no es de soporte tecnico, di amablemente que no puedes ayudar con ese tema.
   ```
3. Prueba con preguntas tecnicas y no tecnicas:
   - `{"pregunta": "Como reseteo mi contrasena?"}`
   - `{"pregunta": "Cual es la capital de Francia?"}`
4. Observa como el modelo responde diferente en cada caso

---

## Ejercicio 8 — Clasificador con IA

**Objetivo**: Usar IA para clasificar el tipo de consulta y enrutar el flujo.

**Pasos:**
1. Crea un nuevo flujo con Webhook
2. Envia la pregunta a OpenAI con este system prompt:
   ```
   Eres un clasificador de consultas. Analiza el mensaje del usuario y responde SOLO con una de estas palabras (sin explicacion): TECNICO, VENTAS, OTRO
   ```
3. Agrega un nodo **Switch** que evalua la respuesta de la IA:
   - Si es `TECNICO` → responde "Te conecto con soporte tecnico"
   - Si es `VENTAS` → responde "Te conecto con el equipo de ventas"
   - Default → responde "No entendi tu consulta, intenta de nuevo"

**Diagrama:**
```
[Webhook] -> [OpenAI: clasificar] -> [Switch]
                                       ├── TECNICO  -> [Respuesta soporte]
                                       ├── VENTAS   -> [Respuesta ventas]
                                       └── default  -> [Respuesta generica]
```

---

## Ejercicio 9 (Avanzado) — Flujo con Bucle

**Objetivo**: Procesar una lista de elementos con el nodo Loop.

**Pasos:**
1. Crea un flujo con **Manual Trigger**
2. Agrega un nodo **Code** que genere una lista de 5 usuarios:
   ```javascript
   return [
     { json: { nombre: 'Ana', email: 'ana@test.com' } },
     { json: { nombre: 'Luis', email: 'luis@test.com' } },
     { json: { nombre: 'Maria', email: 'maria@test.com' } },
     { json: { nombre: 'Pedro', email: 'pedro@test.com' } },
     { json: { nombre: 'Sofia', email: 'sofia@test.com' } }
   ];
   ```
3. Agrega un nodo **Set** que agregue el campo:
   - `saludo` = `Hola {{ $json.nombre }}, tu email es {{ $json.email }}`
4. Observa como se procesan los 5 items a la vez en el panel de salida

---

## Ejercicio 10 (Avanzado) — Flujo de Resumen Diario

**Objetivo**: Crear un flujo programado que llama una API y genera un resumen con IA.

**Pasos:**
1. Trigger: **Schedule** (cada dia a las 9:00 AM, o cada minuto para pruebas)
2. HTTP Request: llama a `https://jsonplaceholder.typicode.com/posts?_limit=5`
3. Code: convierte los 5 posts en un texto unico:
   ```javascript
   const posts = items.map(i => `- ${i.json.title}`).join('\n');
   return [{ json: { resumen_crudo: posts } }];
   ```
4. OpenAI: pide a la IA que haga un resumen ejecutivo del texto
5. Set: formatea la respuesta final con fecha y contenido
6. (Opcional) HTTP Request: envia el resumen a un webhook de Slack o Discord

---

## Recursos de apoyo

- **Documentacion expresiones**: docs.n8n.io/code/expressions/
- **Documentacion nodos**: docs.n8n.io/integrations/
- **Foro de comunidad**: community.n8n.io
- **Cheatsheet del taller**: `material/cheatsheet.md`
