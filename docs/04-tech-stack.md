# 04 – Tech Stack

## 1. Principios de selección

Las tecnologías seleccionadas cumplen con los siguientes criterios:

- Madurez y estabilidad.
- Comunidad activa.
- Compatibilidad con visión computacional.
- Buen soporte para tiempo real.
- Simplicidad operativa en v1.
- Coherencia tecnológica (backend y visión en Python).

El objetivo es evitar sobreingeniería y priorizar claridad arquitectónica.

---

## 2. Vision Engine

Responsable de:

- Ingesta de frames.
- Inferencia de detección.
- Tracking.
- Publicación de eventos.

Stack base:

- Python 3.11
- Framework HTTP ligero para control y health checks
- Librería de detección basada en YOLO
- Librerías de procesamiento de imagen
- Motor de deep learning compatible con CPU y GPU
- Algoritmo de tracking multi-objeto

El motor debe permitir configuración mediante variable de entorno:

```bash
DEVICE=cpu | cuda
```

---

## 3. Backend API

Responsable de:

- Orquestación.
- Gestión de sesiones.
- Agregación de métricas.
- Exposición REST.
- Comunicación WebSocket con frontend.
- Consumo de eventos desde mensajería.

Stack base:

- Python 3.11
- Framework ASGI moderno
- Validación de datos tipada
- ORM para persistencia
- WebSockets nativos

Se prioriza coherencia con el Vision Engine para facilitar mantenimiento.

---

## 4. Messaging / Streaming

Responsable de:

- Desacoplar procesamiento e ingesta.
- Transportar eventos de detección.
- Permitir comunicación asincrónica.

Tecnología seleccionada:

- Redis Streams

Razones:

- Simplicidad.
- Bajo overhead operativo.
- Integración sencilla con Python.
- Adecuado para v1 sin necesidad de sistemas más complejos.

---

## 5. Base de Datos

Responsable de:

- Persistencia de métricas agregadas.
- Históricos.
- Metadatos de sesiones.

Tecnología seleccionada:

- PostgreSQL

Razones:

- Estabilidad.
- Soporte SQL completo.
- Adecuado para métricas agregadas sin necesidad de extensiones especializadas en v1.

---

## 6. Frontend

Responsable de:

- Visualización de video.
- Bounding boxes.
- Métricas en tiempo real.
- Gráficas históricas.
- Indicadores de estado.

Stack base:

- Framework moderno basado en componentes.
- TypeScript.
- Bundler ligero.
- Librería de gráficos.
- Framework de estilos utilitario.

Se prioriza claridad visual y simplicidad de despliegue.

---

## 7. Infraestructura

- Docker
- Docker Compose
- Instancia cloud simple para despliegue inicial

No se incluye:

- Kubernetes.
- Arquitectura distribuida avanzada.
- Sistemas de mensajería adicionales.

---

## 8. Decisiones explícitas de exclusión

En v1 no se utilizarán:

- Microservicios adicionales.
- Orquestadores complejos.
- Sistemas de streaming distribuidos avanzados.
- Bases de datos especializadas en series de tiempo.
- Reconocimiento facial.
