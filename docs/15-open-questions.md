# 15 – Preguntas Abiertas

## 1. Introducción

Este documento recopila decisiones pendientes, dudas técnicas y aspectos que requieren investigación adicional.

El objetivo es evitar decisiones implícitas no documentadas y mantener trazabilidad en la evolución del sistema.

---

## 2. Modelos de detección

- ¿Qué variante específica del modelo se utilizará inicialmente (ligero vs balanceado vs más preciso)?
- ¿Se ajustará la resolución de entrada para equilibrar rendimiento y precisión?
- ¿Se permitirá cambiar dinámicamente el modelo en tiempo de ejecución?

---

## 3. Estrategia de tracking

- ¿Se utilizará un algoritmo simple o uno más robusto frente a oclusiones?
- ¿Qué nivel de consistencia de identidad es suficiente para las métricas agregadas?
- ¿Se manejarán zonas virtuales para conteo de entrada/salida?

---

## 4. Estrategia de procesamiento

- ¿Se procesarán todos los frames o se aplicará frame skipping controlado?
- ¿Se implementará control dinámico de FPS según carga?
- ¿Se limitará el tamaño del buffer en mensajería?

---

## 5. Diseño de métricas

- ¿Cómo se definirá formalmente “densidad”?
- ¿Cómo se calculará congestión relativa?
- ¿Qué ventana temporal se utilizará para métricas agregadas?
- ¿Se incluirán métricas comparativas entre CPU y GPU?

---

## 6. Persistencia

- ¿Qué granularidad tendrán los datos almacenados?
- ¿Se almacenarán métricas por minuto, por sesión o ambas?
- ¿Cuál será la estrategia de limpieza o retención de datos?

---

## 7. Contratos de mensajería

- ¿Qué esquema exacto tendrán los eventos enviados desde el Vision Engine?
- ¿Se versionarán los eventos?
- ¿Se incluirá información de confianza del modelo?

---

## 8. API

- ¿Se implementará autenticación básica en v1?
- ¿Se expondrán endpoints para consulta histórica?
- ¿Cómo se manejarán múltiples sesiones concurrentes?

---

## 9. Frontend

- ¿Se mostrará heatmap en v1 o se dejará para iteración posterior?
- ¿Qué tan detallado será el panel de métricas?
- ¿Se incluirá comparación histórica dentro de la misma vista?

---

## 10. Infraestructura

- ¿Qué tipo de instancia cloud se utilizará para la demo?
- ¿Se habilitará GPU en la primera versión desplegada?
- ¿Se documentará benchmarking comparativo?

---

## 11. Observabilidad

- ¿Se incluirán métricas técnicas expuestas por endpoint?
- ¿Se registrará latencia por frame?
- ¿Se incluirá logging estructurado desde el inicio?

---

## 12. Testing

- ¿Cómo se simularán streams en pruebas de integración?
- ¿Se mockeará el modelo de detección en pruebas unitarias?
- ¿Se incluirán pruebas de carga básicas?

---

## 13. Evolución futura

- ¿Se considerará multi-cámara simultánea en v2?
- ¿Se permitirá configuración dinámica de zonas?
- ¿Se incluirá almacenamiento de snapshots en futuras versiones?
