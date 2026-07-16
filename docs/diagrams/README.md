# Diagramas Arquitectónicos

## 1. Propósito

Este directorio contiene los diagramas que describen la arquitectura y el flujo dinámico del sistema.

Los diagramas complementan la documentación textual y permiten visualizar:

- Estructura de alto nivel.
- Componentes internos.
- Flujo de datos.
- Secuencia de procesamiento.

Todos los diagramas están definidos en formato PlantUML (`.puml`).

---

## 2. Diagramas incluidos

### 2.1 c4-context.puml

Describe el sistema como una única caja dentro de su entorno.

Muestra:

- Actores principales.
- Sistemas externos.
- Límites del sistema.
- Interacciones de alto nivel.

---

### 2.2 c4-container.puml

Describe los contenedores internos del sistema.

Incluye:

- Frontend.
- Backend.
- Vision Engine.
- Redis.
- PostgreSQL.
- Relaciones entre ellos.

Este diagrama representa la arquitectura estructural de v1.

---

### 2.3 dataflow-streams.puml

Describe el flujo de datos desde:

Fuente de video
→ Vision Engine
→ Redis Streams
→ Backend
→ Base de datos
→ Frontend

Enfatiza el desacoplamiento y la mensajería.

---

### 2.4 sequence-frame-ingest.puml

Describe la secuencia dinámica del procesamiento:

- Inicio de sesión.
- Captura de frame.
- Inferencia.
- Publicación de evento.
- Consumo y agregación.
- Actualización en tiempo real.
- Finalización de sesión.

---

## 3. Cómo visualizar los diagramas

Opciones:

- Extensión de PlantUML en el editor.
- Generación local mediante CLI.
- Visualización en herramientas compatibles.

Los archivos `.puml` pueden exportarse a:

- PNG
- SVG
- PDF

---

## 4. Convenciones utilizadas

- Rectángulos: contenedores o componentes.
- Cola: sistema de mensajería.
- Base de datos: persistencia.
- Flechas sólidas: flujo de datos.
- Flechas secuenciales: interacción temporal.

---

## 5. Mantenimiento

Reglas:

- Mantener consistencia con la documentación.
- Actualizar diagramas cuando cambie arquitectura.
- No modificar versión v1 una vez estabilizada.
- Agregar nuevos diagramas si se amplía el sistema.
