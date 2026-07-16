# 13 – Testing

## 1. Introducción

Este documento define la estrategia de pruebas para la versión v1 del sistema.

El objetivo es:

- Validar comportamiento funcional.
- Detectar errores de integración.
- Asegurar estabilidad básica.
- Facilitar refactorización segura.

No se busca cobertura total, sino pruebas representativas y estratégicas.

---

## 2. Principios

- Probar lógica de dominio de forma aislada.
- Evitar dependencias reales en pruebas unitarias.
- Validar contratos entre componentes.
- Mantener pruebas rápidas y reproducibles.
- Separar claramente unitarias e integración.

---

## 3. Testing en el Vision Engine

### 3.1 Pruebas unitarias

Se deben cubrir:

- Transformación de detecciones en objetos de dominio.
- Cálculo de conteos.
- Filtrado de clases relevantes.
- Generación de eventos estructurados.
- Manejo de configuración (cpu/gpu).

El modelo de detección puede ser mockeado.

---

### 3.2 Pruebas de integración

- Simulación de procesamiento de video corto.
- Validación de estructura del evento generado.
- Validación de publicación en mensajería.

No es necesario ejecutar modelo pesado real en todas las pruebas.

---

## 4. Testing en el Backend

### 4.1 Pruebas unitarias

Se deben cubrir:

- Validación de contratos de mensajería.
- Cálculo de métricas agregadas.
- Lógica de sesión.
- Transformación a entidades persistentes.

Dependencias externas deben ser mockeadas.

---

### 4.2 Pruebas de integración

- Consumo real desde Redis en entorno controlado.
- Persistencia en base de datos de prueba.
- Validación de endpoints REST.
- Validación de WebSocket básico.

---

## 5. Testing del Frontend

Se pueden incluir:

- Pruebas de componentes aislados.
- Validación de renderizado condicional.
- Simulación de eventos WebSocket.
- Validación de visualización de métricas.

No es necesario probar visualización pixel-perfect en v1.

---

## 6. Testing de contratos

Debe validarse:

- Que el evento generado por Vision Engine coincide con el contrato documentado.
- Que el Backend maneja correctamente la versión del evento.
- Que la API responde según estructura definida.

Se pueden utilizar fixtures JSON representativos.

---

## 7. Testing end-to-end básico

Escenario mínimo:

1. Iniciar sesión.
2. Procesar video corto.
3. Generar eventos.
4. Backend consume eventos.
5. Métricas se calculan.
6. Frontend recibe actualización.
7. Sesión finaliza correctamente.

Este test puede ser manual o semiautomatizado en v1.

---

## 8. Entorno de pruebas

- Base de datos separada para testing.
- Configuración independiente mediante variables de entorno.
- No reutilizar base de datos de desarrollo.

---

## 9. Exclusiones en v1

No se incluye:

- Pruebas de carga intensivas.
- Benchmarking automatizado.
- Pruebas distribuidas complejas.
- Cobertura total obligatoria.

---

## 10. Criterios de aceptación

El sistema se considera estable cuando:

- Las pruebas unitarias pasan consistentemente.
- Las pruebas de integración validan contratos.
- El flujo principal funciona sin errores críticos.
- El sistema se puede reiniciar sin corrupción de datos.
