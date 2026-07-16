# 11 – Observabilidad

## 1. Introducción

Este documento define cómo el sistema expone información para monitoreo, diagnóstico y análisis operativo.

En v1, la observabilidad será básica pero suficiente para:

- Detectar fallos.
- Medir rendimiento.
- Comparar ejecución CPU vs GPU.
- Diagnosticar problemas de integración.

No se implementarán soluciones avanzadas de monitoreo distribuido en esta versión.

---

## 2. Objetivos de observabilidad

El sistema debe permitir responder preguntas como:

- ¿Está el Vision Engine activo?
- ¿Está el Backend consumiendo eventos?
- ¿Cuál es la latencia promedio por frame?
- ¿Se están acumulando mensajes en el stream?
- ¿Está el sistema en modo CPU o GPU?
- ¿Existen errores recurrentes?

---

## 3. Logging

### 3.1 Vision Engine

Debe registrar:

- Inicio y fin de sesión.
- Modo de ejecución (cpu/gpu).
- Reconexión de stream.
- Errores de inferencia.
- Latencia por frame (opcional agregada).

Formato recomendado:

- Logs estructurados.
- Nivel de severidad (INFO, WARNING, ERROR).
- Timestamp en formato estándar.

---

### 3.2 Backend

Debe registrar:

- Inicio y cierre de sesión.
- Consumo de eventos.
- Errores de validación.
- Fallos en persistencia.
- Conexiones WebSocket activas.

---

### 3.3 Base de datos y Redis

En v1:

- Logs accesibles desde contenedores.
- Sin integración externa.
- Diagnóstico manual si ocurre fallo.

---

## 4. Métricas técnicas

### 4.1 Métricas del Vision Engine

- Frames procesados por segundo.
- Latencia promedio por frame.
- Tiempo de inferencia.
- Uso estimado de dispositivo (si es posible).

---

### 4.2 Métricas del Backend

- Eventos procesados por segundo.
- Tiempo de procesamiento por evento.
- Número de sesiones activas.
- Conexiones WebSocket activas.

---

## 5. Indicadores en el frontend

El dashboard debe mostrar:

- Estado del sistema (running, reconnecting, error).
- Modo de ejecución (CPU/GPU).
- Latencia estimada.
- Conteo actual.

Esto permite observabilidad funcional desde la interfaz.

---

## 6. Health Checks

El Backend debe exponer endpoint de health check.

El Vision Engine puede exponer endpoint simple para:

- Verificar que está activo.
- Confirmar modo de ejecución.

Estos endpoints permiten monitoreo básico externo.

---

## 7. Manejo de errores

- Errores no deben detener el sistema completo.
- Deben registrarse con suficiente contexto.
- Debe evitarse saturación de logs en errores repetitivos.
- El sistema debe continuar procesando cuando sea posible.

---

## 8. Limitaciones en v1

No se incluye:

- Sistema centralizado de logs.
- Métricas exportadas a plataforma externa.
- Alertas automáticas.
- Tracing distribuido.

Estas mejoras pueden incorporarse en versiones futuras.

---

## 9. Evolución futura

La arquitectura permite incorporar:

- Exportadores de métricas.
- Dashboards externos.
- Alertas por umbral.
- Instrumentación más detallada.

Estas mejoras no requieren rediseño estructural.
