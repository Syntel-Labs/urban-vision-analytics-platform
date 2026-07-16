# 08 – Contratos de API

## 1. Introducción

Este documento define los contratos expuestos por el Backend hacia clientes externos, principalmente el Frontend.

Incluye:

- Endpoints REST.
- WebSocket para tiempo real.
- Estructura de request y response.
- Convenciones generales.

En v1 no se implementará un sistema complejo de autenticación.

---

## 2. Principios de diseño

- API explícita y versionada.
- Respuestas estructuradas y tipadas.
- Separación clara entre datos históricos y datos en tiempo real.
- No exponer detalles internos del Vision Engine.
- Manejo consistente de errores.

Prefijo base:

```id="9l2pxa"
/api/v1
```

---

## 3. Endpoints REST

### 3.1 Health Check

GET `/health`

Propósito:

Verificar que el backend está operativo.

Response conceptual:

```json id="g5t2dn"
{
  "status": "ok",
  "timestamp": "iso8601"
}
```

---

### 3.2 Crear sesión

POST `/sessions`

Request:

```json id="r2v8cm"
{
  "source_type": "rtsp|webcam|file",
  "source_identifier": "string",
  "execution_mode": "cpu|gpu"
}
```

Response:

```json id="k1z7hx"
{
  "session_id": "uuid",
  "status": "created",
  "started_at": "iso8601"
}
```

---

### 3.3 Finalizar sesión

POST `/sessions/{session_id}/stop`

Response:

```json id="m3q4bt"
{
  "session_id": "uuid",
  "status": "stopped",
  "ended_at": "iso8601"
}
```

---

### 3.4 Obtener resumen de sesión

GET `/sessions/{session_id}`

Response conceptual:

```json id="y6w1ne"
{
  "session_id": "uuid",
  "source_type": "rtsp",
  "execution_mode": "cpu",
  "started_at": "iso8601",
  "ended_at": "iso8601",
  "summary": {
    "total_people_detected": 1200,
    "total_vehicles_detected": 430,
    "peak_people_count": 45,
    "peak_vehicle_count": 20,
    "max_density": 0.78
  }
}
```

---

### 3.5 Métricas históricas

GET `/sessions/{session_id}/metrics`

Parámetros opcionales:

- from
- to
- interval

Response conceptual:

```json id="z9u4fd"
{
  "session_id": "uuid",
  "metrics": [
    {
      "interval_start": "iso8601",
      "interval_end": "iso8601",
      "people_count_avg": 12,
      "vehicle_count_avg": 4,
      "density_estimate": 0.34,
      "congestion_score": 0.21
    }
  ]
}
```

---

## 4. WebSocket – Tiempo Real

Ruta conceptual:

```id="j8p1rs"
/ws/sessions/{session_id}
```

Propósito:

Transmitir actualizaciones en tiempo real hacia el frontend.

---

### 4.1 Evento de actualización en vivo

Ejemplo de mensaje enviado por el backend:

```json id="t7b2qx"
{
  "event_type": "live_metrics",
  "session_id": "uuid",
  "timestamp": "iso8601",
  "current_counts": {
    "people": 8,
    "vehicles": 3
  },
  "density_estimate": 0.45,
  "execution_mode": "cpu",
  "latency_ms": 52,
  "system_status": "running"
}
```

---

### 4.2 Evento de estado

```json id="w2c9lm"
{
  "event_type": "system_status",
  "session_id": "uuid",
  "status": "running|reconnecting|error",
  "message": "string"
}
```

---

## 5. Convenciones generales

### 5.1 Formato

- JSON como formato estándar.
- Fechas en formato ISO 8601.
- UUID para identificadores.
- Campos en snake_case.

---

### 5.2 Manejo de errores

Formato estándar de error:

```json id="u4n8ka"
{
  "error": {
    "code": "string",
    "message": "string",
    "details": "string | null"
  }
}
```

Códigos HTTP típicos:

- 200 OK
- 201 Created
- 400 Bad Request
- 404 Not Found
- 500 Internal Server Error

---

## 6. Versionado

La API se versiona mediante prefijo:

```id="v5x3op"
/api/v1
```

Cambios incompatibles requerirán `/api/v2`.

El WebSocket seguirá la misma política de versionado lógico.

---

## 7. Exclusiones

En v1 no se incluye:

- Autenticación avanzada.
- Control granular de permisos.
- Multi-tenant real.
- Rate limiting complejo.

---

## 8. Relación con mensajería

- El Backend traduce eventos internos del stream a contratos públicos.
- El contrato de mensajería y el contrato de API son independientes.
- Cambios en mensajería no deben afectar directamente la API externa.
