# 07 – Modelo de Datos

## 1. Introducción

Este documento define el modelo de datos conceptual y persistente del sistema.

El objetivo es:

- Establecer las entidades principales.
- Definir relaciones.
- Delimitar qué información se almacena.
- Separar datos efímeros (tiempo real) de datos persistentes.

En v1, el almacenamiento se centra en métricas agregadas y sesiones, no en frames individuales.

---

## 2. Principios del modelo

- No almacenar imágenes ni video.
- No almacenar datos biométricos.
- Persistir solo información agregada.
- Diseñar modelo extensible para futuras métricas.
- Mantener esquema simple y normalizado.

---

## 3. Entidades principales

### 3.1 Session

Representa una ejecución de análisis sobre una fuente de video.

Campos conceptuales:

- id
- source_type (rtsp, webcam, file)
- source_identifier
- started_at
- ended_at
- execution_mode (cpu, gpu)
- status
- total_frames_processed
- average_latency_ms

Relación:

- Una sesión tiene múltiples métricas agregadas.

---

### 3.2 FrameMetric (opcional en v1)

Representa métricas calculadas por intervalo de tiempo.

En lugar de almacenar por frame, se recomienda almacenar por ventana temporal.

Campos conceptuales:

- id
- session_id
- timestamp_interval_start
- timestamp_interval_end
- people_count_avg
- vehicle_count_avg
- density_estimate
- congestion_score

Relación:

- Muchas métricas pertenecen a una sesión.

---

### 3.3 AggregateSummary

Resumen final de la sesión.

Campos conceptuales:

- id
- session_id
- total_people_detected
- total_vehicles_detected
- peak_people_count
- peak_vehicle_count
- max_density
- average_congestion_score

Se puede generar al finalizar la sesión.

---

## 4. Datos efímeros (no persistentes)

Estos datos solo existen en memoria o en mensajería:

- Bounding boxes por frame.
- Tracking IDs temporales.
- Confianza por detección.
- Métricas instantáneas por frame.
- Estado en tiempo real.

Estos datos se transmiten vía WebSocket pero no se almacenan.

---

## 5. Relaciones principales

Session (1)
→ (N) FrameMetric

Session (1)
→ (1) AggregateSummary

No existen relaciones complejas en v1.

---

## 6. Modelo conceptual del dominio

Separación por capas:

Dominio:

- Session (entidad)
- Metrics (value objects)
- ExecutionMode (value object)
- SourceType (value object)

Infraestructura:

- Modelos ORM.
- Esquemas de persistencia.
- Migraciones.

El dominio no debe depender del ORM.

---

## 7. Versionado y extensibilidad

El modelo está diseñado para permitir:

- Agregar nuevas métricas sin romper sesiones anteriores.
- Añadir campos opcionales.
- Expandir tipos de fuente.
- Incorporar zonas virtuales en versiones futuras.

---

## 8. Consideraciones de rendimiento

- Índices en session_id.
- Índices en timestamp_interval_start.
- Evitar inserciones excesivas por frame.
- Agrupar métricas por ventana temporal.

En v1 se prioriza claridad sobre optimización extrema.

---

## 9. Exclusiones explícitas

No se almacenará:

- Imagen original.
- Imagen procesada.
- Embeddings.
- Identificadores biométricos.
- Datos personales.
