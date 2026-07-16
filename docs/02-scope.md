# 02 – Alcance

## 1. Objetivo del alcance

Este documento define qué incluye y qué no incluye la versión v1 del sistema, estableciendo límites claros para evitar sobreingeniería y desviaciones.

La versión inicial busca demostrar capacidad técnica, diseño arquitectónico sólido y procesamiento en tiempo real con métricas significativas.

---

## 2. Alcance funcional (Incluido en v1)

### 2.1 Ingesta de video

El sistema soportará:

- Stream RTSP público.
- Webcam local.
- Archivo de video cargado manualmente.

No se contempla gestión masiva de múltiples cámaras simultáneas en v1.

---

### 2.2 Detección y tracking

El sistema será capaz de:

- Detectar personas.
- Detectar vehículos.
- Rastrear objetos entre frames.
- Mantener identidad temporal mediante tracking.

No se realizará identificación individual ni reconocimiento facial.

---

### 2.3 Analítica en tiempo real

Se incluirán métricas como:

- Conteo actual de objetos.
- Conteo por intervalo de tiempo.
- Flujo de entrada y salida.
- Densidad aproximada.
- Latencia estimada del sistema.

Las métricas serán calculadas y expuestas en tiempo real mediante WebSocket.

---

### 2.4 Persistencia

Se almacenarán:

- Conteos agregados.
- Métricas por sesión.
- Históricos básicos.
- Metadatos de ejecución.

No se almacenarán imágenes ni video procesado en v1.

---

### 2.5 Dashboard

El frontend incluirá:

- Visualización de video en tiempo real.
- Bounding boxes superpuestos.
- Métricas en vivo.
- Gráficas históricas.
- Indicador de modo de ejecución (CPU/GPU).
- Estado del sistema.

---

### 2.6 Arquitectura

La versión v1 incluirá:

- Vision Engine independiente.
- Backend API.
- Redis Streams como sistema de mensajería.
- Base de datos PostgreSQL.
- Docker y Docker Compose para entorno local.
- Deploy inicial en instancia EC2.

No se incluirá orquestación con Kubernetes.

---

## 3. Alcance técnico

- Arquitectura modular inspirada en principios de separación de capas.
- Comunicación asincrónica entre componentes.
- Soporte para ejecución en CPU y GPU mediante configuración.
- Manejo básico de errores y reconexión de streams.

No se implementará alta disponibilidad distribuida ni escalado automático en esta versión.

---

## 4. Fuera de alcance (Explícitamente excluido)

- Reconocimiento facial.
- Identificación de individuos.
- Modelos entrenados custom.
- Multi-tenant real.
- Sistema de autenticación complejo.
- Panel administrativo avanzado.
- Gestión de usuarios empresariales.
- Facturación o monetización.
- Procesamiento batch masivo offline.
- Integración con servicios externos de analítica.

---

## 5. Alcance de investigación

Durante el desarrollo podrán evaluarse:

- Diferentes modelos de detección compatibles.
- Alternativas de tracking.
- Optimización de rendimiento CPU vs GPU.

Estas evaluaciones no alterarán la arquitectura base definida.

---

## 6. Criterios de finalización de v1

La versión v1 se considera completa cuando:

1. Se puede conectar a una fuente de video.
2. Se detectan y rastrean objetos correctamente.
3. Se generan métricas agregadas en tiempo real.
4. El frontend visualiza datos y bounding boxes.
5. Existe persistencia básica de métricas.
6. El sistema puede ejecutarse tanto en CPU como en GPU.
7. La documentación arquitectónica está completa.
