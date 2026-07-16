# 09 – Contratos de Mensajería

## 1. Introducción

Este documento define los contratos de eventos intercambiados entre el Vision Engine y el Backend mediante el sistema de mensajería.

El objetivo es:

- Establecer estructura clara de los mensajes.
- Evitar acoplamiento implícito.
- Permitir evolución controlada del esquema.
- Facilitar pruebas e integración.

En v1 se utilizará un único flujo principal de eventos de detección.

---

## 2. Principios de diseño

- Los eventos deben ser explícitos y versionados.
- El Vision Engine no debe conocer la lógica del backend.
- El Backend debe validar la estructura recibida.
- Los eventos deben ser serializables en JSON.
- Los cambios de contrato deben documentarse mediante ADR.

---

## 3. Stream principal

Nombre conceptual del stream:

```id="ev1t9k"
vision.detections.v1
```

Propósito:

Transportar eventos de detección y tracking por frame o por lote pequeño de frames.

---

## 4. Evento de detección (DetectionEvent)

### 4.1 Estructura conceptual

```json
{
  "event_version": "v1",
  "event_type": "frame_detections",
  "session_id": "uuid",
  "timestamp": "iso8601",
  "frame_index": 1234,
  "execution_mode": "cpu|gpu",
  "latency_ms": 45,
  "objects": [
    {
      "tracking_id": 7,
      "class": "person",
      "confidence": 0.91,
      "bbox": {
        "x": 100,
        "y": 120,
        "width": 60,
        "height": 140
      }
    }
  ],
  "counts": {
    "people": 5,
    "vehicles": 3
  }
}
```

---

### 4.2 Campos obligatorios

- event_version
- event_type
- session_id
- timestamp
- objects
- counts

---

### 4.3 Campos opcionales

- latency_ms
- frame_index
- métricas adicionales futuras

---

## 5. Semántica del evento

- Un evento representa el estado de detecciones en un momento determinado.
- No representa deltas acumulativos.
- El Backend es responsable de calcular métricas agregadas.
- Los tracking_id son válidos solo dentro de una sesión.

---

## 6. Versionado

El campo `event_version` permite evolución del contrato.

Reglas:

- Cambios incompatibles → nueva versión (v2).
- Cambios compatibles → mantener versión.
- El Backend debe validar versión antes de procesar.

Nunca modificar la estructura v1 una vez estabilizada.

---

## 7. Garantías esperadas

En v1 no se garantiza:

- Exactly-once delivery.
- Orden perfecto bajo fallos extremos.

Se asume:

- Entorno controlado.
- Carga moderada.
- Una fuente principal de eventos.

El Backend debe tolerar:

- Eventos duplicados.
- Eventos fuera de orden ocasionalmente.

---

## 8. Evento de estado (opcional)

Puede definirse un segundo tipo de evento para estado del Vision Engine:

```json
{
  "event_version": "v1",
  "event_type": "engine_status",
  "session_id": "uuid",
  "status": "running|reconnecting|error",
  "message": "string",
  "timestamp": "iso8601"
}
```

Este evento permite que el frontend refleje el estado operativo.

---

## 9. Validación

El Backend debe:

- Validar estructura.
- Validar tipos.
- Manejar eventos inválidos sin bloquear el sistema.
- Registrar errores de contrato.

Se recomienda usar validación tipada en capa de aplicación.

---

## 10. Evolución futura

En versiones posteriores podría incluirse:

- Zonas virtuales.
- Métricas por región.
- Identificador de cámara.
- Información de calidad de detección.
- Batch de múltiples frames por mensaje.

Cualquier extensión deberá:

- Mantener compatibilidad.
- O introducir nueva versión del evento.

---

## 11. Exclusiones

No se enviará:

- Imagen completa.
- Frame codificado.
- Embeddings.
- Datos personales.

La mensajería solo transporta metadatos estructurados.
