# 03 – Requisitos No Funcionales (NFR)

## 1. Introducción

Este documento define los requisitos no funcionales del sistema.
Estos requisitos establecen criterios de calidad, rendimiento, confiabilidad y mantenibilidad que deben cumplirse en la versión v1.

Los NFRs son tan importantes como los requisitos funcionales, ya que determinan la viabilidad técnica del sistema en escenarios reales.

---

## 2. Rendimiento

### 2.1 Latencia

- El sistema debe procesar frames con una latencia razonable para considerarse “casi en tiempo real”.
- El objetivo en CPU es mantener procesamiento estable.
- En GPU, se espera reducción significativa de latencia.

No se establece un SLA estricto en v1, pero el rendimiento debe ser medible.

---

### 2.2 Throughput

- El sistema debe poder procesar al menos un stream de video continuo.
- Debe evitar acumulación descontrolada de frames en la cola.
- Redis Streams no debe convertirse en cuello de botella bajo carga básica.

---

### 2.3 Uso de recursos

- Debe poder ejecutarse en una instancia estándar de nube.
- Debe soportar ejecución en CPU sin requerir GPU obligatoriamente.
- El consumo de memoria debe mantenerse controlado durante ejecución prolongada.

---

## 3. Escalabilidad

- La arquitectura debe permitir escalar horizontalmente el Vision Engine en el futuro.
- La mensajería debe desacoplar procesamiento e ingestión.
- El backend debe poder manejar múltiples conexiones WebSocket.

En v1 no se implementará autoescalado, pero la arquitectura debe permitirlo.

---

## 4. Disponibilidad y resiliencia

- El sistema debe manejar caída de stream de video.
- Debe poder reconectar a fuentes RTSP.
- No debe fallar completamente ante errores de un componente aislado.
- Debe registrar errores críticos para diagnóstico.

No se requiere alta disponibilidad multi-zona en v1.

---

## 5. Mantenibilidad

- Código organizado por capas.
- Separación clara entre dominio, aplicación e infraestructura.
- Bajo acoplamiento entre módulos.
- Decisiones arquitectónicas documentadas mediante ADR.

El proyecto debe ser comprensible para otro ingeniero.

---

## 6. Observabilidad

- Logs estructurados.
- Métricas básicas de latencia.
- Indicador de estado del sistema.
- Visibilidad del modo de ejecución (CPU/GPU).

No se integrarán sistemas externos avanzados de monitoreo en v1.

---

## 7. Seguridad

- No almacenar datos personales identificables.
- No realizar reconocimiento facial.
- Evitar exposición pública de endpoints sin control.
- Variables sensibles gestionadas mediante variables de entorno.

La seguridad en v1 será básica pero correcta.

---

## 8. Portabilidad

- El sistema debe ejecutarse en entorno local mediante Docker Compose.
- Debe poder desplegarse en una instancia cloud sin cambios estructurales.
- No depender de configuraciones manuales complejas.

---

## 9. Testabilidad

- Capacidad de ejecutar pruebas unitarias.
- Capacidad de ejecutar pruebas de integración.
- Componentes desacoplados para facilitar mocking.

No se exige cobertura total en v1, pero sí pruebas representativas.

---

## 10. Compatibilidad

- Python 3.11 como base del backend y Vision Engine.
- Navegadores modernos para el frontend.
- Base de datos PostgreSQL estándar.

---

## 11. Restricciones técnicas

- No se usará Kubernetes en v1.
- No se usarán sistemas de mensajería complejos adicionales.
- No se implementará arquitectura de microservicios distribuida completa.
