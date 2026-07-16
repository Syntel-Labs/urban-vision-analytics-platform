# 06 – Flujo de Datos

## 1. Introducción

Este documento describe cómo fluye la información a través del sistema desde la fuente de video hasta la visualización en el frontend y la persistencia en base de datos.

El objetivo es definir claramente:

- Qué datos se generan.
- Cómo se transforman.
- Cómo se transportan.
- Dónde se almacenan.

---

## 2. Flujo principal (tiempo real)

### 2.1 Ingesta de video

Origen posible:

- Stream RTSP.
- Webcam local.
- Archivo de video.

El Vision Engine:

1. Captura frame.
2. Realiza preprocesamiento si es necesario.
3. Envía el frame al modelo de detección.

---

### 2.2 Inferencia y tracking

Para cada frame:

1. Se ejecuta detección.
2. Se filtran clases relevantes (personas, vehículos).
3. Se aplica algoritmo de tracking.
4. Se genera estructura de detecciones enriquecida.

Salida conceptual por frame:

- Timestamp.
- Lista de objetos detectados.
- Bounding boxes.
- Identificador temporal (tracking ID).
- Nivel de confianza.
- Métricas instantáneas básicas.

---

### 2.3 Publicación en mensajería

El Vision Engine publica un evento estructurado que contiene:

- Metadatos del frame.
- Lista de objetos rastreados.
- Conteo instantáneo.
- Información del modo de ejecución (CPU/GPU).

Este evento se envía a Redis Streams.

El Vision Engine no espera respuesta del backend.

---

### 2.4 Consumo en Backend

El Backend:

1. Consume eventos desde el stream.
2. Valida estructura.
3. Actualiza métricas agregadas.
4. Calcula:

   - Conteo por intervalo.
   - Flujo de entrada/salida.
   - Densidad aproximada.
5. Decide qué información persistir.
6. Emite actualización vía WebSocket.

---

### 2.5 Actualización del Frontend

El Frontend recibe datos en tiempo real mediante WebSocket:

- Conteo actual.
- Métricas por minuto.
- Estado del sistema.
- Modo CPU/GPU.
- Bounding boxes.

El dashboard actualiza:

- Overlay de video.
- Indicadores.
- Gráficas dinámicas.

---

## 3. Flujo de persistencia

No todos los eventos se almacenan.

Estrategia:

- Persistir métricas agregadas.
- Persistir datos por sesión.
- No almacenar frames ni imágenes.

Flujo:

Backend
→ Transformación a entidad de dominio
→ Persistencia en base de datos

Granularidad prevista:

- Métricas por intervalo (ej. minuto).
- Resumen por sesión.

---

## 4. Flujo de sesión

1. Se inicia sesión de análisis.
2. Se registra metadata (fuente, timestamp inicio).
3. Se procesan frames.
4. Se generan métricas agregadas.
5. Se finaliza sesión.
6. Se almacena resumen final.

---

## 5. Flujo de errores

### 5.1 Caída de stream

- Vision Engine detecta desconexión.
- Intenta reconectar.
- Reporta estado al backend.
- Frontend muestra estado degradado.

---

### 5.2 Error de procesamiento

- Se registra error en logs.
- Se descarta frame si es necesario.
- El sistema continúa procesando siguientes frames.

---

### 5.3 Error en mensajería

- Reintentos básicos.
- Registro de error.
- No se bloquea procesamiento principal indefinidamente.

---

## 6. Flujo en modo CPU vs GPU

El flujo de datos es idéntico en ambos casos.

La única diferencia:

- Tiempo de procesamiento por frame.
- Latencia general del sistema.

El contrato de eventos no cambia.

---

## 7. Flujo resumido end-to-end

1. Fuente genera frame.
2. Vision Engine detecta y rastrea.
3. Se genera evento.
4. Evento se publica en Redis Streams.
5. Backend consume evento.
6. Se calculan métricas.
7. Se persiste información relevante.
8. Se emite actualización en tiempo real.
9. Frontend renderiza resultados.
