# 05 – Arquitectura General

## 1. Introducción

Este documento describe la arquitectura de alto nivel del sistema en su versión v1.

La arquitectura está diseñada para:

- Procesar video en tiempo real.
- Desacoplar inferencia y analítica.
- Permitir ejecución en CPU o GPU.
- Mantener simplicidad operativa.
- Ser extensible sin sobreingeniería.

---

## 2. Vista de alto nivel

El sistema está compuesto por los siguientes bloques principales:

1. Fuente de video (RTSP / Webcam / Archivo)
2. Vision Engine
3. Sistema de mensajería
4. Backend API
5. Base de datos
6. Frontend

Flujo conceptual:

Video
→ Vision Engine
→ Mensajería
→ Backend
→ WebSocket
→ Dashboard

Persistencia: Backend → Base de datos

---

## 3. Componentes principales

### 3.1 Vision Engine

Responsabilidades:

- Capturar frames.
- Ejecutar inferencia.
- Aplicar tracking.
- Emitir eventos estructurados.
- Exponer estado de ejecución.

Características:

- Independiente del backend.
- Configurable para CPU o GPU.
- No conoce detalles del frontend ni de la base de datos.

Es un componente especializado en procesamiento de visión.

---

### 3.2 Sistema de Mensajería

Responsabilidades:

- Desacoplar procesamiento de consumo.
- Transportar eventos de detección.
- Actuar como buffer entre productores y consumidores.

Permite que el Vision Engine no dependa directamente del Backend.

---

### 3.3 Backend API

Responsabilidades:

- Consumir eventos de detección.
- Agregar métricas.
- Gestionar sesiones.
- Persistir datos históricos.
- Exponer endpoints REST.
- Emitir datos en tiempo real vía WebSocket.

El backend es el orquestador del sistema.

---

### 3.4 Base de Datos

Responsabilidades:

- Persistencia de métricas agregadas.
- Almacenamiento de sesiones.
- Históricos básicos.

No almacena video ni imágenes.

---

### 3.5 Frontend

Responsabilidades:

- Mostrar stream procesado.
- Dibujar bounding boxes.
- Visualizar métricas.
- Mostrar gráficas históricas.
- Indicar estado del sistema.

Es la capa de presentación y visualización.

---

## 4. Estilo arquitectónico

La arquitectura sigue principios de:

- Separación por capas.
- Bajo acoplamiento.
- Comunicación asincrónica.
- Responsabilidad única por componente.

No es una arquitectura de microservicios distribuida completa, pero sí separa claramente:

- Procesamiento de visión.
- Orquestación y analítica.
- Visualización.

---

## 5. Desacoplamiento

El desacoplamiento se logra mediante:

- Mensajería intermedia.
- Contratos de eventos bien definidos.
- Separación física de contenedores.
- APIs explícitas.

Esto permite:

- Escalar Vision Engine en el futuro.
- Sustituir el modelo de detección sin afectar el backend.
- Modificar frontend sin tocar lógica de inferencia.

---

## 6. Flujo de datos resumido

1. Se recibe frame desde fuente.
2. Se ejecuta detección.
3. Se aplica tracking.
4. Se genera evento estructurado.
5. Evento se publica en mensajería.
6. Backend consume evento.
7. Se actualizan métricas agregadas.
8. Se persisten datos relevantes.
9. Se envían actualizaciones vía WebSocket.
10. Frontend actualiza visualización.

---

## 7. Modo CPU / GPU

El sistema mantiene la misma arquitectura en ambos modos.

La única diferencia es:

- El dispositivo de ejecución del modelo.

No se modifican:

- Contratos.
- Mensajería.
- Backend.
- Frontend.

Esto garantiza consistencia estructural.

---

## 8. Límites del sistema

Incluye:

- Procesamiento de una fuente activa en v1.
- Métricas agregadas básicas.
- Dashboard en tiempo real.

No incluye:

- Escalado automático.
- Alta disponibilidad distribuida.
- Balanceadores complejos.
- Multi-región.

---

## 9. Evolución prevista

La arquitectura permite, en futuras versiones:

- Multi-cámara.
- Escalado horizontal del Vision Engine.
- Métricas más avanzadas.
- Integración con sistemas externos.
- Orquestación más sofisticada.

Estas extensiones no requieren rediseño estructural, solo expansión controlada.
