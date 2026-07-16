# 10 – Despliegue

## 1. Introducción

Este documento describe la estrategia de despliegue para la versión v1 del sistema.

El objetivo es:

- Mantener simplicidad operativa.
- Garantizar reproducibilidad.
- Facilitar ejecución local y en la nube.
- Evitar sobreingeniería en infraestructura.

---

## 2. Entornos

Se contemplan dos entornos principales:

### 2.1 Entorno local

- Uso de Docker.
- Orquestación mediante Docker Compose.
- Ejecución en CPU por defecto.
- Configuración mediante variables de entorno.

Propósito:

- Desarrollo.
- Pruebas.
- Debugging.
- Validación funcional.

---

### 2.2 Entorno cloud (v1)

- Instancia única en proveedor cloud.
- Despliegue mediante Docker.
- Posibilidad de usar instancia con GPU opcional.
- Base de datos ejecutándose en contenedor o servicio gestionado simple.

Propósito:

- Demo pública.
- Prueba de despliegue real.
- Validación de arquitectura en entorno externo.

---

## 3. Componentes desplegados

En v1 se desplegarán los siguientes contenedores:

1. Vision Engine
2. Backend API
3. Redis
4. PostgreSQL
5. Frontend
6. Proxy web (si aplica)

Cada componente se ejecuta en contenedor independiente.

---

## 4. Estrategia de contenedores

Cada servicio tendrá:

- Dockerfile propio.
- Variables de entorno configurables.
- Red interna compartida.
- Dependencias explícitas en docker-compose.

No se utilizará:

- Orquestador de contenedores avanzado.
- Autoescalado automático.
- Service mesh.

---

## 5. Configuración

La configuración se realizará mediante:

- Variables de entorno.
- Archivos `.env` separados por entorno.
- Parámetros configurables para:

  - Fuente de video.
  - Modo de ejecución (cpu/gpu).
  - Credenciales de base de datos.
  - Configuración de Redis.

No se almacenarán secretos en código.

---

## 6. Flujo de despliegue local

1. Clonar repositorio.
2. Configurar variables de entorno.
3. Ejecutar `docker compose up`.
4. Verificar:

   - Backend accesible.
   - Frontend cargando.
   - Redis operativo.
   - Base de datos inicializada.
5. Iniciar sesión de análisis desde frontend o API.

---

## 7. Flujo de despliegue en cloud

1. Provisionar instancia.
2. Instalar Docker.
3. Clonar repositorio o subir imágenes.
4. Configurar variables de entorno.
5. Ejecutar contenedores.
6. Configurar acceso público (puertos o proxy).
7. Validar conectividad externa.

---

## 8. Soporte CPU / GPU

El Vision Engine soporta ambos modos.

Modo CPU:

- Ejecutable en cualquier instancia estándar.

Modo GPU:

- Requiere instancia compatible.
- Drivers y runtime configurados.

La arquitectura no cambia entre modos.

---

## 9. Exposición de servicios

Exposición pública mínima:

- Frontend (HTTP/HTTPS).
- Backend API (opcionalmente expuesto).
- WebSocket integrado en backend.

Servicios internos no expuestos:

- Redis.
- Base de datos.

Se recomienda red interna aislada.

---

## 10. Logs y monitoreo básico

En v1:

- Logs accesibles vía contenedor.
- Sin integración con sistema externo de monitoreo.
- Diagnóstico manual.

La observabilidad avanzada se considera futura mejora.

---

## 11. Escalabilidad futura

La arquitectura permite:

- Separar Vision Engine en múltiples instancias.
- Migrar base de datos a servicio gestionado.
- Introducir balanceador.
- Adoptar orquestador en versiones posteriores.

Estas mejoras no forman parte de v1.

---

## 12. Restricciones

En v1 no se implementará:

- Alta disponibilidad multi-zona.
- Rolling updates automatizados.
- Pipeline CI/CD completo.
- Gestión automatizada de infraestructura.
