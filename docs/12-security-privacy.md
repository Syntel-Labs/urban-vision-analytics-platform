# 12 – Seguridad y Privacidad

## 1. Introducción

Este documento define las consideraciones de seguridad y privacidad aplicables a la versión v1 del sistema.

El enfoque del proyecto es analítica agregada, no identificación individual.
Las decisiones técnicas deben alinearse con ese principio.

---

## 2. Principios generales

- Minimización de datos.
- No almacenamiento de información personal.
- No procesamiento biométrico.
- Separación clara entre datos en tránsito y datos persistidos.
- Configuración mediante variables de entorno, no valores hardcodeados.

---

## 3. Privacidad

### 3.1 No reconocimiento facial

El sistema:

- No implementa reconocimiento facial.
- No genera embeddings biométricos.
- No intenta identificar personas específicas.

La detección es genérica (clases como “persona” o “vehículo”).

---

### 3.2 No almacenamiento de imágenes

En v1 no se almacenan:

- Frames originales.
- Imágenes procesadas.
- Capturas.
- Video histórico.

Solo se almacenan métricas agregadas.

---

### 3.3 Datos agregados

La persistencia se limita a:

- Conteos.
- Métricas estadísticas.
- Resúmenes por sesión.

Esto reduce significativamente riesgos de privacidad.

---

## 4. Seguridad de la aplicación

### 4.1 Variables sensibles

Las siguientes configuraciones deben manejarse mediante variables de entorno:

- Credenciales de base de datos.
- Configuración de Redis.
- Parámetros de infraestructura.
- Claves secretas si se agregan en el futuro.

No deben almacenarse en el repositorio.

---

### 4.2 Exposición de servicios

Servicios que pueden exponerse públicamente:

- Frontend.
- Backend API.

Servicios que no deben exponerse:

- Redis.
- Base de datos.

Se recomienda red interna aislada para servicios internos.

---

### 4.3 Validación de datos

El Backend debe:

- Validar estructura de eventos recibidos.
- Validar requests entrantes.
- Manejar errores sin revelar detalles internos.

---

## 5. Seguridad en mensajería

- Los eventos no contienen información personal.
- No se transmiten imágenes.
- El backend debe validar el esquema de cada evento.
- Debe tolerar mensajes malformados sin colapsar.

---

## 6. Seguridad en despliegue

Recomendaciones para entorno cloud:

- Restringir puertos abiertos.
- Usar firewall básico.
- Exponer solo servicios necesarios.
- No permitir acceso directo público a la base de datos.

En v1 no se implementa gestión avanzada de identidades.

---

## 7. Autenticación y autorización

En v1:

- No se implementa sistema complejo de autenticación.
- El sistema se asume en entorno controlado o demo.

En futuras versiones podría considerarse:

- Autenticación basada en tokens.
- Control de acceso por sesión.
- Multi-tenant seguro.

---

## 8. Riesgos residuales

- Uso accidental de cámaras no autorizadas.
- Exposición de API sin protección en entorno público.
- Configuración incorrecta de red.

Mitigación:

- Uso de cámaras públicas.
- Configuración mínima de firewall.
- Documentación clara de despliegue.

---

## 9. Cumplimiento conceptual

Dado que:

- No se identifican personas.
- No se almacenan imágenes.
- No se procesan datos biométricos.

El sistema se posiciona como herramienta de analítica estadística agregada, no como sistema de vigilancia individual.
