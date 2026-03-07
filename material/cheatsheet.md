# Cheatsheet de n8n

## Expresiones esenciales

### Acceder a datos del item actual
```
{{ $json.campo }}                        # campo del item actual
{{ $json.objeto.subcampo }}              # campo anidado
{{ $json.lista[0] }}                     # primer elemento de array
{{ $json.lista[0].nombre }}              # campo dentro de array
```

### Referenciar otros nodos
```
{{ $('Nombre del Nodo').item.json.campo }}    # item actual del nodo
{{ $('Nombre del Nodo').all()[0].json.campo }} # primer item del nodo
{{ $('Nombre del Nodo').all().length }}        # cuantos items tiene
```

### Fecha y hora
```
{{ $now }}                               # fecha/hora actual (objeto)
{{ $now.toISO() }}                       # "2025-01-15T10:30:00.000Z"
{{ $now.format('YYYY-MM-DD') }}          # "2025-01-15"
{{ $now.format('DD/MM/YYYY HH:mm') }}    # "15/01/2025 10:30"
{{ $now.minus({days: 7}).toISO() }}      # hace 7 dias
```

### Strings
```
{{ $json.texto.toUpperCase() }}          # MAYUSCULAS
{{ $json.texto.toLowerCase() }}          # minusculas
{{ $json.texto.trim() }}                 # quitar espacios
{{ $json.texto.includes('hola') }}       # true/false
{{ $json.texto.replace('a', 'b') }}      # reemplazar
{{ $json.texto.split(',') }}             # dividir en array
{{ $json.nombre + ' ' + $json.apellido }} # concatenar
```

### Numeros
```
{{ $json.precio * 1.21 }}                # calcular IVA
{{ Math.round($json.valor) }}            # redondear
{{ Math.max($json.a, $json.b) }}         # maximo
{{ parseInt($json.texto) }}              # texto a entero
{{ parseFloat($json.texto) }}            # texto a decimal
{{ $json.numero.toFixed(2) }}            # 2 decimales
```

### Arrays / Listas
```
{{ $json.lista.length }}                 # longitud
{{ $json.lista.join(', ') }}             # unir con coma
{{ $json.lista.includes('valor') }}      # contiene?
{{ $json.lista.filter(x => x > 5) }}    # filtrar
{{ $json.lista.map(x => x * 2) }}       # transformar
```

### Condicionales (ternario)
```
{{ $json.activo ? 'SI' : 'NO' }}
{{ $json.precio > 100 ? 'caro' : 'barato' }}
{{ $json.nombre || 'Sin nombre' }}       # valor por defecto
```

---

## Variables especiales

| Variable | Descripcion |
|---|---|
| `$json` | Datos del item actual |
| `$binary` | Datos binarios del item actual |
| `$now` | Fecha/hora actual (luxon) |
| `$today` | Fecha de hoy (sin hora) |
| `$workflow.id` | ID del flujo |
| `$workflow.name` | Nombre del flujo |
| `$execution.id` | ID de la ejecucion |
| `$execution.mode` | "manual" o "trigger" |
| `$env.MI_VARIABLE` | Variable de entorno |

---

## Nodos mas usados

### Webhook
- Trigger que recibe peticiones HTTP
- Metodo: GET, POST, PUT, DELETE
- Modos de respuesta: inmediata o al final del flujo

### HTTP Request
- Llama cualquier API externa
- Configurar: Method, URL, Headers, Body
- Autenticacion: Bearer Token, Basic Auth, etc.

### Set
- Crear o modificar campos del item
- Modo "Keep Only Set": elimina campos no especificados

### IF
- Divide el flujo en rama true y false
- Condiciones: igual, mayor, contiene, regex, etc.

### Switch
- Como IF pero con multiples ramas
- Util para enrutar por tipo, estado, etc.

### Code (JavaScript)
- Logica personalizada en JS
- `return items` para pasar items al siguiente nodo
- `return [{ json: { resultado: 'algo' } }]` para crear items nuevos

### Merge
- Une dos ramas del flujo
- Modos: Append, Merge By Index, Merge By Key

### Loop Over Items
- Itera sobre items de uno en uno
- Util cuando necesitas procesar secuencialmente

### Wait
- Pausa la ejecucion X segundos/minutos
- Util para respetar rate limits de APIs

---

## Codigo JavaScript en el nodo Code

```javascript
// Acceder al item actual
const item = items[0].json;

// Modificar y devolver items
return items.map(item => {
  return {
    json: {
      ...item.json,             // mantener campos existentes
      nuevo_campo: 'valor',
      precio_con_iva: item.json.precio * 1.21
    }
  };
});

// Filtrar items
return items.filter(item => item.json.activo === true);

// Crear items nuevos
return [
  { json: { id: 1, nombre: 'Primero' } },
  { json: { id: 2, nombre: 'Segundo' } }
];
```

---

## Atajos de teclado en el canvas

| Atajo | Accion |
|---|---|
| `Ctrl/Cmd + S` | Guardar flujo |
| `Ctrl/Cmd + Z` | Deshacer |
| `Ctrl/Cmd + A` | Seleccionar todo |
| `Ctrl/Cmd + C / V` | Copiar / Pegar nodos |
| `Delete` | Eliminar nodo seleccionado |
| `Ctrl/Cmd + Enter` | Ejecutar flujo |
| `Scroll` | Zoom in/out |
| `Espacio + Drag` | Mover el canvas |

---

## Probar webhooks con curl

```bash
# Peticion GET simple
curl https://TU-N8N/webhook-test/mi-flujo

# Peticion POST con JSON
curl -X POST https://TU-N8N/webhook-test/mi-flujo \
  -H "Content-Type: application/json" \
  -d '{"nombre": "Ana", "email": "ana@ejemplo.com"}'

# Peticion POST con autenticacion
curl -X POST https://TU-N8N/webhook/mi-flujo \
  -H "Content-Type: application/json" \
  -H "Authorization: Bearer mi-token-secreto" \
  -d '{"dato": "valor"}'
```

---

## APIs publicas gratuitas para practicar

| API | URL | Descripcion |
|---|---|---|
| Open-Meteo | `https://api.open-meteo.com/v1/forecast?latitude=40.42&longitude=-3.70&current_weather=true` | Clima en tiempo real |
| JSONPlaceholder | `https://jsonplaceholder.typicode.com/posts` | Posts de prueba |
| Cat Facts | `https://catfact.ninja/fact` | Datos aleatorios |
| Random User | `https://randomuser.me/api/` | Usuarios aleatorios |
| IP Info | `https://ipinfo.io/json` | Info de tu IP |
| Chuck Norris | `https://api.chucknorris.io/jokes/random` | Chistes aleatorios |

---

## Errores comunes y soluciones

| Error | Causa | Solucion |
|---|---|---|
| `Cannot read property of undefined` | Campo no existe en el JSON | Usar `$json.campo || 'default'` |
| `Workflow did not return data` | Ultimo nodo no devuelve nada | Revisar que Respond to Webhook este al final |
| `401 Unauthorized` | Credenciales incorrectas | Verificar API key o token |
| `429 Too Many Requests` | Rate limit de la API | Agregar nodo Wait entre llamadas |
| `Test URL only works in test mode` | Usar URL de produccion en test | Usar Test URL con "Listen for test event" |
