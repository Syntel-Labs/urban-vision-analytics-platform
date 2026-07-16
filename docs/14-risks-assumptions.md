# 14 – Riesgos y Supuestos

## 1. Introducción

Este documento identifica los principales riesgos técnicos y operativos del proyecto, así como los supuestos bajo los cuales se diseña la arquitectura.

El objetivo es hacer explícitas las incertidumbres para reducir sorpresas durante la implementación.

---

## 2. Supuestos

### 2.1 Supuestos técnicos

- El modelo de detección seleccionado tendrá precisión suficiente para personas y vehículos en escenarios urbanos.
- El tracking funcionará de manera estable en condiciones normales de iluminación y movimiento.
- Redis Streams soportará el volumen de mensajes generado por un stream de video único en v1.
- PostgreSQL será suficiente para persistencia de métricas agregadas sin necesidad de extensiones adicionales.
- El sistema podrá ejecutarse en CPU, aunque con menor rendimiento que en GPU.

---

### 2.2 Supuestos operativos

- Las cámaras utilizadas serán públicas y accesibles sin autenticación compleja.
- Los streams serán relativamente estables.
- No habrá requerimientos regulatorios adicionales al no almacenar datos biométricos.
- El sistema será evaluado como proyecto técnico, no como solución certificada para producción empresarial.

---

## 3. Riesgos técnicos

### 3.1 Rendimiento insuficiente en CPU

Existe el riesgo de que la inferencia en CPU no alcance una experiencia fluida.

Mitigación:

- Permitir configuración de resolución.
- Ajustar tasa de frames procesados.
- Documentar diferencias CPU vs GPU.

---

### 3.2 Inestabilidad de streams RTSP

Los streams públicos pueden caer o cambiar de formato.

Mitigación:

- Implementar reconexión automática.
- Permitir cambio dinámico de fuente.
- Soportar subida de video como alternativa.

---

### 3.3 Desfase entre detección y métricas

Si el procesamiento se retrasa, las métricas podrían no reflejar el estado real en tiempo cercano al presente.

Mitigación:

- Controlar tamaño de buffer.
- Monitorear latencia.
- Limitar acumulación de frames en cola.

---

### 3.4 Complejidad excesiva prematura

Existe riesgo de sobreingeniería al intentar anticipar escenarios futuros.

Mitigación:

- Mantener alcance claro de v1.
- No introducir microservicios adicionales.
- No añadir tecnologías innecesarias.

---

### 3.5 Integración entre componentes

Problemas de serialización, contratos de eventos o desacople incorrecto entre Vision Engine y Backend.

Mitigación:

- Definir contratos claros de mensajería.
- Documentar esquemas de eventos.
- Implementar pruebas de integración tempranas.

---

## 4. Riesgos de producto

### 4.1 Demo frágil

Depender de una única cámara pública puede afectar la estabilidad de la demostración.

Mitigación:

- Mantener múltiples fuentes configurables.
- Permitir fallback manual.
- Tener videos locales preparados.

---

### 4.2 Falta de diferenciación

Existe riesgo de que el proyecto parezca una simple demo de detección.

Mitigación:

- Priorizar analítica agregada.
- Incluir métricas de flujo y densidad.
- Mostrar históricos y gráficas significativas.

---

## 5. Riesgos legales y de privacidad

- Uso accidental de cámaras no autorizadas.
- Malinterpretación como sistema de vigilancia biométrica.

Mitigación:

- Usar únicamente cámaras públicas.
- No implementar reconocimiento facial.
- No almacenar video ni imágenes.

---

## 6. Riesgos de infraestructura

- Costos elevados al usar instancias GPU en la nube.
- Limitaciones de red en entorno local.

Mitigación:

- Soporte completo en CPU.
- Arquitectura compatible con instancias estándar.
- Documentación clara de requerimientos mínimos.

---

## 7. Riesgos de mantenimiento

- Aumento de complejidad con futuras extensiones.
- Dependencia de librerías externas activamente mantenidas.

Mitigación:

- Arquitectura modular.
- Documentación clara.
- ADRs para decisiones relevantes.

---

## 8. Riesgo estratégico

El proyecto puede expandirse más allá del alcance inicial si no se controla adecuadamente.

Mitigación:

- Definir criterios claros de finalización de v1.
- Documentar nuevas ideas en backlog, no en alcance activo.
